---
title: SDK更新日志
type: product
order: 2
---

## 2018/11/19
1. [Unity] Unity发布了3.2.0版本。
2. [Unity] 增加了一个Unity组件DisplayObjectInfo，可以在Inspector下查看和修改GameObject对应的UI节点的信息。支持了网易的UI自动化测试工具[AirTest](https://github.com/AirtestProject/Poco-SDK/tree/master/Unity3D)。但默认不挂这个新组件，要启用这个功能，在Scripting Define Symbols里添加**FAIRYGUI_TEST**。
3. [Laya] 现在已支持LayaAir2.0，要注意master中的代码是支持LayaAir2.0的，分支layaair1.x才是支持LayaAir1.x的。这两个分支目前都保持维护状态，直到LayaAir2.0完全稳定。
4. [Laya] 现在支持图片的填充方式。
5. [Laya] 现在支持反向遮罩（挖洞）。
6. [All] 修正了混合不同资源ITEM的虚拟列表在滚动过程中可能会出现ITEM的深度不正确的bug。
7. [All] 修正了GList.scrollToView在横向流动或者竖向流动布局中滚动位置不正确的bug。
8. [MonoGame] 新增了MonoGame SDK。

## 2018/10/30
Unity发布了3.1.0版本，以下是更新内容
1. 改进了文字描边由原来的4次叠加增加到8次叠加，效果更好。但可以通过设置UIConfig.enhancedTextOutlineEffect=false强制为原来的4次叠加效果。
2. 优化了Update里的逻辑，CPU消耗最大可减少30%。
3. GoWrapper现在支持一个网格渲染器内多个材质。
4. 手机上输入文本激活时，调整光标到最后。
5. 消除了ColorFilter调整参数时产生的GC。
6. 修正了设置GLoader.texture=null时显示不正确的bug。
7. 调整着色器，修正了在Unity5.4版本错误的激活了线性空间颜色相关逻辑的bug。
8. 修正了PC上无法粘贴多行文本到输入框的bug。
9. 修正了RemovePackage里有关资源释放的一个bug。
10. 修正API titleFontSize实现的bug。
11. 修正了GTween在duration=0时工作不正确的bug。

## 2018/9/7
1. [Cocos2dx/Cry] 新增支持二进制的包格式。。[详情](../guide/editor/upgrade_binary_format.html)
2. [Unity] Unity发布了3.0.0正式版本，提供二进制格式的支持。

## 2018/8/27
1. [Egret/Laya/Unity] 新增支持二进制的包格式。。[详情](../guide/editor/upgrade_binary_format.html)
2. [Egret] 修正了MovieClip在egret下发布为native后不显示的bug。
3. [Laya] 修正了绘制圆角矩形不正确的bug。
4. [Laya] 修正了GLoader在加载外部资源时没有指定为图片格式导致不能加载后缀不带png或jpg的网络图片的bug。
5. [Unity] Unity发布了2.4.0版本，主要是使用了新的Tween库，以及从2.3版本后的bug修正。升级说明：
使用了新的Tween库后，原来GObject.TweenXXX方法返回的不再是DOTween的Tweener对象，而是GTweener对象。如果你有使用TweenXXX系列函数，并且有使用返回的对象，那么需要小心的处理，特别是在Lua中调用。GTweener与DOTween.Tweener的常用API基本一致，回调注册函数也一致，重要差异在于SetEase方法，即GTweener.SetEase。建议全局搜索SetEase，参数修改为FairyGUI的EaseType.XXXX。

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
6. [Unity] Unity发布了2.3.0版本。
7. [Unity] 修正了文本变灰颜色不正确的问题(注意是着色器的修改）。
8. [Unity] 修正了UIPanel设置了锚点，且Hit Test设置为Raycast后，碰撞体位置不正确的bug。

## 2018/5/23
1. [All] 修正了当元件有设置轴心或锚点，部分关联关系工作不正常的问题。
2. [All] 修正了进度条max设置为0时，进度显示不正确的bug。
3. [Laya] 增加了UIConfig.packageFileExtension，配合编辑器改发布文件扩展名的功能（小程序需要）。
4. [Cocos2dx] Cocos2dx发布了1.3版本。
5. [Cocos2dx] 修正了一个致命的引用计数问题。
6. [Cocos2dx] 修正了像素点击测试不准确的问题。
7. [Unity] Unity发布了2.2.0版本。
8. [Unity] 增加了对线性颜色空间的支持。
9. [Unity] 消除了动效播放过程中可能产生的GC。
10. [Unity] 修正了Blend Mode为Multiply或者Screen时，显示不正确的bug。
11. [Unity] 修正了UIPackage里延时卸载AB的bug。
12. [Unity] 修正了滚动容器的虚化边缘没有在边缘回弹时显示的bug。
13. [Unity] 修正了显示对象修改visible属性时，可能出现fairyBatching不正确的bug。
14. [Unity] 修正了当文本宽度为0，仍然显示1个字符的bug。

## 2018/3/30
1. [All] 增加了对GLoader新填充类型的支持。
2. [All] 修改了虚拟列表滚动时在某些特殊情况下不再响应滚动的bug。
3. [Unity] Unity发布了2.1.0版本。
4. [Unity] 现在支持图形中的圆角。
5. [Unity] 修正了共享材质系统中的一个bug。

## 2018/3/19
1. [All] GRoot.ShowPopup里现在会自动检查sortingOrder，保证popup的东西一定在目标前面。
2. [Unity] Unity发布了2.0.0版本。
3. [Unity] 增加了对阿拉伯语言显示的支持。如果需要打开此功能，使用源码版本的，需要在Unity Player Settings的Scripting Define Symbols里增加RTL_TEXT_SUPPORT；使用DLL版本的，请自行编译包含这个功能的DLL，或者向谷主索取。
4. [Unity] 增加了UIConfig.depthSupportForPaitingMode.如果你要对使用了自定义遮罩的组件进行设置倾斜、设置BlendMode，设置滤镜，又或者曲面UI中含有自定义遮罩的组件时，需要设置这个开关为true才能显示正常。
5. [Unity] 修正了轴心不为中心或左上角时，倾斜显示不正确的bug。

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

1. [Unity] Unity发布了1.9.3版本。
2. [Unity] 如果UIPackage.AddPackage方法用于AssetBundle，现在增加了一个参数unloadBundleAfterLoaded，可以控制是否由FairyGUI释放AB。默认是true（和旧版本行为一致）。
3. [Unity] 修正了触摸输入处理的一个bug。
4. [Unity] 修正了1.9.2办法引入的一个bug，可能导致窗口无法拖动。
5. [Cocos2dx] Cocos2dx发布了1.1版本。
6. [Cocos2dx] 修正了loadComponentChildren的bug。
7. [Cocos2dx] 修正了析构函数没有加virtual修饰符的问题。
8. [Cocos2dx] 修正了剪裁容器外面的元素也能响应点击的bug。

## 2017/11/28

1. [All] GObject.visible修改了底层的实现方式，以前visible=false时是将原生对象移出显示列表的，现在改成不移出，仅仅是设置原生对象的visible=false。
2. [Laya] 更新了例子，使用LayaAir 1.7.12。
3. [Egret] 更新了例子，使用EgretEngine 5.0.13。
4. [Laya] 修正了Base64编码的bug。（影响像素点击测试功能）
5. [Cocos2dx] 正式版发布。
6. [Unity] Unity发布了1.9.2版本。
7. [Unity] 修正了如果UIPanel中有使用倾斜功能。则在编辑模式会报错的bug。
8. [Unity] 修正了如果位图字体有重名时报错的bug。
9. [Unity] 增加了onTouchMove事件。调用context.CaptureTouch后，对象可以收到这个事件（无论鼠标是否在对象上），直到touchEnd。
10. [Unity] 改进了GoWrapper，现在会复制对象的材质，避免造成原始材质的变化。
11. [Unity] 修正了有时当对象销毁后，自动合批出现不正确的bug。
12. [Unity] 增加了对HTML语法的支持，支持设置图片的宽度或高度为百分比，例如width='50%' height='50%'。
13. [Unity] 增加了GTextInput.SetSelection，可以选定输入框中的文字。
14. [Unity] 修正了滚动容器关闭滚动惯性后，item贴近或分页滚动没有正确处理的bug。

## 2017/9/24

1. [All] 修正了虚拟列表滚动后丢失ITEM选中状态的问题。
2. [All] 修正了滚动容器的一个bug，对于设置了贴近ITEM的滚动容器，当滚动时，如果点击停止，停止后没有回到贴近ITEM的正确位置。
3. [Flash/Starling/Egret] 增加了MovieClip的平滑设置支持。
4. [Egret] 修正了富文本点击链接触发两次的问题。
5. [Unity] Unity发布了1.9.0版本。
6. [Unity] 移动平台现在也支持RollOver和RollOut事件了。
7. [Unity] GoWrapper现在可以包装UGUI的Canvas，实现FairyGUI中嵌入UGUI的需求。[使用方法](../guide/unity/insert3d.html#插入Canvas)
8. [Unity] 如果手势是全屏的，也就是没有具体对象的，现在可以直接建立在GRoot上。[使用方法](../guide/unity/input.html#手势)
9. [Unity] 修正了部分输入法，以及在Mac下无法输入中文的bug。
10. [Unity] 修正了字体贴图重建时，没有销毁材质，可能造成内存泄漏的bug。
11. [Unity] GoWapper增加了对sharedMaterial是否为空的判断。
12. [Unity] 如果使用源码版本，不再为移动平台屏蔽OnGUI函数，这会造成不响应按键事件。
13. [Unity] Stage.touchScreen不再只读，现在可以手工改变。
14. [Unity] 对于设置为使用Raycast进行点击测试的UIPanel，现在你可以使用HitTestContext.layerMask排除掉一些不关心的层。