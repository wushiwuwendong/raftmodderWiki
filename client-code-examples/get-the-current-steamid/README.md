# 获取当前 SteamID
---
要检索当前用户 <code class="lang-csharp">steamid</code>，我们使用 <code class="lang-csharp">SteamUser</code> 类的 <code class="lang-csharp">GetSteamID</code> 方法。
我们还需要通过添加 <code class="lang-csharp">using Steamworks;</code> 在你的类顶部添加来使用 <code class="lang-csharp">Steamworks</code> 命名空间。
```csharp
using Steamworks;
CSteamID steamid = SteamUser.GetSteamID();
```