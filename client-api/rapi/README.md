# RAPI


---
### 从 SteamID 获取玩家用户名
<!-- tabs:start -->

#### **方法**
```csharp
string GetUsernameFromSteamID(CSteamID steamid)
```


#### **例子**

```csharp
string username = RAPI.GetUsernameFromSteamID(playersteamid);
RConsole.Log("The player name is " + username);
```

<!-- tabs:end -->


### 切换鼠标光标 

<!-- tabs:start -->
#### **方法**
```csharp
void ToggleCursor(bool var)
```


#### **例子**

```csharp
RAPI.ToggleCursor(true); // 这将显示光标.
RAPI.ToggleCursor(false); // 这将隐藏光标
```
<!-- tabs:end -->
### 注册一个新项目 
<!-- tabs:start -->
#### **方法**
```csharp
void RegisterItem(Item_Base item)
```


#### **例子**

```csharp
// 这里我们从assetbundle加载项目
Item_Base yournewitem = asset.LoadAsset<Item_Base>("yournewitem");
// 然后我们使用 RegisterItem() 注册它
RAPI.RegisterItem(yournewitem);
```
<!-- tabs:end -->
### 设置项目预制件
<!-- tabs:start --> 
#### **方法**
```csharp
void SetItemObject(Item_Base item, GameObject prefab)
// 默认手是右手 RItemHand 可以是 leftHand 或 rightHand
void SetItemObject(Item_Base item, GameObject prefab, RItemHand parent)
```


#### **例子**

```csharp
// 这里我们从assetbundle中加载item
Item_Base yournewitem = asset.LoadAsset<Item_Base>("yournewitem");
// 然后我们使用 RegisterItem() 注册它
RAPI.RegisterItem(yournewitem);
// 然后我们通过从assetbundle加载它来设置他的预制件
RAPI.SetItemObject(yournewitem, asset.LoadAsset<GameObject>("yournewitem"));
``` 
<!-- tabs:end -->
### 获取本地 Network_Player 脚本
<!-- tabs:start -->
#### **方法**
```csharp
Network_Player GetLocalPlayer()
```


#### **例子**

```csharp
Network_Player player = RAPI.GetLocalPlayer();
// player 变量将包含本地播放器脚本
```
<!-- tabs:end -->
### 给本地玩家一件物品
<!-- tabs:start -->
#### **方法**
```csharp
void GiveItem(Item_Base item, int amount)
```


#### **例子**

```csharp
RAPI.GiveItem(ItemManager.GetItemByName("raw_potato"),10);
//这将为本地玩家提供 10 个生土豆
```
<!-- tabs:end -->
### 广播聊天消息
<!-- tabs:start -->
#### **方法**
```csharp
void BroadcastChatMessage(string message)
```


#### **例子**

```csharp
RAPI.BroadcastChatMessage("我是一个消息");
// 将向每个玩家发送"我是一个消息"
``` 
<!-- tabs:end -->
### 允许在网格系统上放置块
<!-- tabs:start --> 
#### **方法**
```csharp
void AddItemToBlockQuadType(Item_Base item, RBlockQuadType quadtype)
```


#### **例子**

```csharp
RAPI.AddItemToBlockQuadType(YourNewItem, RBlockQuadType.quad_foundation);
// 允许将"YourNewItem"放置在地基上
```
<!-- tabs:end -->
### 不允许在网格系统上放置块
<!-- tabs:start --> 
#### **方法**
```csharp
void RemoveItemFromBlockQuadType(string itemUniqueName, RBlockQuadType quadtype)
```


#### **例子**

```csharp
RAPI.RemoveItemFromBlockQuadType("YourItemUniqueName", RBlockQuadType.quad_foundation);
// 不允许将具有唯一名称"YourItemUniqueName"的项目放置在地基上

```
<!-- tabs:end -->
### 侦听特定网络通道上的网络消息（0和1为游戏锁定，不能使用） 
<!-- tabs:start -->
#### **方法**
```csharp
NetworkMessage ListenForNetworkMessagesOnChannel(int channel = 0)
```


#### **例子**

```csharp
// 为频道 ID 选择唯一 ID，以免干扰其他模组.
NetworkMessage netMessage = RAPI.ListenForNetworkMessagesOnChannel(30);
if (netMessage != null)
{
  CSteamID id = netMessage.steamid;
  Message message = netMessage.message;
   // 这里我们使用 5000 因为我们不能修改枚举，你可以使用任何值
   // 只要它不在 Messages 枚举中 大于 1000 是完美的
  if(message.Type == (Messages)5000){
   // 用你知道的消息做你的事情
   // 它是你的和它想要的类型
    YourMessageClass msg = message as YourMessageClass;
  }
}
```
<!-- tabs:end -->
### 向所有玩家发送网络消息
<!-- tabs:start -->
#### **方法**
```csharp
void SendNetworkMessage(Message message, int channel = 0, EP2PSend ep2psend = EP2PSend.k_EP2PSendReliable, Target target = Target.Other, CSteamID fallbackSteamID = new CSteamID())
```


#### **例子**

```csharp
// 这会将您的网络消息发送给所有玩家
RAPI.SendNetworkMessage(new YourMessageClass((Messages)5000));
```
<!-- tabs:end -->