---
title: Drag&Drop
type: guide_editor
order: 205
---

## 自由拖放

使一个元件能够被拖动，很简单，设置draggable属性就可以了，例如：

```csharp
    aObject.draggable = true;
```

设置后当玩家按住元件，就可以随意拖动它。可以设置一个矩形限制拖动范围：

```csharp
    //注意这里的矩形范围使用的是舞台上的坐标，不是元件的本地坐标。
    aObject.dragBounds = new Rect(100,100,200,200);
```

拖动开始、拖动的过程和拖动结束都可以获得通知：

```csharp
    //Unity/Cry
    aObject.onDragStart.Add(onDragStart);
    aObject.onDragMove.Add(onDragMove);
    aObject.onDragEnd.Add(onDragEnd);

    //AS3
    aObject.addEventListener(DragEvent.DRAG_START, onDragStart);
    aObject.addEventListener(DragEvent.DRAG_MOVING, onDragMove);
    aObject.addEventListener(DragEvent.DRAG_END, onDragEnd);

    //Egret
    aObject.addEventListener(fairygui.DragEvent.DRAG_START, this.onDragStart, this);
    aObject.addEventListener(fairygui.DragEvent.DRAG_MOVING, this.onDragMove, this);
    aObject.addEventListener(fairygui.DragEvent.DRAG_END, this.onDragEnd, this);

    //Laya
    aObject.on(fairygui.Events.DRAG_START, this, this.onDragStart);
    aObject.on(fairygui.Events.DRAG_MOVE, this, this.onDragMove);
    aObject.on(fairygui.Events.DRAG_END, this, this.onDragEnd);

    //Cocos2dx
    aObject->addEventListener(UIEventType::DragStart, CC_CALLBACK_1(AClass::onDragStart, this));
    aObject->addEventListener(UIEventType::DragMove, CC_CALLBACK_1(AClass::onDragMove, this));
    aObject->addEventListener(UIEventType::DragEnd, CC_CALLBACK_1(AClass::onDragEnd, this));
```

## 转换拖动

如果不希望点击元件的任何地方都可以拖动，那么可以用转换拖动的方式。例如FairyGUI里的窗口，点击它的标题栏，就可以拖动窗口，我们以这个为例子分析怎么实现：

```csharp
    //设置拖动区域为可拖动，然后侦听拖动开始事件
    _dragArea.draggable = true;
    _dragArea.onDragStart.Add(onDragStart);

    void onDragStart(EventContext context)
    {
        //取消掉源拖动，也就是_dragArea不会被实际拖动
        context.PreventDefault();
    
        //设置窗口处于拖动状态。context.data是手指的id。
        this.StartDrag((int)context.data);
    }
```

通过以上的方式，实现当_dragArea被尝试拖动时，会转换为Window自身的拖动。

## 替身拖动

拖动只能在元件的父组件内移动，如果你需要在全屏幕内移动，那么需要用到替身拖动。替身拖动的启动方式需要先作转换：

```csharp
    aObject.draggable = true;
    aObject.onDragStart.Add(onDragStart);

    void onDragStart(EventContext context)
    {
        //取消掉源拖动
        context.PreventDefault();
    
        //icon是这个对象的替身图片，userData可以是任意数据，底层不作解析。context.data是手指的id。
        DragDropManager.inst.StartDrag(null, icon, userData, (int)context.data);
    }
```

使用了替身拖动后，如果要检测拖动结束，不能在监听原来的对象，而应该使用：

```csharp
    DragDropManager.inst.dragAgent.onDragEnd.Add(onDragEnd);
```

DragDropManager还提供了常用的拖->放功能，如果一个组件需要接收其他元件拖动到它里面并释放的事件，可以使用：

```csharp
    aComponent.onDrop.Add(onDrop);

    void onDrop(EventContext context)
    {
        //这里context.data就是StartDrag里传入的userData
        Debug.Log(context.data);
    }
```

DragDropManager使用了一个图片资源表达替身，这个图片是用装载器显示的。这个装载器是DragDropManager.inst.dragAgent，你可以调整它的参数以适应实际项目需求。

如果你的替身不是一个图片那么简单，比如你需要用一个组件作为替身，那么你可以定义自己的DragDropManager，直接复制一个DragDropManager，在上面修改写你自己的逻辑，然后使用你这个DragDropManager即可。DragDropManager这个类的设计就没有考虑到所有实际情况的，它只是作为一个参考。