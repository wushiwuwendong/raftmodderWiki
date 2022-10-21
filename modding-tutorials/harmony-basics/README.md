# Harmony 基础

---

<code class="lang-csharp">using HarmonyLib;</code>

---

## 添加到代码 
要使您的代码多次与现有的 mechaincs 一起工作，您需要将自己的代码添加到内置对象中。  这也可以让您的 mod 采取不同的行动，具体取决于玩家是否安装了特定的其他 mod，例如修改配方以使用 mod 项目（如果可用）或使用原版项目。 

Harmony 允许您通过创建补丁来做到这一点。  有几种有效的方法来设置你的补丁，但一种可靠的方法是使用属性。 

每个补丁都将是它自己的类，如下所示： 
```Csharp
[HarmonyPatch(typeof(BlockCreator), "CanBuildBlock")]
public class HarmonyPatch_IgnoreCollisionOnAlt
{
    [HarmonyPrepare]
    static bool IsCoolModInstalled()
    {...}

    [HarmonyPostfix]
    static BuildError CheckForAlt(BuildError __result)
    {...}
}
```
使用一种或多种添加或作用于目标代码的静态方法。 
>[!WARNING]
>请记住，您的方法必须是静态的！ 

class 属性让 Harmony 知道要在"什么"上使用代码。  typeof(BlockCreator) 表示它正在尝试更改 BlockCreator 的方法之一的工作方式。  该方法是 CanBuildBlock() ，Harmony 通过字符串搜索它，因此请确保它与方法名称匹配！ 
method 属性让 Harmony 知道"何时"使用补丁中的代码。[HarmonyPrepare]、[HarmonyPrefix]、[HarmonyPostfix]、[HarmonyTranspiler] 和 [HarmonyTargetMethod] 分别在不同的时间或以不同的方式执行。 
## [HarmonyPrepare] 
这使您可以在补丁发生之前检查一些事情，看看您是否甚至想要更改某些内容。 
此方法期望您返回 false表示 Harmony 应该跳过这个补丁。 
```Csharp
[HarmonyPrepare]
static bool IsBoxOpeningChanged()
{
    //查看所有已修补的方法
    foreach(MethodBase method in Harmony.GetAllPatchedMethods()){
        //看看你需要的有没有修改过
        if(method.Name == "OpenBox")
            //跳过补丁，因为另一个模组改变了你使用的东西
            return false;
    }
    return true;
} 
```
## [HarmonyPrefix] 
这段代码发生在原始方法发生之前，可以用来一起跳过它。 
这意味着您可以通过使用 Prefix 来完全替换方法，然后通过返回跳过原始方法 false
如果原始方法需要返回一些值，则修补方法可以接受 ref "Type" __result引用论证。  这将使它看起来像原始方法正在返回任何 __result被设定为 
```Csharp
[HarmonyPrefix]
static bool AlwaysReturnTrue(ref bool __result)
{
    //将返回值设置为 true
    __result = true;
    //告诉 Harmony 不要运行原始方法
    return false;
}
```

您也可以使用此方法存储可以在 Postfix 中访问的值，如果您有一个 via <code class="lang-csharp">__state</code>. 您可以使用 <code class="lang-csharp">ref</code>或者 <code>out</code>设置<code class="lang-csharp">__state</code>到您想要的任何值或类型。  如果您需要存储更多数据，那么您需要制作自己的对象以传递给 Postfix 
```Csharp
static void Prefix(out Stopwatch __state)
{
    __state = new Stopwatch(); // 秒表是一个自定义计时器类
    __state.Start();
}

static void Postfix(Stopwatch __state)
{
    __state.Stop();
    FileLog.Log(__state.Elapsed.ToString());
}
```
[HarmonyPostfix] 
此代码发生在原始方法之后。  它可以在 __result从原始方法并使用它甚至更改它。 
Postfix 的一个好处是它们始终运行。  无论原始方法从哪里逃脱其代码，它都会立即进入 Postfix。 
>[!NOTE]
>Postfix 的一个怪癖是它们可以改变 __result通过 ref 或者可以返回一个以相同方式工作的值，但是如果它们返回一个结果，则它必须与方法中的第一个参数是相同的类型！ 

```Csharp
[HarmonyPostfix]
static BuildError CheckForAlt(BuildError __result, BlockCreator __instance)
{...}
```
[HarmonyTranspiler] 
转译器是让您自己的代码运行的更高级的方法。   这里有点超出教程的范围，所以我将链接到[这里](https://harmony.pardeike.net/articles/patching-transpiler.html#basic-transpiler-tutorial) 
  
提供一个简化的概述：您不是编写要执行的代码，而是查看一段代码所代表的所有 IL 指令，并在您认为合适的时候执行一些您自己的指令。 
还有一些很棒的工具可以帮助你展示你的 IL 代码在 C# 代码片段中的样子。尝试检查[LINQPAD](https://www.linqpad.net/) 
  
[HarmonyTargetMethod] 
TargetMethod 是另一个强大的工具，可让您重用代码并将更改应用于多个不同的方法。  它还可用于根据代码找到的内容有条件地应用更改。 
TargetMethod 必须返回 MethodBase 类型，该类型指向您的补丁将应用于哪个方法。  或者，[HarmonyTargetMethods] 也可以用于将相同的补丁逻辑应用于多个方法，尽管在这种情况下它需要一些 MethodBases 的 IEnumerable 集合。 
两种不同的方法是迭代，修补所有方法： 
```Csharp
[HarmonyTargetMethods]
static IEnumerable<MethodBase> PatchInventoryMethods()
{
    yield return AccessTools.Method(typeof(Inventory), "Add");
    yield return AccessTools.Method(typeof(Inventory), "Remove");
    // you could also iterate using reflections over many methods
}
```
或者，影响您收集的一组方法： 
```Csharp
[HarmonyTargetMethods]
IEnumerable<MethodBase> PatchAllPlayerMethods()
{
    //Find all non-void methods beginning with "Player"
    return AccessTools.GetTypesFromAssembly(someAssembly)
        .SelectMany(type => type.GetMethods())
        .Where(method => method.ReturnType != typeof(void) && method.Name.StartsWith("Player"))
        .Cast<MethodBase>();
}
```
有效补丁规则 
您的补丁方法可以接受各种争论，让您可以查看正在运行或即将运行的代码。 
每个前缀和后缀都可以获取原始方法的所有参数以及实例（如果原始方法不是静态的）和返回值。  为了修补方法，您的补丁在定义它们时需要遵循以下原则： 
- 补丁必须是静态方法 
- 前缀补丁的返回类型为 void 或 bool 
- 后缀补丁的返回类型为 void 或返回签名必须与第一个参数的类型匹配（直通模式） 
- 补丁可以使用一个名为的参数 <code class="lang-csharp">__instance</code>如果原始方法不是静态的，则访问实例值 
- 补丁可以使用一个名为的参数 <code class="lang-csharp">__result</code>访问返回值（前缀获取默认值） 
- 补丁可以使用一个名为的参数 <code class="lang-csharp">__state</code>将信息存储在前缀方法中，可以在后缀方法中再次访问。  将其视为局部变量。  它可以是任何类型，您负责在前缀中初始化它的值 
- 以三个下划线开头的参数名称，例如 <code class="lang-csharp">___someField</code>, 可用于在具有相同名称的实例上读取和写入（使用'ref'）私有字段（减去下划线） 
- 补丁可以只定义他们想要访问的那些参数（不需要定义所有） 
- 补丁参数必须使用与原始方法完全相同的名称和类型（对象也可以） 
- 补丁既可以正常获取参数，也可以通过声明任何参数 ref（用于操作） 
- 为了允许补丁重用，可以通过使用名为的参数注入原始方法<code class="lang-csharp">__originalMethod</code>
>[!NOTE]
>使用三个下划线 ___likeSo读取变量非常有用 

转译器还有一些其他可选参数： 
- 将设置为当前 IL 代码生成器的 ILGenerator 类型的参数 
- MethodBase 类型的参数，将设置为正在修补的当前原始方法 
- 它们必须包含一个类型的参数 <code class="lang-csharp">IEnumerable\<CodeInstruction\></code>将用于将 IL 代码传递给它 

还有其他方法可以组合属性和构建补丁。 检查[这里](https://harmony.pardeike.net/articles/annotations.html#combining-annotations)。 
>[!NOTE]
>如果您的模组开始变得复杂， 

---

## 应用你的补丁 
为了让你的代码真正工作，Harmony 需要被告知完成你为它设置的所有工作。  这样做的一个地方是你的 Mod 的 Start 方法。 
为此，请确保您的文件已经 <code class="lang-csharp">using HarmonyLib</code>;
然后，您需要为您的补丁集创建一个带有 id 的新 Harmony 实例。  id 的常见结构是"com.company.project.product"或对我们来说是"com.User.Project.Feature" 
然后我们告诉 Harmony 实例修补它可以在我们的代码中找到的所有内容。  在旧版本中，您需要告诉 Harmony 查找当前正在运行的游戏 <code class="lang-csharp">Assembly.GetExecutingAssembly()</code>尝试修补它，但这些天它会默认出现在那里。 
```Csharp
   public void Start()
  {
      MyInput.Keybinds.Add("Alt", new Keybind("alt", KeyCode.LeftAlt, KeyCode.RightAlt));

      harmonyInstance = new Harmony("com.Soggylithe.IgnoreBuildCollision");
      harmonyInstance.PatchAll(Assembly.GetExecutingAssembly());

      Debug.Log("Place Anywhere successfully loaded! - Default-AltKey");
  } 
```
请记住，您为加载 mod 所做的任何事情都需要在卸载时撤消，以防止出现奇怪的错误并保持客户端稳定。  这也意味着您需要告诉 Harmony 实例取消修补您更改的内容。
```Csharp
  public void OnModUnload()
  {
      MyInput.Keybinds.Remove("Alt");

      harmonyInstance.UnpatchAll(harmonyInstance.Id);
      Destroy(gameObject);
      Debug.Log("Mod Brine has been unloaded!");
  }
  ```
在旧版本，您需要在卸载时 <code class="lang-csharp">Destroy()</code>模组对象。 
撰写者： Soggylithe 2020 年 10 月 31 日 


