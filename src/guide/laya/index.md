---
title: 在LayaAir中使用FairyGUI
type: guide_laya
order: 0
---

## 小游戏开发必读

1. 因为rawinflate这个库在小游戏平台有问题，所以直接不使用它。请使用最新编辑器，在发布对话框，全局设置里不勾选“压缩描述文件”就可以了。rawinflate这个库就不会再引用到（也不需要打包了）。
2. 小游戏不支持fui扩展名，所以在发布界面要把扩展名修改成小游戏支持的扩展名（自己查阅小游戏文档）。然后在代码里设置：
  ```
    fairygui.UIConfig.packageFileExtension = "你定义的扩展名";
  ```
3. 下载[xml解析库](http://fairygui.oss-cn-shenzhen.aliyuncs.com/js.rar)，引入方式：
  ```
    window.Parser = require("./js/dom_parser");
  ```
4. AddPackage有两种方式，一种是传统的传入文件名方式，另一种是直接传入fui整个文件的内容，也就是说不管你内容是从哪里来的。两种方式可以按需选择。
5. 如果遇到加载失败，请检查laya的加载流程。因为FairyGUI不负责加载，你需要确保资源已经顺利加载了再AddPackage。

## TS/JS版本

1. 将FairyGUI库以及依赖的rawinflate库拷贝到bin/libs目录。（如果你在编辑器发布时没有勾选`压缩描述文件`，那么这个库是不需要的）。

  ![](../../images/20170809155135.png)

2. 把fairygui.d.ts拷贝到libs目录。

  ![](../../images/20170809155742.png)

3. 在index.html里添加上述两个库的引用，注意放置的位置。

  ![](../../images/20170809160052.png)

  注：FairyGUI只依赖laya.core， laya.html两个模块，不需要laya.ui。

4. 使用FairyGUI编辑器完成UI编辑。发布目录请选择Laya工程的bin/res目录（当然其他目录也是可以的）。发布后得到两个（或以上）文件。

  ![](../../images/20170809160159.png)

5. 在程序启动时（或者在需要用到这些UI的适当地方）加载这两个文件，并完成初始化。

  ```csharp
    // 程序入口
    class GameMain {
        constructor()
        {
            Laya.init(1136, 640, Laya.WebGL);
            laya.utils.Stat.show(0, 0);
            //设置适配模式
            Laya.stage.scaleMode = "showall";
            Laya.stage.alignH = "left";
            Laya.stage.alignV = "top";
            //设置横竖屏
            Laya.stage.screenMode = "horizontal";
            
            Laya.loader.load([{ url: "res/Basic@atlas0.png", type: Loader.IMAGE },
                { url: "res/Basic.fui", type: Loader.BUFFER }
            ], Handler.create(this, this.onLoaded));
        }
       
        onLoaded(): void {
            Laya.stage.addChild(fairygui.GRoot.inst.displayObject);
            
            fairygui.UIPackage.addPackage("res/Basic");
            fairygui.UIConfig.defaultFont = "宋体";
            fairygui.UIConfig.verticalScrollBar = "ui://Basic/ScrollBar_VT";
            fairygui.UIConfig.horizontalScrollBar = "ui://Basic/ScrollBar_HZ";
            fairygui.UIConfig.popupMenu = "ui://Basic/PopupMenu";
            fairygui.UIConfig.buttonSound = "ui://Basic/click";
            
            new MainPanel();
        }
    }
  ```

## AS版本

1. 从GITHUB中拉FairyGUI layabox SDK的源代码，放到你的源码工程里。
2. 在index.html里加入rawinflate.min.js，注意要放在你的js前。（如果你在编辑器发布时没有勾选`压缩描述文件`，那么这个库是不需要的）。