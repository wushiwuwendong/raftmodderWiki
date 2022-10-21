# RAPI


---
### 从 SteamID 获取玩家用户名
<!-- tabs:start -->

#### **方法**
```C#
string GetUsernameFromSteamID(CSteamID steamid)
```


#### **例子**

```C#
string username = RAPI.GetUsernameFromSteamID(playersteamid);
RConsole.Log("The player name is " + username);
```

<!-- tabs:end -->