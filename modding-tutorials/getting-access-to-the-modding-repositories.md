# Getting access to the modding repositories

In order to make a mod, it is essential to understand how the game works under the hood. Therefore, you can use tools like [dnSpy](https://github.com/dnSpy/dnSpy) \(to decompile the game's code\) or [uTinyRipper ](https://github.com/mafaca/UtinyRipper)\(to export the game's assets\). However, the results of those tools are usually not ideal and need to be fixed up in some places.

That's why we are uploading the exported contents of the game for every update to repositories at gitlab.com.

### What's contained in the repositories?

There are two repositories available:

* The [raft-code](https://gitlab.com/traxam/raft-code) repository contains the decompiled code of the game. It is not possible to compile this code and you won't be able to link your mods agains this code. The purpose of the repository is to provide a "reference" decompilation so that we can talk about certain line numbers in the original code \(i.e. by linking to a certain line number in a certain file via the GitLab web frontend\). GitLab also provides a useful search function.
* The [raft-unity-project](https://gitlab.com/traxam/raft-unity-project) repository contains an optimized export of the game's unity assets. You can open the repository with a specific \(!\) version of the Unity Editor to take a look at Raft's textures, prefabs, scenes, etc. and you will need to use the Unity Editor with this repository if you want to [create AssetBundles](how-to-create-an-assetbundle.md) to add new content to the game. The raft-unity-project contains stubbed scripts \(that means that only properties and fields of the scripts are available and methods are not\) to avoid Unity complaining about compilation errors.

{% hint style="danger" %}
You can neither compile the raft-code nor the raft-unity-project into a working game!
{% endhint %}

### Why are the repositories access-controlled?

You might have already found out that the repositories are not available to the public. This has multiple reasons:

1. We do not want to share Raft's code with anyone. The content of the repositories is none of our main tasks here at RaftModding. We do provide them but we don't want to put focus on them.
2. We are not allowed to share Raft's code with anyone. Raft is a game that you need to pay for. Its source code is copyrighted and we don't want to get into legal trouble for distributing the game's source code. In fact, if Redbeet \(the game studio that made Raft\) told us to stop hosting these repositories, we would do so immediately!

### Who can access the repositories?

We will only provide access to people who already own the game. This is because everyone who owns the game is also able to decompile it and extract its assets on their own, we're only making it easier. 

More specifically, access is only provided to you if you meet the following criteria:

1. You need to own the game on Steam and verify with us that you do.
2. You need to have a GitLab account.

{% hint style="warning" %}
If you have bought the game from some other place than Steam or can't verify with us that you own the game on Steam, we will not give you access.
{% endhint %}

### What am I allowed to do with the repository contents?

Other than the mod loader and website themselves, the Raft code and assets are not owned by us. They are owned by Redbeet Interactive and/or Axolot Games and provided to you with the license that you obtained when you bought the game.

In the context of modding, this especially means the following:

* Do **not** distribute these sources!
* You can use the sources to build your mod but you may not include protected materials in the files you upload.

{% hint style="info" %}
We're no law experts and this is no real legal advice. You are responsible for your own actions!
{% endhint %}

### How do I get access?

For the purpose of verification, the process of getting access is a bit tedious.

1. In your Steam profile, make sure that the list of your owned games is visible to everyone. We need to be able to see your games list to verify that you own Raft.
2. Send a direct message to `@TeK#6884` or `@traxam#7012` via Discord and tell us that you want access to the Raft repositories. Make sure to include a link to your Steam profile as well as your GitLab.com username or email address! \(alternatively, you can contact us via [mail](mailto:traxam@raftmodding.com)\)
3. We will check your Steam profile and send you a response as soon as possible. Please keep in mind that for us, RaftModding is a free-time project and it might take a while until we answer.
4. In some cases, we will ask for additional proof to make sure that the profiles are really from you.
5. When we've accepted your verification, we will send you another message and add you to a GitLab group that will allow you to access the repositories. You can find the links to the repos at the [top of this article](getting-access-to-the-modding-repositories.md#whats-contained-in-the-repositories). 

Happy modding!

