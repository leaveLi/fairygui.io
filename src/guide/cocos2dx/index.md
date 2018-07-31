---
title: 在Cocos2dx中使用FairyGUI
type: guide_cocos2dx
order: 0
---

## 运行Demo

1. 从GitHub Clone或者直接下载FairyGUI-cocos2dx。
2. 下载3.x版本cocos2dx命名成cocos2d放在Example根目录。cocos2dx源码需要改动一处地方才能通过编译，打开2d/CCLabel.h，大约在672行，为updateBMFontScale函数打上virtual修饰符。即

  ```
    virtual void updateBMFontScale();
  ```

3. 使用Visual Studio打开Examples/proj.win32/Examples.sln，然后直接运行Demo。
4. Demo的UI工程在Examples/UIProject下。结合UI工程和例子代码可以了解到FairyGUI在Cocos2dx下的使用方法。

## 加入FairyGUI到你的项目

将libfairygui加入到你的WorkSpace里，然后添加引用即可。libfairygui是一个静态库，最后链接到你的程序里。

## 关于GRoot

GRoot是UI的根对象，我们可以像Demo那样，每个场景都创建一个GRoot对象。GRoot::getInstance()（或者简单的使用UIRoot这个宏）指向的是最后创建的GRoot对象，一般来说就是当前Scene包含的GRoot对象。Cocos2dx在切换Scene时Scene里的一切东西都会销毁，也包括加入到Scene里的GRoot。如果你想避免界面的重复创建，一个GRoot在不同Scene里公用，那么可以自己对GRoot retain。切换场景后自行add GRoot到Scene即可。

## 事件机制

与事件相关的API有

```csharp
    void addEventListener(int eventType, const EventCallback& callback);
    void addEventListener(int eventType, const EventCallback& callback, const EventTag& tag);
    void removeEventListener(int eventType);
    void removeEventListener(int eventType, const EventTag& tag);
    void removeEventListeners();
```

`eventType` 一个事件类型常量，定义在event/UIEventType.h里。
`callback` 回调函数，可以用CC_CALLBACK_1宏来定义，也可以用Lambda表达式。
`tag` 如果事件注册后要进行remove操作，那么add的时候必须标识这个事件。这里的机制是调用者提供一个EventTag。EventTag可以用整形，或者一个指针地址来构造，例如可以直接传递this。特别地，0表示没有EventTag，也就是不要使用0作为特殊的EventTag。

## Lua Binding
FairyGUI-cocos2dx只提供了C++版本，Lua Binding需要由你自行完成。网上有很多方案可以完成这点。例如这位热心网友的分享：[https://www.jianshu.com/p/547e584e05d8](https://www.jianshu.com/p/547e584e05d8 "FairyGUI在Cocos2d-x下的多平台接入和lua绑定")
