---
title: 升级到二进制包格式
type: guide_editor
order: 230
---

编辑器从3.9.0版本起，支持了新的发布格式，即二进制格式（下面简称新格式）。和以往使用的XML格式（下面简称旧格式）不同，二进制格式更加紧凑，解析时更快，而且占用的内存和产生的内存垃圾更少。
**(注意：只是指发布后的格式，编辑器里的文件格式不会有任何改变）**

新格式对比旧格式，在不同场合下可以获得的提升为：
- `AddPackage`： GC最大可降低90% CPU耗时最大可降低80%
- `CreateObject`：GC最大可降低约15% CPU耗时最大可降低20%

目前Unity、Laya和Egret均已经支持新格式，Cocos2dx和CryEngine会稍后提供，AS3和Starling版本则不会提供新格式的支持。
使用新格式需要更新到GitHub里最新的源码（Unity）或库（Laya/Egret)。编辑器更新到3.9.0版本后，在发布对话框里的全局设置里，勾选“使用二进制格式”。

**文件命名**

新格式的文件命名与旧格式不同。旧格式的文件名中有使用“@”作为连接字符，由于有不少用户反映这个字符在某些场合下不允许使用，所以新格式改为使用“_”字符。在旧格式中，Unity的描述文件名为“xxx.bytes",新格式中，更改为“xxx_fui.bytes"，这是为了可以更快的识别出包文件。新格式只有一个描述文件和一个或多个贴图和声音文件，不会再生成单独的sprites文件。

支持新格式的SDK不再支持读取旧格式的文件，同样，旧的SDK也无法识别新格式的文件，无法无缝切换，你需要做出抉择。

**UI包管理**

使用新格式后，UI包的管理策略有改变，主要为：
1. 现在AddPackage时不会像以前那样把全部资源（贴图、声音）一次性载入，而是用到再载入。如果你需要全部载入，调用`UIPackage.LoadAllAssets`。

2. 如果UIPackage是从AssetBundle中载入的，在RemovePackage时AssetBundle才会被Unload(true)。如果你确认所有资源都已经载入了（例如调用了LoadAllAssets），也可以自行卸载AssetBundle。

3. 调用`UIPackage.UnloadAssets`可以只释放UI包的资源，而不移除包，也不需要卸载从UI包中创建的UI界面（这些界面你仍然可以调用显示，不会报错，但图片显示空白）。当包需要恢复使用时，调用`UIPackage.ReloadAssets`恢复资源，那些从这个UI包创建的UI界面能够自动恢复正常显示。如果包是从AssetBundle载入的，那么在UnloadAssets时AssetBundle会被Unload(true)，所以在ReloadAssets时，必须调用ReloadAssets一个带参数的重载并提供新的AssetBundle实例。

4. Egret版本目前不支持Unload和Reload；Laya版本支持Unload，但不需要调用Reload，贴图资源一旦用到就会自动Reload。

5. 使用新格式后，UI包描述部分占用的内存急剧减少（只有几十KB~几百KB的规模），所以可以不用再管理包的装载卸载，而直接让UIPackage都常驻，再配合使用UnloadAssets和ReloadAssets就可以达到控制内存的效果了（当然这只是建议）。

6. 在Unity编辑器中运行时，即使UI包没有放置在Resources下，现在也能够正常加载，路径用全路径即可，当然，打包APP后是不可能加载的，你需要用宏做区别。例如：
  ```csharp
    //编辑器环境下直接从文件路径中加载，方便测试，正式环境则使用AssetBundle
    #if UNITY_EDITOR
        UIPackage.AddPackage("Assets/SomePath/Package1");
    #else
        UIPackage.AddPackage(ab);
    #endif
  ```
  同样，UIPanel也可以选择任意路径下的包，编辑器下都能够正常显示。运行时则需要保证包在UIPanel激活前加载好。

**另外的一些接口改变**

1. [Unity] UIConfig.buttonSound原来接受一个AudioClip对象，现在需要用NAudioClip，可以用直接用new NAudioClip(audioClip)包装。UIPacakge.GetItemAsset(声音item）现在返回的也是NAudioClip对象。

2. [Unity] UIPackage.AddPackage的接受AssetBundle参数的重载，不再提供unloadBundleAfterLoaded参数。AssetBundle也不会在AddPackage后立刻卸载。

3. 获取编辑器内在组件设计期设置的自定义数据，需要使用新加的API：GComponent.baseUserData；获取编辑器内对元件定义的自定义数据的方法则不变，仍然是GObject.data。
