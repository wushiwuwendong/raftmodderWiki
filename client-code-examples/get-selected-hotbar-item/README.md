# 获取选定的快捷栏项目 
---
要获取玩家选择的当前项目，我们使用 从  <code class="lang-csharp">PlayerInventory</code> 类里  <code class="lang-csharp">GetSelectedHotbarItem</code> 的方法
```csharp
ItemInstance currentItem = RAPI.GetLocalPlayer().Inventory.GetSelectedHotbarItem();
RConsole.Log("Current Item : " + currentItem.settings_Inventory.DisplayName);
```