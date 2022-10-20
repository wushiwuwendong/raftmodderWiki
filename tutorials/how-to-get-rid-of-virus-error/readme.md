# 如何配置您的防病毒软件 
此页面将向您解释如何摆脱因误报而导致的 RMLLauncher 上的多个错误 

---

## 1. 为什么我的杀毒软件怀疑 RMLLauncher 是木马/恶意软件？ 
RMLLauncher 在正在运行的进程（游戏）中注入代码（mods），这是一个非常可疑的行为。   作为一个 mod 启动器，我们需要这样做，但这不是软件的“正常行为”。   然后，您的防病毒软件将其标记为特洛伊木马或试图将自己隐藏在其他应用程序中的恶意软件。 
如果您因此而担心并不确定是否要下载它，您可以前往[开黑了](https://www.kookapp.cn/app/channels/2357391926592835/7880396329959789)或[raft官方频道](https://discord.com/invite/raft) 
上，在 modding 区询问其他用户对 RML 的看法。 
实际上RMLLauncher和Core是[开源](https://www.kookapp.cn/app/channels/2357391926592835/7880396329959789)的，然后由该启动器的用户和有分析知识的修改者进行分析。 如果没有人表明它是一种病毒，那主要是因为它不是，所以不要担心🙂 
## 2.找到你有哪个杀毒软件（如果你已经知道可以通过这一步） 
利用 Ctrl+R然后输入 WSCUI.CPL
![T1](./image%20(4).png)
当您在 Windows 安全中心时，打开“安全”选项卡。 
![T1](./image%20(5).png)
如果您使用的是旧的 Windows 10 版本，您将在此处获得防病毒软件的名称（它是“病毒防护”行）。
![T1](./image%20(2).png)
Windows Defender 防病毒软件示例 
否则，如果您使用的是较新的 Windows 10 版本，您将在“病毒防护”行中看到“在 Windows 安全中查看”，单击它，在这个新窗口中您应该有您的防病毒名称。
![T1](./image%20(3).png) 
Avast 防病毒软件示例 
## 3. 在您的防病毒软件中添加所需的排除项 
现在，您必须在防病毒软件中有 2 个排除项，以避免启动器的所有误报。 
这两个文件是那些： 
RMLLauncher.exe
Rmlassets.asse

    C:\Users\你电脑用户名\AppData\Roaming\RaftModLoader

要在 Windows Defender 中添加例外，您可以点击下一个链接： 
 
  
## 4.尝试通过RMLauncher启动游戏 
如果在那之后你有另一个错误或者你仍然遇到同样的错误，请进入 
的[#获得帮助](https://www.kookapp.cn/app/channels/2357391926592835/7880396329959789)频道，我看到会回复🙂 