---
title: 坐标转换
type: guide_unity
order: 5
---

FairyGUI是以左上角为原点的，Unity的屏幕坐标是以左下角为原点的。如果需要进行这两者的转换，可以用：

```csharp
    //Unity的屏幕坐标系，以左下角为原点
    Vector2 pos = Input.mousePosition;

    //转换为FairyGUI的屏幕坐标
    pos.y = Screen.height - pos.y;
```

如果要获得任意一个UI元件在屏幕上的坐标，可以用：

```csharp
    Vector2 screenPos = aObject.LocalToGlobal(Vector2.zero);
```

如果要获取屏幕坐标在UI元件上的局部坐标，可以用：

```csharp
    Vector2 screenPos = Stage.inst.touchPosition;
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

如果要转换任意两个UI对象间的坐标，例如需要知道A里面的坐标(10,10)在B里面的位置，可以用：

```csharp
    Vector2 posInB = aObject.TransformPoint(bObject, new Vector2(10,10));
```

如果要转换世界空间的坐标到UI里的坐标，可以用：

```csharp
    Vector3 screenPos = Camera.main.WorldToScreenPoint(worldPos);
    //原点位置转换
    screenPos.y = Screen.height - screenPos.y; 
    Vector2 pt = GRoot.inst.GlobalToLocal(screenPos);
```

如果要转换UI里的坐标到世界空间的坐标，可以用：

```csharp
    Vector2 screenPos = GRoot.inst.LocalToGlobal(pos);
    //原点位置转换
    screenPos.y = Screen.height - screenPos.y; 
    Vector3 worldPos = Camera.main.ScreenToWorldPoint(screenPos);
```