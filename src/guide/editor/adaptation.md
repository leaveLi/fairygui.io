---
title: 自适应
type: guide_editor
order: 220
---

FairyGUI为手游开发提供了自动适应各种设备分辨率的UI适配策略，这意味着开发者只需要制作一套UI，就可以适配所有分辨率的设备。

## 设计分辨率与设备分辨率

通常我们会选择一个固定的分辨率进行UI设计和制作，这个分辨率称为设计分辨率。例如1136×640,1280×720都是比较常用的设计分辨率。选定一个设计分辨率后，最大的UI界面（通常就是全屏界面）的大小就限制在这个分辨率。
运行时设备的分辨率称为设备分辨率，这个分辨率与设计分辨率不一定一致，需要将UI界面经过一定缩放投射到屏幕上。

## 全局缩放

Egret/Laya/Cocos2dx/CocosCreator不使用FairyGUI提供的全局缩放，需要使用他们自带的全局缩放策略，请忽略本节内容。参考资料：
- [Egret](http://developer.egret.com/cn/2d/screenAdaptation/explanation) 
- [LayaAir](https://ldc.layabox.com/doc/?nav=zh-as-1-8-0)
- [CocosCreator](https://docs.cocos.com/creator/manual/zh/ui/multi-resolution.html)

当设计分辨率和设备分辨率不一致时，首先进行的是全局缩放。这个全局缩放对UI内部是透明的，也就是说，所有UI界面都不需要理会这个缩放的存在。例如如果整体放大两倍，一个窗口的大小为400×400像素，那么最终显示到屏幕的窗口大小将为800×800像素，但如果你读取这个窗口的width和height，仍然将返回400×400，内部的所有坐标也不会发生变化。

设置全局缩放的方式是：

```csharp
    GRoot.inst.SetContentScaleFactor(1136，640，ScreenMatchMode.MatchWidthOrHeight);
```

这里1136和640是设计分辨率的宽度和高度。ScreenMatchMode定义了适配模式，可用的常量有：

- `MatchWidthOrHeight` 取宽和高比例较小的进行缩放。例如，设计分辨率是960x640，设备分辨率是1280×720，那么可以得到宽边的比例是1280/960=1.33，高边的比例是720/640=1.125，最后取较小的1.125作为全局缩放系数。这种缩放方式保证内容缩放后始终在屏幕内，可能会留边，但不会超出屏幕看不到。

- `MatchWidth` 固定取宽的比例进行缩放。高边可能会超出屏幕。

- `MatchHeight` 固定取高的比例进行缩放。宽边可能会超出屏幕。

在Unity版本里，除了使用API设置全局缩放外，还提供了一个Unity组件：UIContentScaler。在启动场景里任何一个GameObject挂上UIContentScaler组件即可。并不需要每个场景都挂。参考[这里](#../unity/index.html#UIContentScaler)。

## 逻辑屏幕

全局缩放后的屏幕大小就是逻辑屏幕大小，在上例中，设备分辨率是1280×720，全局缩放系数是1.125，那么逻辑屏幕大小就是（1280/1.125=1138, 720/1.125=640）= 1138x640。GRoot这个UI根组件的大小始终占满逻辑屏幕，也就是说，逻辑屏幕的大小可以通过GRoot.inst.width和GRoot.inst.height获得。

## 全屏界面适配

全局缩放后，大部分UI都不需要做任何调整，只有一个例外，就是设计为全屏的界面。在上例中，在设计分辨率下，全屏界面的大小是960x640，我们也是按这个大小设计全屏组件的。全局缩放后，这时逻辑屏幕的大小变成1138x640了，那大小就不一致了。这时我们需要重新调整组件大小使之满屏。

如果你使用的是UIPanel，那么在Inspector上设置Fit Screen为Fit Size就可以了；如果是动态创建的UI，那么可以使用API：

```csharp
    //设置组件全屏，即大小和逻辑屏幕大小一致。
    //组件的内部应该做好关联处理， 以应对大小改变。
    aComponent.SetSize(GRoot.inst.width, GRoot.inst.height);
    //或者更简洁的方式
    aComponnet.MakeFullScreen();
```

一般来说，手游在进入游戏后，屏幕大小就不会变化的。但如果你开发的是桌面游戏，又或者支持横竖屏切换的游戏，即屏幕大小是会变化的，那么全屏界面还需要加上对屏幕大小的约束，即

```csharp
   aComponent.AddRelation(GRoot.inst, RelationType.Size);
```

当然，这仅仅是处理全屏界面的一种方式。在有的情况下，例如如果选用“MatchHeight”模式，也就是高度优先的适配方法，这种方法保证了UI界面垂直方向的内容总是铺满，而水平方向就有可能超出屏幕。这种适配方式需要设计师有“安全区域”的设计思维，不能安排内容在超出屏幕的部分。例如，将全屏界面居中，牺牲掉两边的内容：

```csharp
    aComponent.x = (GRoot.inst.width – aComponent.width)/2;
```

这样，左边缘和右边缘将会被屏幕边缘裁剪掉，这要求设计师在设计时就考虑到这种情况。

## 自动调整UI布局

全屏组件在适配过程中需要重新设置全屏，那么组件大小就会发生变化，这时需要使用关联系统让组件内的元素自动布局在正确的位置。实例可以参考[关联系统](relation.html#实例解析)。