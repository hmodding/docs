---
description: >-
  This tutorial aims to show you how to embed/merge an asset bundle into your
  .cs mod file.
---

# How to embed an assetbundle

Let's get started with the requirements!   
For this tutorial you will need the same requirements as the first tutorial.

**1\)** Upload your .assets file to [**RaftModdingTools**](https://www.raftmodding.com/rmtools/) and copy the code snippet it gives you as shown below.

![If your assetbundle is bigger than 1MB it can take some time to copy, Just wait until it finish.](../.gitbook/assets/capture%20%282%29.png)

**2\)** Paste the copied code at the bottom of your mod file as shown below. 

![If your assetbundle is bigger than 1MB it can take some time, Just wait until its pasted.](../.gitbook/assets/1%20%282%29.png)

**3\)** Now that your asset bundle is in your mod file we need to load it, its a little bit different than the [**How to create an AssetBundle**](https://api.raftmodding.com/modding-tutorials/how-to-create-an-assetbundle) tutorial.  
Simply use that code below, as you can see instead of loading the asset using a file we load it using **`AssetBundle.LoadFromMemoryAsync(Convert.FromBase64String(embeddedAssetBundle.data))`** as shown below.

{% tabs %}
{% tab title="Help Image" %}
![](../.gitbook/assets/3%20%281%29.png)
{% endtab %}

{% tab title="Code" %}
```csharp
AssetBundle asset;

public IEnumerator Start()
{
    AssetBundleCreateRequest request = AssetBundle.LoadFromMemoryAsync(Convert.FromBase64String(embeddedAssetBundle.data));
    yield return request;
    AssetBundle asset = request.assetBundle;
}

```
{% endtab %}
{% endtabs %}

**And here you go! Your assetbundle should load just fine and faster!** 😉 

