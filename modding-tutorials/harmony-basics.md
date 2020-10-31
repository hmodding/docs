
# Harmoy Basics
```csharp
using HarmonyLib;
```
### Adding to Code
To make your code work with existing mechaincs many times you will need to add your own code to a built in object. This can also let your mod act differently depending on if the player has specific other mods installed as well such as modifying recipes to use mod items if available or use vanilla items otherwise.

Harmony lets you do this by creating Patches. There are a few valid ways to set up your patches but a reliable one is to use attributes.

Each Patch will be its own class like so:
```csharp
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
With one or more static methods that add or act on the target code.

{% hint style="warning" %} Remember, your methods need to be static! {% endhint %}

The class attribute lets Harmony know "what" to use the code on. typeof(BlockCreator) means it is trying to change how one of the methods of the BlockCreator works. That method is CanBuildBlock() and Harmony searches for it by string so make sure it matches the method name!

The method attribute lets Harmony know "when" to use the code in the patch.  There are [HarmonyPrepare], [HarmonyPrefix], [HarmonyPostfix], [HarmonyTranspiler], and [HarmonyTargetMethod] each executing at a different time or in a different way.

--- 
###### [HarmonyPrepare] 
This lets you chech some things before the patch takes place to see if you even want to change something.

This method expects you to return  `false` to indicate Harmony should skip this patch.

```csharp
	[HarmonyPrepare]
	static bool IsBoxOpeningChanged()
	{
		//Look at all methods that got patched.
		foreach(MethodBase method in Harmony.GetAllPatchedMethods()){
			//See if the one you need has been modified
			if(method.Name == "OpenBox")
				//Skip patch because another mod changed something you use
				return false;
		}
		return true;
	}

```

---
###### [HarmonyPrefix]
This code takes place just before the original method happens and can be used to skip it all together. 

This means you can replace a method entirely by having a Prefix do stuff then skip the original method by returning `false`

If the original method needed to return some value the patched method can accept the `ref "Type" __result` arguement by reference. This will make it look like the original method is returning whatever `__result` is set to
```csharp
	[HarmonyPrefix]
	static bool AlwaysReturnTrue(ref bool __result)
	{
		//Set return value to true
		__result = true;
		//Tell Harmony to not run the original method
		return false;
	}
```

You can also use this method to store values that can be accessed in the Postfix if you have one via `__state`. You can use `ref` or `out` to set the `__state` to whatever value or type you want. If you need to store more data then you need to make your own object to pass on to the Postfix

```csharp
	static void Prefix(out Stopwatch __state)
    {
        __state = new Stopwatch(); // Stopwatch is a custom timer class
        __state.Start();
    }

    static void Postfix(Stopwatch __state)
    {
        __state.Stop();
        FileLog.Log(__state.Elapsed.ToString());
    }
```



---
###### [HarmonyPostfix]
This code takes place right after the original method. It can take in the `__result` from the original method and use it or even change it. 

One benefit of Postfixs is that they always run. No matter where the original method escaped its code from it will go to the Postfix right after. 

{% hint style="info" %} One quirk of Postfixs is that they can either change `__result` by ref or can return a value that would work the same way, but if they return a result it has to be the same Type as whatever the first arguement in the method is! {% endhint %}

```csharp
	[HarmonyPostfix]
	static BuildError CheckForAlt(BuildError __result, BlockCreator __instance)
	{...}
```

---
###### [HarmonyTranspiler]
Transpilers are a much more advanced way to get your own code to run. It is a bit out of the scope of a tutorial here so I will link to [this article](https://harmony.pardeike.net/articles/patching-transpiler.html#basic-transpiler-tutorial)

To give a simplified overview: Instead of writing code to be executed, you are looking at all of the IL instructions that a chunk of code represents and doing some of your own instructions whenever you think the right time is. 

---
###### [HarmonyTargetMethod]
I'll be honest I don't understand this one and works other than it is a different way to tell Harmony which method you are changing instead of using the class attribute

---
### Valid Patch Rules
Your patch methods can accept a variety of arguements that let you peek into the code that is running or is about to run. 

Each prefix and postfix can get all the parameters of the original method as well as the instance (if original method is not static) and the return value. In order to patch a method your patches need to follow the following principles when defining them:

- A patch must be a static method
- A prefix patch has a return type of void or bool
- A postfix patch has a return type of void or the return signature must match the type of the first parameter (passthrough mode)
- Patches can use a parameter named `__instance` to access the instance value if original method is not static
- Patches can use a parameter named `__result` to access the returned value (prefixes get default value)
- Patches can use a parameter named` __state` to store information in the prefix method that can be accessed again in the postfix method. Think of it as a local variable. It can be any type and you are responsible to initialize its value in the prefix
- Parameter names starting with three underscores, for example `___someField`, can be used to read and write (with 'ref') private fields on the instance that has the same name (minus the underscores)
- Patches can define only those parameters they want to access (no need to define all)
- Patch parameters must use the exact same name and type as the original method (object is ok too)
- Patches can either get parameters normally or by declaring any parameter ref (for manipulation)
- To allow patch reusing, one can inject the original method by using a parameter named `__originalMethod`

{% hint style="info" %} Using three underscores `___likeSo` to read variables is quite usefull {% endhint %}

Transpilers have some other optional parameters:

- A parameter of type ILGenerator that will be set to the current IL code generator
- A parameter of type MethodBase that will be set to the current original method being patched
- They must contain one parameter of type `IEnumerable<CodeInstruction>` that will be used to pass the IL codes to it


{% hint style="info" %} There are other ways to combine attributes and structure your patches. Try checking [here](https://harmony.pardeike.net/articles/annotations.html#combining-annotations) if your mod starts getting complicated. {% endhint %}

---
### Applying your patches
To get your code actually working Harmony needs to be told to do all the work you have set up for it. One place to do that from is Start method of your Mod.

To do this make sure your file is already `using HarmonyLib;`

Then you need to create a new instance of Harmony with an id for your set of patches. A common structure for an id is "com.company.project.product" or for us "com.User.Project.Feature"

Then we tell the Harmony instance to patch everything it can find in our code. In older versions you need to tell Harmony to look for the currently running game with `Assembly.GetExecutingAssembly()` to try and patch it but these days it will look there by default.

```csharp
public void Start()
    {
        MyInput.Keybinds.Add("Alt", new Keybind("alt", KeyCode.LeftAlt, KeyCode.RightAlt));

        harmonyInstance = new Harmony("com.Soggylithe.IgnoreBuildCollision");
        harmonyInstance.PatchAll(Assembly.GetExecutingAssembly());

        Debug.Log("Place Anywhere successfully loaded! - Default-AltKey");
    }
```

Remember, anything you do to load your mod you need to undo when you unload it to prevent strange errors and to keep the client stable. This also means you need to tell your Harmony instance to unpatch the things you changed.

```csharp
public void OnModUnload()
    {
        MyInput.Keybinds.Remove("Alt");

        harmonyInstance.UnpatchAll();
        Destroy(gameObject);
        Debug.Log("Mod Brine has been unloaded!");
    }
```

In older versions you needed to `Destroy()` the mod object when unloading but that is also handled gracefully these days.

## Traversing
