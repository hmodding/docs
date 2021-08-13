---
description: >-
  This page will explain you how to get rid of multiple error on RMLLauncher you
  can have because of false positives
---

# How to configure your antivirus

## 0. Why is my antivirus suspecting RMLLauncher to be a Trojan/Malware ?

RMLLauncher is injecting code \(mods\) in a running process \(the game\), and this is a very suspicious action. As a mod launcher we need to do this, but it's not a "normal behavior" for a software. Then your antivirus is flagging it as a Trojan or a malware that try to hide itself in others applications.  
If you are worried and not sure if you want to download it because of that, you can go on the[ RaftModding discord server](https://discord.gg/r83T8Q) or on the [official raft server](https://discord.gg/raft) to ask on the modding area what others user thinks of RML.

{% hint style="success" %}
Actually the RMLLauncher and Core are [open-sourced](https://gitlab.com/TeKGameR950/HyTeKModLoader), then it's analysed by users of this launcher and modders that have the knowledge to analyse it.  
If no one showed it's a virus, it's mainly because it's not, so don't worry 🙂
{% endhint %}

## 1. Find which anti-virus you have \(you can pass this step if you already know\)

Use `Ctrl+R` then type `WSCUI.CPL`

![](../.gitbook/assets/image%20%284%29.png)

When you are on the Windows Security Center open the Security tab.

![](../.gitbook/assets/image%20%285%29.png)

If you are on an old windows 10 version, you will have the name of your antivirus here \(it's the "virus protection" row\).

![Exemple for Windows Defender antivirus](../.gitbook/assets/image%20%282%29.png)

Else, if you are on the newer windows 10 version, you will have "View in Windows Security" in the "virus protection" row, click on it and on this new window you should have your antivirus name.

![Exemple for Avast antivirus](../.gitbook/assets/image%20%283%29.png)

## 2. Add the needed exclusions in your antivirus

Now you will have to have 2 exclusions in your antivirus to avoid all the false positive for the launcher.

The two files are those ones :

* `RMLLauncher.exe`
* `C:\Users\Your user name\AppData\Roaming\RaftModLoader`

{% tabs %}
{% tab title="Windows Defender" %}
To add the exceptions in Windows Defender you can follow the next link :  
[https://support.microsoft.com/en-us/help/4028485/windows-10-add-an-exclusion-to-windows-security](https://support.microsoft.com/en-us/help/4028485/windows-10-add-an-exclusion-to-windows-security)
{% endtab %}

{% tab title="Avast" %}
To add the exceptions in Avast you can follow the next link :  
[https://support.avast.com/en-ww/article/168/](https://support.avast.com/en-ww/article/168/)
{% endtab %}

{% tab title="BitDefender" %}
To add the exceptions in BitDefender you can follow the next link :  
[https://www.bitdefender.com/consumer/support/answer/13427/](https://www.bitdefender.com/consumer/support/answer/13427/)
{% endtab %}

{% tab title="Norton" %}
1. Check out this help article by Norton to add the files listed above to the exceptions list: [https://support.norton.com/sp/en/us/home/current/solutions/v3672136](https://support.norton.com/sp/en/us/home/current/solutions/v3672136)
2. If Raft Mod Loader can't connect to the internet, you'll need to add an exception to your firewall too:
   1. Open My Norton
   2. Go to Settings -&gt; Firewall -&gt; Application Control
   3. Use the search function to find `HML`
   4. Allow network access for `HML`

_Thanks to Sly for providing instructions for Norton!_
{% endtab %}

{% tab title="Others antivirus" %}
Type on google "How to add an exclusion in " and the name of your antivirus after, then go on the website of your antivirus and follow the step for the two files above
{% endtab %}
{% endtabs %}

## 3. Try to start the game through RMLLauncher

If after that you have another error or you still get the same error, go in the \#support channel of the [Raft Modding discord server](https://discord.gg/r83T8Q) and we will be glad to help you🙂

