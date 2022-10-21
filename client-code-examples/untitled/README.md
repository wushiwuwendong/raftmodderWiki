# 读取私有变量 

---

在修改 raft 时，读取私有变量显然是必要的。  让我们看看它是多么容易！ 
要访问私有变量，我们需要使用 ***Harmony*** 。   您可以点击[此处](https://harmony.pardeike.net/) 访问 Harmony wiki 。 
>Harmony 是用于在运行时修补、替换和修饰 .NET 和 Mono 方法的库

要使 Visual Studio 自动完成功能正常工作，您需要安装 Nuget 包 Lib.Harmony 版本 2。然后，要使用 Harmony，您只需添加 <code class="lang-csharp">HarmonyLib</code> 通过添加命名空间<code class="lang-csharp"> using HarmonyLib;</code>在您的班级中名列前茅。  你不需要为 Raft 安装 nuget 包来编译代码，但它使开发更容易。 
要访问 ***非静态私有变量*** ，您可以使用带有对象实例的 Traverse.Create() 和带有字段名称的 .Field()。 
```csharp
string myvalue = Traverse.Create(ScriptInstance).Field("fieldname").GetValue() as string;

// 例如，要获取 Network_Player 类的值"stats"，我们可以这样做 :
PlayerStats stats = Traverse.Create(RAPI.getLocalPlayer()).Field("stats").GetValue() as PlayerStats;
```
要访问 ***静态私有变量*** ，您还可以使用 <code class="lang-csharp">Traverse.Create()</code> 但使用类类型和使用 .Field() 使用字段名称。 

```csharp
string myvalue = Traverse.Create(typeof(classname)).Field("fieldname").GetValue() as string;
// 例如，要获取 ItemManager 类的值"allAvailableItems"，我们可以这样做:
List<Item_Base> allAvailableItems = Traverse.Create(typeof(ItemManager)).Field("allAvailableItems").GetValue() as List<Item_Base>;
```