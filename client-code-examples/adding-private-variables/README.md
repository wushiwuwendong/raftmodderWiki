# 添加私有变量 
---

## 添加私有变量 
在现有的 Raft 类中添加私有变量有时是非常必要的。有很多方法可以添加新变量。在本文中，我们将考虑 
[Whitebrim](https://www.raftmodding.com/user/Whitebrim)提供的方法。（Discord Whitebrim#4444）
我们将在新创建的类中存储变量，并为原始 Raft 类添加扩展。您可以 
阅读 C# 扩展的工作原理。 
我们需要创造新的 <code class="lang-csharp">ConditionalWeakTable\<TKey, TValue\></code>它将存储我们的数据类并将它们链接到原始类。这个类是专门为我们的案例创建的，如果你有兴趣可以[这里](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)阅读更多。 
在这个例子中，我们将添加新的 <code class="lang-csharp">private bool isLocked</code>可变为 <code class="lang-csharp">SteeringWheel class</code>： 
**首先** ，我们需要 **创建数据类** 来存储我们需要添加的所有数据： 
```csharp
[Serializable]
public class SteeringWheelAdditionalData
{
    public bool isLocked;

    public SteeringWheelAdditionalData()
    {
        isLocked = false;
    }
}
```
**您必须添加 [Serializable]我们的类可以保存的属性。**您还需要创建不带参数的默认构造函数来使用默认值初始化类。 
**其次** ，我们需要 **创建一个扩展类** 来存储和处理数据： 
```csharp
public static class SteeringWheelExtension
{
    private static readonly ConditionalWeakTable<SteeringWheel, SteeringWheelAdditionalData> data = 
        new ConditionalWeakTable<SteeringWheel, SteeringWheelAdditionalData>();

    public static SteeringWheelAdditionalData GetAdditionalData(this SteeringWheel steeringWheel)
    {
        return data.GetOrCreateValue(steeringWheel);
    }

    public static void AddData(this SteeringWheel steeringWheel, SteeringWheelAdditionalData value)
    {
        try
        {
            data.Add(steeringWheel, value);
        }
        catch (Exception) { }
    }
}
```
然后你需要 **修补原始类** 。   在我们的例子中，我们需要根据 **IsLocked** 状态。  我们会打补丁 **OnIsRayed()**里面的方法 **SteeringWheel** class： 
```csharp
[HarmonyPatch(typeof(SteeringWheel), "OnIsRayed")]
class SteeringWheelPatchOnIsRayed
{
    private static void Postfix(SteeringWheel __instance, ref DisplayTextManager ___displayText)
    {
        if (!__instance.GetAdditionalData().isLocked)
            ___displayText.ShowText("Press to LOCK rotation", MyInput.Keybinds["RMB"].MainKey, 2, 0, false);
        else
            ___displayText.ShowText("Press to UNLOCK rotation", MyInput.Keybinds["RMB"].MainKey, 2, 0, false);

        if (MyInput.GetButtonDown("RMB"))
            __instance.GetAdditionalData().isLocked = !__instance.GetAdditionalData().isLocked; // Toggle bool
    }
}
```
阅读有关 **Harmony补丁**[这里](https://github.com/pardeike/Harmony/wiki/Patching) 的更多信息。 
您还需要 **应用补丁** ： 
```csharp
public class BetterSteeringWheel : Mod
{
    private HarmonyInstance harmonyInstance;

    public void Start()
    {
        harmonyInstance = new Harmony("com.whitebrim.bettersteeringwheel"); // It's custom patch name, you need to name your patch differently
        harmonyInstance.PatchAll(Assembly.GetExecutingAssembly());
    }

    public void OnModUnload()
    {
        harmonyInstance.UnpatchAll();
        Destroy(gameObject);
    }
}
```
您可以使用类实例访问新数据。 **SteeringWheel.GetAdditionalData()**. 
示例 **float** 和 **string**: 
```csharp
[Serializable]
public class YourClassAdditionalData
{
    public float floatVar;
    public string stringVar;

    public CustomClassAdditionalData()
    {
        floatVar = 0;
        stringVar = "None";
    }
}

public static class CustomExtension // Nothing changed :)
{
    private static readonly ConditionalWeakTable<YourClass, YourClassAdditionalData> data = 
        new ConditionalWeakTable<YourClass, YourClassAdditionalData>();

    public static YourClassAdditionalData GetAdditionalData(this YourClass yourClass)
    {
        return data.GetOrCreateValue(yourClass);
    }

    public static void AddData(this YourClass yourClass, YourClassAdditionalData value)
    {
        try
        {
            data.Add(yourClass, value);
        }
        catch (Exception) { }
    }
}
```
---
## 保存私有变量 
要保存私有变量，您需要添加补丁 **RestoreRGDGame(RGD_Game game)** 方法的 **SaveAndLoad** 类中的 switch 内部调用的 **RDG_%RaftClassName%** 类和方法。 
在这个例子中，我们将使用 **SteeringWheel**类并保存数据 **Adding private variables**部分。 
**首先** ，我们必须添加新链接 **RGD Class -> our additional data class**（条件弱表）： 
```csharp
public static class SteeringWheelExtension
{
    public static ConditionalWeakTable<RGD_SteeringWheel, SteeringWheelAdditionalData> RGD_data = 
            new ConditionalWeakTable<RGD_SteeringWheel, SteeringWheelAdditionalData>();

    public static void AddData(this RGD_SteeringWheel RGD_SteeringWheel, SteeringWheelAdditionalData value)
    {
        try
        {
            RGD_data.Add(RGD_SteeringWheel, value);
        }
        catch (Exception) { }
    }
}
```
其次 ，我们添加补丁 **RGD_SteeringWheel** 类。有两个构造函数（一个用于保存，另一个用于加载）和 **GetObjectData**方法（控制序列化流程）。 
```csharp
class RGD_SteeringWheelPatch
{
    [HarmonyPatch(typeof(RGD_SteeringWheel), MethodType.Constructor, new Type[]{ typeof(RGDType), typeof(SteeringWheel) })]
    class RGD_SteeringWheelConstructor1 // Constructor for saving
    {
        private static void Prefix(RGD_SteeringWheel __instance, ref SteeringWheel steeringWheel)
        {
            __instance.AddData(steeringWheel.GetAdditionalData());
        }
    }

    [HarmonyPatch(typeof(RGD_SteeringWheel), MethodType.Constructor, new Type[] { typeof(SerializationInfo), typeof(StreamingContext) })]
    class RGD_SteeringWheelConstructor2 // Constructor for loading
    {
        private static void Prefix(RGD_SteeringWheel __instance, ref SerializationInfo info)
        {
            try
            {
                __instance.AddData(JsonUtility.FromJson<SteeringWheelAdditionalData>(info.GetString("AdditionalData"))); // Loads from Json that we will create in GetObjectData
            }
            catch (Exception) { }
        }
    }

    [HarmonyPatch(typeof(RGD_SteeringWheel), "GetObjectData")]
    class GetObjectData // Interfering with the serialization flow and adding our custom data to the save
    {
        private static void Postfix(RGD_SteeringWheel __instance, ref SerializationInfo info)
        {
            SteeringWheelAdditionalData value;
            if (SteeringWheelExtension.RGD_data.TryGetValue(__instance, out value))
                info.AddValue("AdditionalData", JsonUtility.ToJson(value)); // We need to use json because worlds loads before mod compiles
        }
    }
}
```
**最后**，我们需要找到加载保存的块数据并修补它的方法。 此方法在 **RestoreRGDGame(RGD_Game game)** 方法的 **SaveAndLoad** 类的 **switch** 内部调用：
```csharp
switch (rgd.type)
    {
    case RGDType.Block:
    {
      // Important code
      break;
    }
    case RGDType.Block_Door:
    {
      // Important code
      break;
    }
    // etc...
```
我们的 *案例* ： 
```csharp
case RGDType.Block_SteeringWheel:
    {
      RGD_SteeringWheel rgd_SteeringWheel = rgd as RGD_SteeringWheel;
      Block block19 = this.RestoreBlock(network_Player.BlockCreator, rgd_SteeringWheel);
      if (block19 != null)
      {
          SteeringWheel componentInChildren3 = block19.GetComponentInChildren<SteeringWheel>();
          if (componentInChildren3 != null)
          {
              componentInChildren3.RestoreWheel(rgd_SteeringWheel); // <- This line
          }
      }
      break;
    }
```
在我们的例子中，方法称为 **RestoreWheel(RGD_SteeringWheel rgdWheel)**。 此方法包含有关如何反序列化 RGD 类以检索保存的数据的说明。 我们将添加我们的自定义说明：
```csharp
[HarmonyPatch(typeof(SteeringWheel), "RestoreWheel")]
class SteeringWheelRestoreWheelPatch
{
    private static void Prefix(SteeringWheel __instance, RGD_SteeringWheel rgdWheel)
    {
        SteeringWheelAdditionalData value;
        if (SteeringWheelExtension.RGD_data.TryGetValue(rgdWheel, out value))
            __instance.AddData(value);
    }
}
```
**恭喜** ，我们的方向盘自定义数据已成功保存和加载。 
您可以在[此处](https://raftmodder.mcxiaodong.top/mods/better-steering-wheel)下载mod 时找到完整代码。 