# RAPI


---

<!-- tabs:start -->
### 从 SteamID 获取玩家用户名


#### **方法**

    <font color="#dd0000">string</font> GetUsernameFromSteamID(CSteamID steamid)



#### **例子**

    string username = RAPI.GetUsernameFromSteamID(playersteamid);
    
    RConsole.Log("The player name is " + username);


<!-- tabs:end -->