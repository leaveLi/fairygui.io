---
title: Cry-坐标系统
type: guide_cry
order: 5
---

GObject里的x/y/position值都是**局部坐标**，也就是相对于父元件的偏移。GObject没有提供直接的属性获得对象的全局坐标，但提供了方法进行转换。

如果要获得任意一个UI元件在屏幕上的坐标，可以用：

```csharp
    Vector2 screenPos = aObject.LocalToGlobal(Vector2.zero);
```

（注意，这里说的屏幕，是指FairyGUI语义中的屏幕，是以屏幕左上角为原点的，不是指Unity语义中的屏幕）

相反，如果要获取屏幕坐标在UI元件上的局部坐标，可以用：

```csharp
    Vector2 localPos = aObject.GlobalToLocal(screenPos);
```

如果有UI适配导致的全局缩放，那么逻辑屏幕大小和物理屏幕大小不一致，逻辑屏幕的坐标就是GRoot里的坐标。如果要进行局部坐标与逻辑屏幕坐标的转换，可以用：

```csharp
    //物理屏幕坐标转换为逻辑屏幕坐标
    Vector2 logicScreenPos = GRoot.inst.GlobalToLocal(screenPos);
    
    //UI元件坐标与逻辑屏幕坐标之间的转换
    aObject.LocalToRoot(pos);
    aObject.RootToLocal(pos);
```

**注意，我们在编辑器里定义的，代码里处理的，一般就是指这个逻辑屏幕坐标。**

如果要转换任意两个UI对象间的坐标，例如需要知道A里面的坐标(10,10)在B里面的位置，可以用：

```csharp
    Vector2 posInB = aObject.TransformPoint(bObject, new Vector2(10,10));
```
