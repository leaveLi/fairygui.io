---
title: 插入模型/粒子/Spine/Canvas
type: guide_unity
order: 50
---

FairyGUI提供了非常完整的解决方案解决模型、粒子、骨骼动画等3D对象与UI穿插的问题，并且支持与UGUI Canvas穿插。

## 直接放入3D对象

这种方式是直接用UI相机渲染3D对象，相比RenderTexture的方案，使用简单且节省内存，缺点是在UI相机下3D对象没有透视。

在UI中插入3D对象需要用到一个图形占位和`GoWrapper`对象。

1. 实例化你的3D对象，例如:

  ```csharp
    Object prefab = Resources.Load("Role/npc");
    GameObject go = (GameObject)Object.Instantiate(prefab);
  ```
2. 给3D对象设置合适的位置、缩放和旋转。

  ```csharp
    go.transform.localPosition = new Vector3(61, -89, 1000); 
    go.transform.localScale = new Vector3(180, 180, 180);
    go.transform.localEulerAngles = new Vector3(0, 100, 0);
  ```

  注意：对于模型这种有“厚度”的对象（在z轴有一定范围），localPosition的z值不应该为0，可以设置一个较大的正数值（正数表示远离摄像机）。因为Shader是开启了ZTest的，如果模型在z轴的坐标为0，和UI的z值相同，那么他的前端就可能与他上一层的UI重叠。

  模型的缩放可以根据这个公式估算：缩放倍数 = 显示大小（单位像素）/ 模型大小（单位米）。例如如果一个模型是1米高，最终需要显示400像素高，那么需要放大400倍。

2. 在UI中放置一个空白的图形，假设名称为“holder”。

  ```csharp
    GGraph holder = view.GetChild("holder").asGraph;
  ```

3. 构建GoWrapper对象，放入到holder中。

  ```csharp
    GoWrapper wrapper = new GoWrapper(go);
    holder.SetNativeObject(wrapper);
  ```

**点击处理**

GoWrapper默认是没有大小的，所有不能处理点击事件。如果需要对3D对象显示区域的点击事件，可以在holder上再放置一个图形（透明度设置为0）作为点击区域，或者一个空组件也行。

**调试方式**

使用这种方式插入3D对象，需要使用代码，并在运行时才能看到效果，对设置GameObject的位置、旋转、缩放等都不太直观。FairyGUI提供了直观的方法，可以把3D对象直接挂到UIPanel对象下。**首先设置3D对象的Layer为UI**，然后勾选UIPanel的“Set Native Children Order"。如下：

![](../../images/20170809140223.png)

但这种方法GameObject并不会随着UIPanel的移动而移动，因此只能用作调试用途。例如制作UI上用的粒子时，可以提供这种方法给美术。

**更新GameObject**

GoWrapper会在构造函数里查询你的GameObject里所有的Renderer并保存。如果你的GameObject后续发生了改变，需要告知GoWrapper重新查询和保存，否则显示不正确。

```csharp
    wrapper.CacheRenderers();
```

**更换GameObject**

如果要修改GoWrapper包装的对象，可以使用：

```csharp
    wrapper.wrapTarget = anotherGameObject;
```

设置新的包装对象后，原来的包装对象只会被删除引用，但不会被销毁。如果你要销毁原来的GameObject，必须自行处理，例如：

```csharp
    Object.Destroy(wrapper.wrapTarget);
    wrapper.wrapTarget = anotherGameObject;
```

**复制材质**

如果GoWrapper包装的对象是在很多地方同时使用，而且你在实例化时没有自行处理材质的复制，那么会产生一些问题。例如一个模型，你既在UI上显示，也在场景中使用，UI系统需要修改模型的材质参数，这就会对场景中的显示产生影响。造成这个问题的根源是GoWrapper默认是使用共享材质的，这是出于效率的影响。要解决这个问题，有两种途径。一、在你实例化对象时自行复制材质。你需要小心处理，避免过度的复制操作。二、让GoWrapper自动复制。在设置包装对象时这样调用即可：

```csharp
    //第二个参数为true，表示复制材质
    wrapper.setWrapTarget(anotherGameObject, true);
```

**剪裁**

如果需要对3D对象进行剪裁，可以利用自定义遮罩（**模型、粒子、骨骼动画等均适用**）。自定义遮罩的使用方法参考：[组件的遮罩](../editor/component.html#遮罩)。

例如需要在一个列表内的item显示模型，并希望模型在列表滚动时被正确隐藏，这时仅通过列表自身的溢出滚动功能是无法实现的。首先将列表转换为一个组件，在组件内拖入一个矩形图形覆盖列表的视口，然后将这个图形设置为组件的自定义遮罩。

模型的着色器也必须做出相应的修改，把以下代码添加到模型的着色器的Properties段（可参考FairyGUI-Image.shader）：

```csharp
    _StencilComp ("Stencil Comparison", Float) = 8
    _Stencil ("Stencil ID", Float) = 0
    _StencilOp ("Stencil Operation", Float) = 0
    _StencilWriteMask ("Stencil Write Mask", Float) = 255
    _StencilReadMask ("Stencil Read Mask", Float) = 255
```

把以下代码加到模型的着色器的SubShader段（可参考FairyGUI-Image.shader）：

```csharp
    Stencil
    {
        Ref [_Stencil]
        Comp [_StencilComp]
        Pass [_StencilOp] 
        ReadMask [_StencilReadMask]
        WriteMask [_StencilWriteMask]
    }
```

最后，设置GoWrapper支持自定义遮罩：

```csharp
    wrapper.supportStencil = true;
```

**注意，如果有多个相同的被剪裁的对象，他们的材质不能共用一个，否则会出现显示异常。解决方法是复制材质，请参考上一段，复制材质。**

## 使用RenderTexture

在UI上展现3D内容的另一种方式是使用RenderTexture。使用RenderTexture的步骤比较复杂，需要另外新建相机渲染目标对象，然后把该相机的输出定向到一张RenderTexture。有了RenderTexture后，我们将它赋值到Image.texture即可。详细的代码可以参考[RenderImage](https://github.com/fairygui/FairyGUI-unity/blob/master/Assets/Examples/RenderTexture/RenderImage.cs)。

RenderTexture可以设置背景颜色为透明，方便和UI混合，具体在例子中就是把“this._image.blendMode = BlendMode.Off;”注释掉即可。但如果渲染的内容包含有透明贴图，那么和UI混合时就会出现透明部分的显示错误，有两种解决方案，第一种方案可以参考[这里的资料](https://blog.uwa4d.com/archives/Severe_MOBA.html)，修改模型或者粒子的着色器，以及RenderTexture的着色器。 第二种方案是FairyGUI提供的独特方案，也就是像RenderImage演示的那样，将RenderTexture所在位置的背景图片影射到RenderTexture渲染相机的背景上：

```csharp
    public void SetBackground(GObject image);
    public void SetBackground(GObject image1, GObject image2);
```

可以看到，最多可以设置两个图片。如果RenderTexture的背后有超过两个图片的叠加，就无法处理了。这两个图片不需要和RenderTexture在同一个容器里，它们可以在UI的任何层次。

## 插入Canvas

FairyGUI无论从功能上还是效率上，都能满足所有大部分UI设计的需求，因此在使用FairyGUI的项目里，很少会有需要再使用UGUI的情况。大部分UGUI需要用插件完成的功能，FairyGUI均已经内置，而且很多可以在编辑器零脚本完成。如果确实需要用到一些UGUI的插件，并且不方便移植，FairyGUI也提供了方案插入UGUI的Canvas到FairyGUI的显示层次中。步骤如下：

1. 设置Canvas的Render Mode为WorldSpace，Event Camera为Stage Camera。
2. 删除Canvas Scaler组件（如果有）。
3. 使用GoWrapper包装Canvas：

  ```csharp
    GameObject canvasObject;
    GoWrapper gw = new GoWrapper(canvasObject);
  ```