# 获取当前用户名
---
要检索当前用户名，我们使用 <code class="lang-csharp">SteamFriends</code> 类中的 <code class="lang-csharp">GetPersonaName</code> 方法。
我们还需要通过添加 <code class="lang-csharp">using Steamworks;</code> 在你的类顶部添加来使用 <code class="lang-csharp">Steamworks</code> 命名空间。
```csharp
using Steamworks;
string username = SteamFriends.GetPersonaName();
```