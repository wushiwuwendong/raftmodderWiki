# 访问玩家实例
---

为了访问本地玩家实例，我们使用 RAPI 类中的 <code class="lang-csharp">GetLocalPlayer</code> 方法，它返回玩家 <code class="lang-csharp">Network_Player</code> 的实例
```csharp
Network_Player player = RAPI.GetLocalPlayer();
```