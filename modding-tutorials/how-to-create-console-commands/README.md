# 如何创建控制台命令 
###### 本教程旨在向您展示如何使用新的控制台属性创建自定义控制台命令。 

---

控制台命令是方法属性，这意味着您只需添加 [ConsoleCommand(name: "...", docs:"...")]或者 [ConsoleCommand("...","...")]上面你的方法。 
 参数是  name 命令名称本身。docs 参数是命令描述 
>[!NOTE]
>您的方法不需要是公开的，但它需要是静态的
 
如果您想在命令运行时获取命令的参数，您可以简单地添加一个名为的字符串 [] 参数 args如下所示。 
   
    [ConsoleCommand(name: "useful", docs: "this command is useful.")]
    public static void MyCommand(string[] args)
    {
        Debug.Log("You entered " + args.Length + " arguments!");
    }

该方法还可以具有字符串返回类型，并将显示返回值，如下所示。

    [ConsoleCommand(name: "useful", docs: "this command is useful.")]
    public static string MyCommand(string[] args)
    {
        return "You entered " + args.Length + " arguments!";
    }

如上所述，命令需要是静态的，因此为了访问您的非静态变量，您可以使用 mod 实例，如下所示。 

    public string myNonStaticVariable = "some stuff";
    public MyModType instance;

    void Start(){
        instance = this;
    }

    [ConsoleCommand(name: "useful", docs: "this command is useful.")]
    public static string MyCommand(string[] args)
    {
        return "My Variable : " + instance.myNonStaticVariable;
    }
    
>[!NOTE]
>卸载模块时，命令会自动注册和自动注销。