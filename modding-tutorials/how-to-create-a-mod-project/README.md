#如何创建一个模组项目 
本教程旨在向您展示如何使用 RaftModLoader 在 Raft 上创建一个 mod 项目！ 
---



让我们从需求开始吧！  要开始改装，您需要以下软件：‌ 
​
- [Visual Studio Community](https://visualstudio.microsoft.com/downloads/)我们强烈建议您下载 2019 版本。 
- 如果您安装了[Unity 2019.3.5f1](https://unity3d.com/fr/unity/whats-new/2019.3.5)，只需单击 
- [dnspy](https://github.com/0xd4d/dnSpy/releases/latest)这是最新的可用版本。
   
> [!WARNING]
>您必须使用 Unity 2019.3.5f1！  不支持使用其他版本！ 

现在您已经拥有了所需的软件，让我们创建您的模组项目吧！‌ 
1) 打开 RMLLLauncher 并使用 mod creator 创建一个 mod 项目，如下所示。 
2) 在项目名称文本字段中输入您的模组名称并点击 创建项目！   然后它会告诉您是否成功并打开项目文件夹，如下所示。‌ 
3) 现在您的 mod 项目已创建，您可以打开 .sln 文件，在上述情况下，我 打开ExampleMod.sln 使用 Visual Studio  。  打开后，打开 .cs 文件，如下图。‌ 
4) 现在我们创建了我们的 mod 项目并打开它，我们可以开始创建我们的 mod！   正如您在上面的屏幕截图中看到的，有很多绿线，那些是注释，阅读它们以了解每行的作用。   现在，让我们在 mods 文件夹中创建项目文件夹的快捷方式，这样我们就不必在每次修改 mod 中的内容时移动文件或构建 mod。   是的，这是一个很棒的功能！   😁 
该文件夹默认位于 Documents\RaftModding\YourProjectName\YourProjectName 
创建此文件夹的快捷方式。   它的文件夹包含 modinfo.json文件和 .cs文件。    主文件夹也应该可以使用，但我们强烈建议您创建第二个文件夹的快捷方式。   ‌ 
6) 现在，让我们启动 mod loader，加载我们的 mod 并使用 F10 打开控制台，看看发生了什么。‌ 
就在加载mod之后。  状态变为绿色并显示“正在运行...”。  你的模组现在正在运行！‌ 
正如您所看到的，当您按下 F10 时，它会执行 Start() 方法中的代码。‌ 
7) 现在您知道如何编写代码，要修改您的模组信息，例如其描述、许可证、图标、横幅等，只需进入您的模组项目文件并编辑 modinfo.json 文件，如下所示。‌ 
如果您想了解有关此文件的更多信息、如何添加图标、什么是 excludeFiles 字段等，请单击下面。 
  
8) 现在您知道如何编写您的 mod 以及如何更改其信息来构建它，只需生成 Visual Studio 项目，我们的构建脚本将自动打包/构建您的 mod，如下所示。   构建脚本将生成一个名为 YourModName.rmod该文件是要上传到我们 
并放入 mods文件夹的 mod 文件。‌ 
就在这里！  你做了你的第一个模组项目！  请记住，本教程只是要求和基础知识！  更多高级教程即将推出！ 