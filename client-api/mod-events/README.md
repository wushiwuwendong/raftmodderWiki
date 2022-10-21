# 模组事件 
###### Mod Events 让您的生活更轻松！  它允许您在游戏中发生特定事件时运行代码！ 
---

### 如何使用它们？ 
  这很简单！   在您的 mod 类中只需覆盖它们，如下所示。 
```Csharp
public override void WorldEvent_WorldLoaded()
{
    Debug.Log("The world is loaded!");
}
```
### 模组事件列表
|  方法   | 描述  |
|  ----  | ----  |
| <code class="lang-csharp">void WorldEvent_WorldLoaded()</code>  | 加载世界时调用|
| <code class="lang-csharp">void WorldEvent_WorldSaved() </code> | 当世界得救时调用|
| <code class="lang-csharp">void LocalPlayerEvent_Hurt(float damage, Vector3 hitPoint, Vector3 hitNormal, EntityType damageInflictorEntityType)</code>  | 当本地玩家受伤时调用|
| <code class="lang-csharp">void LocalPlayerEvent_Death(Vector3 deathPosition)</code>  | 当本地玩家死亡时调用|
| <code class="lang-csharp">void LocalPlayerEvent_Respawn()</code>  | 当本地玩家重生时调用|
| <code class="lang-csharp">void LocalPlayerEvent_ItemCrafted(Item_Base item)</code>  | 当本地玩家制作物品时调用|
| <code class="lang-csharp">void LocalPlayerEvent_PickupItem(PickupItem item)</code>  | 当本地玩家拾取物品时调用|
| <code class="lang-csharp">void LocalPlayerEvent_DropItem(ItemInstance item, Vector3 position, Vector3 direction, bool parentedToRaft)</code>  | 当本地玩家掉落物品时调用|
| <code class="lang-csharp">void LocalPlayerEvent_OnRespawn()</code>  | 当本地玩家重生时调用|
| <code class="lang-csharp">void WorldEvent_OnPlayerConnected(CSteamID steamid, RGD_Settings_Character characterSettings)</code>  | 当玩家加入世界时调用|
| <code class="lang-csharp">void WorldEvent_OnPlayerDisconnected(CSteamID steamid, DisconnectReason disconnectReason) </code> | 当玩家离开世界时调用|
| <code class="lang-csharp">void WorldEvent_WorldUnloaded()</code>  | 在世界卸载时调用（返回主菜单）|
