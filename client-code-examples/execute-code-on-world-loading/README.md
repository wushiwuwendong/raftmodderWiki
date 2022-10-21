# 在世界加载时执行代码
---

如果您只想在玩家加入世界后执行代码，请使用此方法。

有时你只想在玩家加入世界后执行代码，可能是因为它依赖于游戏对象，然后才被初始化，或者它根本不需要更快地执行。 这是要走的路。
```csharp
public override void WorldEvent_WorldLoaded()
{
    Debug.Log("The world is loaded!");
}
```