---
title: Popup
type: guide_editor
order: 200
---

在UI系统中我们经常需要弹出一些组件，这些组件在用户点击空白地方的情况下就会自动消失。FairyGUI内置了这个功能。

## Popup 管理

弹出和关闭Popup的API在GRoot中提供。

- `ShowPopup` 弹出一个组件。如果指定了目标，则会调整弹出的位置到目标的下方，形成一个下拉的效果。同时提供了参数可以用来指定是向上弹出或者向下弹出。FairyGUI会根据组件的大小自动计算弹出位置，以确保组件显示不会超出屏幕。例如：

```csharp
    //弹出在当前鼠标位置
    GRoot.inst.ShowPopup(aComponent);

    //弹出在aButton的下方
    GRoot.inst.ShowPopup(aComponent, aButton);

    //弹出在自定义的位置
    GRoot.inst.ShowPopup(aComponent);
    aComponent.SetXY(100, 100);
```

窗口也可以通过ShowPopup弹出，这样弹出的窗口也具有了点击空白关闭的特性：

```csharp
    Window aWindow;
    GRoot.inst.ShowPopup(aWindow);

    //和使用aWindow.Show显示窗口的唯一区别就是多了点击空白关闭的功能，其它用法没有任何区别。
```

- `HidePopup` 默认情况下，用户点击空白地方就会自动关闭弹出的组件。也可以调用此API手工关闭。可以指定需要关闭的Popup，不指定参数时，所有当前的弹出都关闭。

点击空白处后弹出框会自动关闭，如果要获得这个关闭的通知，可以监听移出舞台的事件，例如：

```csharp
    //Unity/Cry
    aComponent.onRemoveFromStage.Add(onPopupClosed);

    //AS3
    aComponent.addEventListener(Event.REMOVED_FROM_STAGE, onPopupClosed);

    //Egret
    aComponent.addEventListener(egret.Event.REMOVED_FROM_STAGE, this.onPopupClosed, this);

    //Laya
    aComponent.on(laya.events.Event.UNDISPLAY, this, this.onPopupClosed);

    //Cocos2dx
    aComponent->addEventListener(UIEventType::Exit, CC_CALLBACK_1(AClass::onPopupClosed, this));
```

## PopupMenu

PopupMenu是FairyGUI提供的一个工具类，用于实现弹出菜单。首先需要在编辑器制作一个菜单组件，点击“资源->新建弹出菜单..."，然后根据向导完成。菜单组件里的关键元素是命名为`list`的列表组件，列表的溢出处理模式应该选择为可见，因为一般来说，菜单都是显示全部item的，不需要滚动。

菜单组件制作完成后，可以在代码里生成和调用这个菜单。

首先设置全局的菜单资源：
```csharp
    UIConfig.popupMenu = "ui://包名/菜单组件名";
    
    //如果有设计分隔条
    UIConfig.popupMenu_seperator = "ui://包名/菜单分隔条";
```

```csharp
    //如果构造函数不带参数，则表示使用UIConfig.popupMenu里定义的资源。
    //也可以带一个参数，指定这个菜单使用的菜单组件资源
    PopupMenu menu = new PopupMenu();

    //如果要修改菜单的宽度。
    menu.contentPane.width = 300;

    //添加一个菜单item，并注册点击回调函数
    GButton item = menu.AddItem("标题", MenuItemCallback);
    item.name = "item1";

    //点击回调函数，context.data是当前被点击的item
    void MenuItemCallback(EventContext context)
    {
        GButton item = GButton(context.data);
        Debug.Log(item.name);
    }

    //添加分隔条
    menu.AddSeperator();

    //设置菜单项变灰
    menu.SetItemGrayed("item1", true);
```