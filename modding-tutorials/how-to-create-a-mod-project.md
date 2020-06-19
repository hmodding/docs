---
description: >-
  This Tutorial aims to show you how to create a mod project on Raft using
  RaftModLoader!
---

# How to create a mod project

Let's get started with the requirements!  
  
To get started modding you will need the following softwares :

* \*\*\*\*[**Visual Studio Community**](https://visualstudio.microsoft.com/downloads/). We highly recommend you to download the 2019 version.
* \*\*\*\*[**Unity 2018.3.5f1**](https://unity3d.com/unity/whats-new/2018.3.5). ****If you have [UnityHub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe) installed simply click [here](http://ftp.raftmodding.com:7000/downloadRaftUnityVersion.php).
* \*\*\*\*[**dnSpy**](https://github.com/0xd4d/dnSpy/releases/latest). This is the latest available version.

{% hint style="danger" %}
**You have to use Unity 2018.3.5f1! Using another version is not supported!**
{% endhint %}



Now that you have the required softwares, let's create your mod project!

**1\)** Open the RMLLauncher and create a mod project by using the mod creator as shown below.  
 ![](../.gitbook/assets/1.png) ![](../.gitbook/assets/2%20%281%29.png) 

**2\)** In the project name text field enter your mod name and hit **Create Project!** It will then tells you if it has succeeded and opens you the project folder as shown below.

![](../.gitbook/assets/3.png)

**3\)** Now that your mod project is created you can open the **.sln** file, in the case above i open **ExampleMod.sln** with Visual Studio.  
After opening it, open the **.cs** file as shown below.

![](../.gitbook/assets/4%20%281%29.png)

**4\)** Now that we created our mod project and opened it we can begin creating our mod!  
As you can see on the screenshot above, there is a lot of green lines, those are comments, read them to know what every line do; After reading them and modifying the lines according to your mod you should end up with something like the screenshot below.

![](../.gitbook/assets/4%20%281%29.png)

**5\)** Now, let's create a shortcut of this file in our mods folder so we won't have to move the file each time we modify something in our mod. Yeah this is a great feature! 😁

{% hint style="info" %}
The file is located in _**Documents\RModLoaderDev\YourProjectName\YourProjectName**_
{% endhint %}

![](../.gitbook/assets/6.png)

**6\)** Now, let's start the mod loader, Load our mod and open the console using F10 to see what's happening. 

![](../.gitbook/assets/7%20%281%29.png)

Just after loading the mod. The status change to green and says "Running...". Your mod is now running!

![](../.gitbook/assets/9.png)

As you can see when you press F10 it executes the code in the Start\(\) method.

**And here is it! You made your first mod project!  
Keep in mind this tutorial is just the requirements & basics!  
More advanced tutorials are coming soon!**

