---
title: SDK更新日志
type: product
order: 2
---

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