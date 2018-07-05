---
title: 坐标系统
type: guide_unity
order: 5
---

## 坐标原点

FairyGUI是以屏幕左上角为原点的，Unity的屏幕坐标是以左下角为原点的。一般这个转换都不需要开发者干预，如果确实需要进行这两者的转换，可以用：

```csharp
    //Unity的屏幕坐标系，以左下角为原点
    Vector2 pos = Input.mousePosition;

    //转换为FairyGUI的屏幕坐标
    pos.y = Screen.height - pos.y;
```

## 坐标转换

GObject里的x/y/position值都是**局部坐标**，也就是相对于父元件的偏移。GObject没有提供直接的属性获得对象的全局坐标，但提供了方法进行转换。

如果要获得任意一个UI元件在屏幕上的坐标，可以用：

```csharp
    Vector2 screenPos = aObject.LocalToGlobal(Vector2.zero);
```

（注意，这里说的屏幕，是指FairyGUI语义中的屏幕，是以屏幕左上角为原点的，不是指Unity语义中的屏幕）

相反，如果要获取屏幕坐标在UI元件上的局部坐标，可以用：

```csharp
    Vector2 localPos = aObject.GlobalToLocal(screenPos);
```

如果有UI适配导致的全局缩放，那么逻辑屏幕大小和物理屏幕大小不一致，逻辑屏幕的坐标就是GRoot里的坐标。如果要进行局部坐标与逻辑屏幕坐标的转换，可以用：

```csharp
    //物理屏幕坐标转换为逻辑屏幕坐标
    Vector2 logicScreenPos = GRoot.inst.GlobalToLocal(screenPos);
    
    //UI元件坐标与逻辑屏幕坐标之间的转换
    aObject.LocalToRoot(pos);
    aObject.RootToLocal(pos);
```

**注意，我们在编辑器里定义的，代码里处理的，一般就是指这个逻辑屏幕坐标。**

如果要转换任意两个UI对象间的坐标，例如需要知道A里面的坐标(10,10)在B里面的位置，可以用：

```csharp
    Vector2 posInB = aObject.TransformPoint(bObject, new Vector2(10,10));
```

## 与世界空间坐标转换

如果要转换世界空间的坐标到UI里的坐标，需要先将世界空间的坐标转换为屏幕坐标，再继续转换，例如：

```csharp
    Vector3 screenPos = Camera.main.WorldToScreenPoint(worldPos);
    //原点位置转换
    screenPos.y = Screen.height - screenPos.y; 
    Vector2 pt = GRoot.inst.GlobalToLocal(screenPos);
```

如果要转换UI里的坐标到世界空间的坐标，需要先将UI里的坐标转换为屏幕坐标，再继续转换，例如：

```csharp
    Vector2 screenPos = GRoot.inst.LocalToGlobal(pos);
    //原点位置转换
    screenPos.y = Screen.height - screenPos.y; 
    一般情况下，还需要提供距离摄像机视野正前方distance长度的参数作为screenPos.z(如果需要，将screenPos改为Vector3类型）
    Vector3 worldPos = Camera.main.ScreenToWorldPoint(screenPos);
```
