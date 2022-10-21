# 修改私有变量 
---
要访问私有变量，我们需要使用 ****Harmony**** 。   您可以点击[此处](https://harmony.pardeike.net/) 访问 Harmony wiki 。 
>Harmony 是用于在运行时修补、替换和修饰 .NET 和 Mono 方法的库

要使用****Harmony**** ，您只需添加 <code class="lang-csharp">HarmonyLib</code>通过添加命名空间 <code class="lang-csharp">using HarmonyLib;</code>在您的class引用。 

要修改 ****非静态私有变量**** ，您可以将 Traverse.Create() 与对象实例一起使用，将 .Field() 与字段名称一起使用，然后将 .SetValue() 
```csharp
Traverse.Create(ScriptInstance).Field("fieldname").SetValue(newvalue);
// For example to set the value "stats" of the class Network_Player we can do that :
Traverse.Create(RAPI.getLocalPlayer()).Field("stats").SetValue(mynewstats);
```
要修改 ****静态私有变量**** ，您还可以使用 Traverse.Create() 但使用类类型，使用 .Field() 使用字段名称以及 .SetValue(); 
```csharp
Traverse.Create(typeof(classname)).Field("fieldname").SetValue(newvalue);
// For example to set the value "allAvailableItems" of the class ItemManager we can do that :
Traverse.Create(typeof(ItemManager)).Field("allAvailableItems").SetValue(mynewitemlist);
```