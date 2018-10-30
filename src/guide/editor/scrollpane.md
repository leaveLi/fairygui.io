---
title: 滚动容器
type: guide_editor
order: 95
---

## 滚动属性

对组件或者列表设置了“溢出处理”为“水平滚动”、“垂直滚动”，“自由滚动”后，组件或者列表即成为滚动容器。点击“溢出处理”旁边的![](../../images/20170801144514.png)按钮，可以设置详细的滚动的相关属性。

![](../../images/20170802112058.png)

- `滚动条显示` 滚动条的显示策略。 
 - `默认` 使用全局设置，在主菜单“文件”->“项目属性”->“预览设置”里设置。
 - `可见` 表示滚动条一直显示。
 - `滚动时显示` 表示滚动条只有在滚动时才会显示，其他情况下自动隐藏。
 - `隐藏` 表示滚动条一直不可见的状态。

- `边缘回弹效果` 滚动到达边缘时是否允许继续滑动/拖动一定距离，表现一个回弹的效果。一般在移动平台上使用，PC上较少。有些开发者会提出为什么我的滚动容器里内容没超出视口，却依然能滚动，这其实是边缘回弹效果。

- `触摸滚动效果` 是否允许用户直接拖拽滚动区域内的内容。一般在移动平台上使用，PC上较少，PC上一般需要拖动滚动条，或使用鼠标滚轮。

- `滚动条组件` 设置滚动条资源。一般不需要设置，全局有一个设置，在主菜单“文件”->“项目属性”->“预览设置”里。如果你要使用不同于全局设置的滚动条资源，那么在这里设置。

- `定位` 可以设置滚动条在容器中的位置，这是一个相对于正常位置的偏移值。

- `下拉/上拉刷新组件` 设置上拉刷新或下拉刷新时需要显示的组件。下面是下拉刷新的效果：

<center>
![](../../images/gaollg12.gif)
</center>

- `将垂直滚动条显示在左边` 设置垂直滚动条显示在容器的左边，而不是在容器的右边。

- `仅在内容溢出时才显示滚动条` 只有当容器内的内容超出视口区域时，才显示滚动条，否则隐藏。注意：即使是不显示，滚动条还是有预留的占位的，这个和“滚动条显示”设置为“隐藏”不相同，后者是完全取消滚动条的占位的。

- `滚动位置自动贴近元件` 在滚动结束后，保证滚动位置刚好处于任意元件的上边缘（或左边缘）。

- `页面模式` 以视口大小为页面大小，每次滚动的距离是一页。一般在移动平台上使用，PC上较少，拖动滚动条进行滚动操作与这个模式冲突。

- `禁用惯性` 当用手拖拽内容一段距离，并释放手指后，系统会根据手指移动的速度计算出一个速率，然后滚动会按照将此速率衰减到零的方式慢慢停下来，这称为惯性滚动。如果不需要此特性，可以关闭。这个功能是和“触摸滚动效果”配合使用的。

- `禁用剪裁` 一般情况下，容器会对超出视口的内容进行剪裁。特殊情况下，例如，如果一个列表的item组件自身就是滚动容器，那么item组件可以关闭剪裁。因为大量的剪裁会消耗很多的系统性能。

## ScrollPane

当组件的“溢出处理”设置为“滚动”后，可以通过GComponent.scrollPane使用滚动相关的功能，例如：

```csharp
    ScrollPane scrollPane =  aComponent.scrollPane;
    //设置滚动位置为100像素
    scrollPane.posX = 100;

    //滚动到中间位置，带动画过程
    scrollPane.SetPercX(0.5f, true);
```

当你增删子组件后，或者移动子组件的位置、调整子组件的大小，容器是自动更新滚动区域的，不需要调用任何API。这个刷新发生在本帧绘制之前。如果你希望立刻访问子元件的正确坐标，那么可以调用`EnsureBoundsCorrect`通知GComponent立刻重排。EnsureBoundsCorrect是一个友好的函数，你不用担心重复调用会有额外性能消耗。

ScrollPane中常用的API有：

- `viewWidth` `viewHeight` 视口宽度和高度。

- `contentWidth` `contentHeight` 内容高度和宽度。

- `percX` `percY` `SetPercX` `SetPercY` 获得或设置滚动的位置，以百分比来计算，取值范围是0-1。如果希望滚动条从当前值到设置值有一个动态变化的过程，可以使用Set方法，它们提供了一个是否使用缓动的参数。

- `posX` `posY` `SetPosX` `SetPosY` 获得或设置滚动的位置，以绝对像素值来计算。取值范围是0-最大滚动距离。垂直最大滚动距离=（内容高度-视口高度），水平最大滚动距离=（内容宽度-视口宽度）。如果希望滚动条从当前值到设置值有一个动态变化的过程，可以使用Set方法，它们提供了一个是否使用缓动的参数。

- `currentPageX` `currentPageY` `setCurrentPageX` `setCurrentPageY` 如果滚动设置为页面模式，那么可以通过这些方法设置或者获得当前的页面索引。如果要获得页面数量，可以用contentWidth/viewWidth或者contentHeight/viewHeight。

- `ScrollLeft` `ScrollRight` `ScrollUp` `ScrollDown` 向指定方向滚动N*`scrollStep`。例如，如果scrollStep=20，那么ScrollLeft(1)表示向左滚动20像素，ScrollLeft(2)表示向左滚动40像素。注意：如果滚动属性设置了贴近元件，例如元件大小为41像素，则需要滚动距离超过20像素，才能真正发生滚动，那么如果调用ScrollLeft(1)，在scrollStep=20的情况下，会导致看不到任何效果。
  如果滚动设置为页面模式，那这几个API也有“翻一页”的作用。

- `ScrollToView` 调整滚动位置，使指定的元件出现在视口内。

- `touchEffect` 打开或关闭触摸滚动功能。关闭触摸滚动后，用户就不能拖拽视口进行滚动了。

- `scrollStep` 这个值是指滚动“一格”的距离。这个距离有三个用途：a）scrollUp/scrollDown/scrollLeft/scrollRight； b）点击滚动条的箭头按钮； c）鼠标滚轮，鼠标滚轮滚一次的距离是scrollStep*2。

- `bounceBackEffect` 可以打开或关闭边缘回弹功能。

- `mouseWheelEnabled` 打开或关闭鼠标滚动支持。

- `decelerationRate` 减速率，调整这个值可以控制惯性滚动的距离和时间。惯性滚动是指手指拖动一定距离然后释放后，滚动容器内容继续滚动一定距离后停止。越接近1，减速越慢，意味着滑动的时间和距离更长。默认值是UIConfig.defaultScrollDecelerationRate。

- `CancelDragging` 当滚动面板处于拖拽滚动状态或即将进入拖拽状态时，可以调用此方法停止或禁止本次拖拽。

可以侦听滚动改变，在任何情况下滚动位置改变都会触发这个事件。

```csharp
    //Unity/Cry
    scrollPane.onScroll.Add(onScroll);

    //AS3
    scrollPane.addEventListener(Event.SCROLL, onScroll);

    //Egret
    scrollPane.addEventListener(ScrollPane.SCROLL, this.onScroll, this);

    //Laya，注意是用组件侦听，不是ScrollPane
    aComponent.on(fairygui.Events.SCROLL, this, this.onScroll);

    //Cocos2dx，注意是用组件侦听，不是ScrollPane
    aComponent->addEventListener(UIEventType::Scroll, CC_CALLBACK_1(AClass::onScroll, this));
```

和滚动相关的事件还有：

- `ScrollEnd` 惯性滚动结束后回调。
- `PullDownRelease` 下拉刷新回调。
- `PullUpRelease` 上拉刷新回调。

```csharp
    //Unity/Cry
    scrollPane.onScrollEnd.Add(onScrollEnd);
    scrollPane.onPullDownRelease.Add(onPullDownRelease);
    scrollPane.onPullUpRelease.Add(onPullUpRelease);

    //AS3
    scrollPane.addEventListener(ScrollPane.SCROLL_END, onScrollEnd);
    scrollPane.addEventListener(ScrollPane.PULL_DOWN_RELEASE, onPullDownRelease);
    scrollPane.addEventListener(ScrollPane.PULL_UP_RELEASE, onPullUpRelease);

    //Egret
    scrollPane.addEventListener(ScrollPane.SCROLL_END, this.onScrollEnd, this);
    scrollPane.addEventListener(ScrollPane.PULL_DOWN_RELEASE, this.onPullDownRelease, this);
    scrollPane.addEventListener(ScrollPane.PULL_UP_RELEASE, this.onPullUpRelease, this);

    //Laya，注意是用组件侦听，不是ScrollPane
    aComponent.on(fairygui.Events.SCROLL_END, this, this.onScrollEnd);
    aComponent.on(fairygui.Events.PULL_DOWN_RELEASE, this, this.onPullDownRelease);
    aComponent.on(fairygui.Events.PULL_UP_RELEASE, this, this.onPullUpRelease);

    //Cocos2dx，注意是用组件侦听，不是ScrollPane
    aComponent->addEventListener(UIEventType::ScrollEnd, CC_CALLBACK_1(AClass::onScrollEnd, this));
    aComponent->addEventListener(UIEventType::PullDownRelease, CC_CALLBACK_1(AClass::onPullDownRelease, this));
    aComponent->addEventListener(UIEventType::PullUpRelease, CC_CALLBACK_1(AClass::onPullUpRelease, this));
```