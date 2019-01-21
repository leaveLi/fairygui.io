---
title: 其它SDK更新日志
type: product
order: 3
---

## 2018/11/19
1. [Laya] 现在已支持LayaAir2.0，要注意master中的代码是支持LayaAir2.0的，分支layaair1.x才是支持LayaAir1.x的。这两个分支目前都保持维护状态，直到LayaAir2.0完全稳定。
2. [Laya] 现在支持图片的填充方式。
3. [Laya] 现在支持反向遮罩（挖洞）。
4. [All] 修正了混合不同资源ITEM的虚拟列表在滚动过程中可能会出现ITEM的深度不正确的bug。
5. [All] 修正了GList.scrollToView在横向流动或者竖向流动布局中滚动位置不正确的bug。
6. [MonoGame] 新增了MonoGame SDK。

## 2018/9/7
1. [Cocos2dx/Cry] 新增支持二进制的包格式。。[详情](../guide/editor/upgrade_binary_format.html)

## 2018/8/27
1. [Egret/Laya/Unity] 新增支持二进制的包格式。。[详情](../guide/editor/upgrade_binary_format.html)
2. [Egret] 修正了MovieClip在egret下发布为native后不显示的bug。
3. [Laya] 修正了绘制圆角矩形不正确的bug。
4. [Laya] 修正了GLoader在加载外部资源时没有指定为图片格式导致不能加载后缀不带png或jpg的网络图片的bug。

## 2018/8/1
1. [All] 除C++版本，其他平台增加了内置的Tween库，这意味着Unity不再依赖DOTween，AS3/Starling版本不再依赖greensock, Egret/Laya不再依赖它们自身的Tween库。当然，你仍然可以使用这些Tween库，并且建议使用在游戏开发中使用它们，因为FairyGUI内置的Tween库重点在满足FairyGUI的内部需求，功能比较简洁，未必能符合你的项目需求。
2. [All] 动效现在可以暂停：Transition.setPaused(true);
3. [All] 动效现在可以播放其中一部分，在Play方法中传入startTime和endTime参数，单位为秒。可以通过GetLabelTime的方式获得编辑器中某个关键帧的时间。
4. [All] 由于使用了内置的Tween库，现在在游戏暂停再返回后，动效播放不会再出现混乱的情况。

## 2018/7/5
1. [All] 装载器现在可以载入组件。
2. [All] 文本增加了模板功能。
2. [All] 修正了GButton.FireClick里的bug。
3. [All] 当滚动容器的分页控制器页面改变时，现在滚动容器的翻页带有过渡效果。
4. [Cocos2dx] Cocos2dx发布了1.4.0版本。
5. [Cocos2dx] 修正了编辑器内位图字体设置默认占位不生效的bug。
6. [Cocos2dx] 修正了按钮按下变暗没有生效的问题。

## 2018/5/23
1. [All] 修正了当元件有设置轴心或锚点，部分关联关系工作不正常的问题。
2. [All] 修正了进度条max设置为0时，进度显示不正确的bug。
3. [Laya] 增加了UIConfig.packageFileExtension，配合编辑器改发布文件扩展名的功能（小程序需要）。
4. [Cocos2dx] Cocos2dx发布了1.3版本。
5. [Cocos2dx] 修正了一个致命的引用计数问题。
6. [Cocos2dx] 修正了像素点击测试不准确的问题。

## 2018/3/30
1. [All] 增加了对GLoader新填充类型的支持。
2. [All] 修改了虚拟列表滚动时在某些特殊情况下不再响应滚动的bug。
3. [Unity] Unity发布了2.1.0版本。

## 2018/3/19
1. [All] GRoot.ShowPopup里现在会自动检查sortingOrder，保证popup的东西一定在目标前面。

## 2018/3/5
1. [Laya/Egret/Starling/Flash] 重构ScrollPane，支持了Header和Footer，也就是下拉刷新和上拉刷新功能的支持。（其他SDK早已支持，这次是对这几个SDK的填坑。）。功能演示可以在产品页面下载最新的Demo，里面包含了PullToRefresh这个Demo。
2. [Laya] 适配最新引擎1.7.16。
2. [Egret] 适配最新引擎5.1.5。

## 2018/1/4
1. [All] 增加了对编辑器自定义数据的支持。
2. [Egret/Laya] UIPackage.addPackage增加了一个可选参数，现在你可以直接传入描述文件数据。
3. [Egret/Laya] 增加了对编辑器输出非压缩格式的支持。
4. [Egret] 适配最新引擎5.1.2。

## 2017/12/15
1. [Cocos2dx] Cocos2dx发布了1.1版本。
2. [Cocos2dx] 修正了loadComponentChildren的bug。
3. [Cocos2dx] 修正了析构函数没有加virtual修饰符的问题。
4. [Cocos2dx] 修正了剪裁容器外面的元素也能响应点击的bug。

## 2017/11/28

1. [All] GObject.visible修改了底层的实现方式，以前visible=false时是将原生对象移出显示列表的，现在改成不移出，仅仅是设置原生对象的visible=false。
2. [Laya] 更新了例子，使用LayaAir 1.7.12。
3. [Egret] 更新了例子，使用EgretEngine 5.0.13。
4. [Laya] 修正了Base64编码的bug。（影响像素点击测试功能）
5. [Cocos2dx] 正式版发布。

## 2017/9/24

1. [All] 修正了虚拟列表滚动后丢失ITEM选中状态的问题。
2. [All] 修正了滚动容器的一个bug，对于设置了贴近ITEM的滚动容器，当滚动时，如果点击停止，停止后没有回到贴近ITEM的正确位置。
3. [Flash/Starling/Egret] 增加了MovieClip的平滑设置支持。
4. [Egret] 修正了富文本点击链接触发两次的问题。