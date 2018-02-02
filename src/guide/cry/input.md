---
title: Cry-输入处理
type: guide_cry
order: 30
---

## 鼠标/触摸输入

如果要区分点击UI还是点击场景里的对象，可以使用下面的方法：

```csharp
    if(Stage.isTouchOnUI) //点在了UI上
    {
    }
    else //没有点在UI上
    {
    }
```

这种检测不仅适用于点击，也适用于悬停。例如，如果鼠标悬停在UI上，这个判断也是真。

和鼠标/触摸相关的事件有：

- `onTouchBegin` 鼠标按键按下（左、中、右键），或者手指按下。鼠标按钮可以从context.inputEvent.button获得，0-左键,1-中键,2-右键。
- `onTouchMove` 鼠标指针移动或者手指在屏幕上移动。这个事件只有两种情况会触发，1、在onTouchBegin里调用了context.CaptureTouch()，那么后续的移动事件都会在这个对象上触发（无论手指或指针位置是不是在该对象上方）。2、舞台的onTouchMove始终会触发，即Stage.inst.onTouchMove，不需要使用CaptureTouch捕获。
- `onTouchEnd` 鼠标按键释放或者手指从屏幕上离开。如果鼠标或者触摸位置已经不在组件范围内了，那么组件的TouchEnd事件是不会触发的，如果确实需要，可以在onTouchBegin里调用context.CaptureTouch()请求捕获。
- `onClick` 鼠标或者手指点击。可以从context.inputEvent.isDoubleClick判断是否双击。
- `onRightClick` 鼠标右键点击。

在任何事件（即不只是鼠标/触摸相关的事件）回调中都可以获得当前鼠标或手指位置，以及点击的对象，例如：

```csharp
    void AnyEventHandler(EventContext context)
    {
        Debug.Log(context.inputEvent.x + ", " + context.inputEvent.y);

        Debug.Log(context.sender);
        Debug.Log(context.initiator);
    }
```

如果不在事件中，需要获得当前鼠标或者手指的位置，可以用：

```csharp
    //这是鼠标的位置，或者最后一个手指的位置
    Vector2 pos1 = Stage.inst.touchPosition;

    //获取指定手指的位置，参数是手指id
    Vector2 pos2 = Stage.inst.GetTouchPosition(1);
```

在任何时候，如果需要获得当前点击的对象，或者鼠标下的对象，都可以通过以下的方式获得：

```csharp
    GObject obj = GRoot.inst.touchTarget;

    //判断是不是在某个组件内
    Debug.Log(testComponent.IsAncestorOf(obj));
```

## 多点触摸

FairyGUI支持多点触摸的处理。每个手指都会按照TouchBegin->TouchMove->TouchEnd流程派发事件，可以使用EventContext.inputEvent.touchId区分不同的手指。一般来说，普通的点击事件无需关心手指id，只有需要用到整个触摸流程的才需要处理。

```csharp
    //这是当前按下的手指的数量
    int touchCount = Stage.inst.touchCount;

    //获得当前所有按下的手指id
    int[] touchIDs = Stage.inst.GetAllTouch(null);
```

如果你不想使用多点触摸功能，可以使用Unity的API：Input.multiTouchEnabled = false关闭。

## 键盘输入

侦听键盘输入的方法是：

```csharp
    Stage.inst.onKeyDown.Add(OnKeyDown);

    void OnKeyDown(EventContext context)
    {
        Debug.Log(context.inputEvent.keyCode);
    }
```