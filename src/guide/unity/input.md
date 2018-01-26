---
title: 输入处理
type: guide_unity
order: 30
---

## 鼠标/触摸输入

FairyGUI使用内置的机制进行鼠标和触摸事件的处理，不使用射线。如果确实要使用射线，可以将UIPanel的“HitTest Mode”设置为“Raycast”。无论哪种点击检测模式，下面的事件处理机制都一样。

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
- `onClick` 鼠标或者手指点击。可以从context.inputEvent.isDoubleClick判断是否双击。**如果你在找长按事件，那么请使用LongPressGesture[(长按手势)](#手势)。**
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

## VR输入处理

VR里输入一般使用凝视输入，或者手柄输入，针对这些新的输入方式，FairyGUI提供了封装支持，也就是说，在VR应用里，你仍然可以像处理鼠标或者触摸输入一样处理VR的输入，无任何区别。

首先，需要把这些外部输入传入FairyGUI。在Stage类里提供了这些API：

```csharp
    public void SetCustomInput(ref RaycastHit hit, bool buttonDown);
    public void SetCustomInput(ref RaycastHit hit, bool buttonDown, bool buttonUp);
```

- `hit` 没有手柄的，这里传入眼睛的射线（其实就是摄像机的射线）击中的目标；有手柄的，传入手柄射线击中的目标。

- `buttonDown` 是否有按键按下。对于没有buttonUp参数的API，系统会在下一帧自动模拟一个buttonUp。

- `buttonUp` 是否有按键松开。按下、松开整个过程构成一次点击。如果只有按下没有松开，则不会有点击触发，必须注意这一点。

SetCustomInput可以放在Update里调用，而且必须**每帧调用**。如果使用了SetCustomInput，则FairyGUI不再处理鼠标或者触摸出入。

通过这种方式处理VR输入，UI逻辑不需要做任何修改。

## 键盘输入

侦听键盘输入的方法是：

```csharp
    Stage.inst.onKeyDown.Add(OnKeyDown);

    void OnKeyDown(EventContext context)
    {
        Debug.Log(context.inputEvent.keyCode);
    }
```

在手机上是通过原生的键盘输入。键盘弹出时，派发GTextInput.onFocusIn事件，键盘收回时，派发GTextInput.onFocusOut事件。

Unity在键盘输入时自带了一个额外的输入框，如果你不需要这个输入框，希望像微信那样弹出自己的输入框，你需要自行编写原生代码，FairyGUI这边提供的支持有：

```csharp
    //定义自己的键盘
    KeyBoard yourKeyboard;

    Stage.inst.keyboard = yourKeyboard;
```

**复制粘贴问题**

当使用DLL形式的插件时，因为DLL默认是为移动平台编译的，所以不支持复制粘贴（如果要支持，需要自己写原生）。如果是在PC平台上使用时，需要将[CopyPastePatch.cs](https://github.com/fairygui/FairyGUI-unity/blob/master/Examples.Unity5/Assets/FairyGUI/CopyPastePatch.cs)放到工程里，并在游戏启动时调用CopyPastePatch.Apply()，就可以在PC平台激活复制粘贴功能。如果你是使用源码形式的插件，不需要进行这个处理。

## 手势

FairyGUI提供了手势的支持。使用手势的方式是：

```csharp
    LongPressGesture gesture = new LongPressGesture(targetObject);
    gesture.onAction.Add(OnGestureAction);
```

targetObject是接收手势的元件，注意一定要是可触摸的。图片是不可触摸的，一般建议用组件，或者图形（图形透明度可以设置为0）。如果你需要全屏幕监测手势，那么可以直接用GRoot.inst作为targetObject（需1.9.0 SDK或更高版本支持）

常用的手势有：

`LongPressGesture` 长按手势。

`SwipeGesture` 手指划动手势。

`PinchGesture`两指缩放手势。

`RotationGesture` 两指旋转手势。

手势的使用方法可以参考Gesture这个Demo。
