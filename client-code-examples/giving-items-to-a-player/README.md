# 给玩家物品
---

要将物品提供给玩家，您可以使用 <code class="lang-csharp">PlayerInventory</code> 类的 <code class="lang-csharp">AddItem</code> 方法。
```csharp
// RAPI.GetLocalPlayer().Inventory.AddItem();

RAPI.GetLocalPlayer().Inventory.AddItem("Raw_Potato",1);
```