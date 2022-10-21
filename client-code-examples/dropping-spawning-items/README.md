# 掉落/生成物品
---

要放置/生成一个项目，我们使用 <code class="lang-csharp">Helper</code> 类中的 <code class="lang-csharp">DropItem</code>方法。
```csharp
Network_Player player = RAPI.GetLocalPlayer();
Item_Base item = ItemManager.GetItemByName("Raw_Potato");
Helper.DropItem(new ItemInstance(item, 1, item.MaxUses), player.transform.position, player.CameraTransform.forward,player.transform.ParentedToRaft());
```