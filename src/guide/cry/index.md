---
title: 在Cry Engine中使用FairyGUI
type: guide_cry
order: 0
---

## 加载UI包

将打包后的文件直接发布到Cry项目的Assets目录或者其子目录下，

```csharp
    //demo就是发布时填写的文件名
    UIPackage.AddPackage("demo");
    
    //如果在子目录下
    UIPackage.AddPackage("路径/demo");
```
## 卸载UI包

当一个包不再使用时，可以将其卸载。

```csharp
    //这里可以使用包的id，包的名称，包的路径，均可以
    UIPackage.RemovePackage("package");
```

包卸载后，所有包里包含的贴图等资源均会被卸载，由包里创建出来的组件也无法显示正常（虽然不会报错），所以这些组件应该（或已经）被销毁。
一般不建议包进行频繁装载卸载，因为每次装载卸载必然是要消耗CPU时间（意味着耗电）和产生大量GC的。UI系统占用的内存是可以精确估算的，你可以按照包的使用频率设定哪些包是常驻内存的（建议尽量多）。

## 创建UI

```csharp
    GComponent view = UIPackage.CreateObject(“包名”, “组件名”).asCom;
    
    //以下几种方式都可以将view显示出来：
    
    //1，直接加到GRoot显示出来
    GRoot.inst.AddChild(view);
    
    //2，使用窗口方式显示
    aWindow.contentPane = view;
    aWindow.Show();
    
    //3，加到其他组件里
    aComponnent.AddChild(view);
```

如果界面内容过多，创建时可能引起卡顿，FairyGUI提供了异步创建UI的方式，异步创建方式下，每帧消耗的CPU资源将受到控制，但创建时间也会比同步创建稍久一点。例如：

```csharp
    UIPackage.CreateObjectAsync("包名","组件名", MyCreateObjectCallback);

    void MyCreateObjectCallback(GObject obj)
    {
    }
```

动态创建的界面不会自动销毁，例如一个背包窗口，你并不需要在每次过场景都销毁。如果要销毁界面，需要手工调用Dispose方法，例如

```csharp
    view.Dispose();
```
