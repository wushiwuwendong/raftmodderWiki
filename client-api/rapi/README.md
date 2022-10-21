# RAPI


---

<!-- tabs:start -->
### 从 SteamID 获取玩家用户名 <br/>


#### **方法**

    string GetUsernameFromSteamID(CSteamID steamid)



#### **例子**


    string username = RAPI.GetUsernameFromSteamID(playersteamid);

    RConsole.Log("The player name is " + username);


<!-- tabs:end -->