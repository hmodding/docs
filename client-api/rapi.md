# RAPI

Get the player username from a SteamID.

{% tabs %}
{% tab title="Method" %}
```csharp
string GetUsernameFromSteamID(CSteamID steamid)
```
{% endtab %}

{% tab title="Example" %}
```csharp
string username = RAPI.GetUsernameFromSteamID(playersteamid);
RConsole.Log("The player name is " + username);
```
{% endtab %}
{% endtabs %}

Toggle the mouse cursor.

{% tabs %}
{% tab title="Method" %}
```csharp
void ToggleCursor(bool var)
```
{% endtab %}

{% tab title="Example" %}
```csharp
RAPI.ToggleCursor(true); // This will show the cursor.
RAPI.ToggleCursor(false); // This will hide the cursor.
```
{% endtab %}
{% endtabs %}

Register a new item.

{% tabs %}
{% tab title="Method" %}
```csharp
void RegisterItem(Item_Base item)
```
{% endtab %}

{% tab title="Example" %}
```csharp
// Here we load the item from the assetbundle
Item_Base yournewitem = asset.LoadAsset<Item_Base>("yournewitem");
// Then we register it using RegisterItem()
RAPI.RegisterItem(yournewitem);
```
{% endtab %}
{% endtabs %}

Set an item prefab.

{% tabs %}
{% tab title="Method" %}
```csharp
void SetItemObject(Item_Base item, GameObject prefab)
// The default hand is right. RItemHand can be leftHand or rightHand.
void SetItemObject(Item_Base item, GameObject prefab, RItemHand parent)
```
{% endtab %}

{% tab title="Example" %}
```csharp
// Here we load the item from the assetbundle
Item_Base yournewitem = asset.LoadAsset<Item_Base>("yournewitem");
// Then we register it using RegisterItem()
RAPI.RegisterItem(yournewitem);
// Then we set his prefab by loading it from the assetbundle
RAPI.SetItemObject(yournewitem, asset.LoadAsset<GameObject>("yournewitem"));
```
{% endtab %}
{% endtabs %}

Gets the local Network\_Player script.

{% tabs %}
{% tab title="Method" %}
```csharp
Network_Player GetLocalPlayer()
```
{% endtab %}

{% tab title="Example" %}
```csharp
Network_Player player = RAPI.GetLocalPlayer();
// The player variable will contain the local player script.
```
{% endtab %}
{% endtabs %}

Give an item to the local player.

{% tabs %}
{% tab title="Method" %}
```csharp
void GiveItem(Item_Base item, int amount)
```
{% endtab %}

{% tab title="Example" %}
```csharp
RAPI.GiveItem(ItemManager.GetItemByName("raw_potato"),10);
// This will give 10 raw potatoes to the local player.
```
{% endtab %}
{% endtabs %}

Broadcast a chat message.

{% tabs %}
{% tab title="Method" %}
```csharp
void BroadcastChatMessage(string message)
```
{% endtab %}

{% tab title="Example" %}
```
RAPI.BroadcastChatMessage("I'm a message");
// Will send "I'm a message" to every players.
```
{% endtab %}
{% endtabs %}

Allow the placement of a block on the grid system.

{% tabs %}
{% tab title="Method" %}
```csharp
void AddItemToBlockQuadType(Item_Base item, RBlockQuadType quadtype)
```
{% endtab %}

{% tab title="Example" %}
```csharp
RAPI.AddItemToBlockQuadType(YourNewItem, RBlockQuadType.quad_foundation);
// Allow "YourNewItem" to be placed on foundations.
```
{% endtab %}
{% endtabs %}

Disallow the placement of a block on the grid system.

{% tabs %}
{% tab title="Method" %}
```csharp
void RemoveItemFromBlockQuadType(string itemUniqueName, RBlockQuadType quadtype)
```
{% endtab %}

{% tab title="Example" %}
```csharp
RAPI.RemoveItemFromBlockQuadType("YourItemUniqueName", RBlockQuadType.quad_foundation);
// Disallow the item with the uniquename "YourItemUniqueName" to be placed on foundations.

```
{% endtab %}
{% endtabs %}

Listen for network messages on a specific network channel. \(0 and 1 are locked for the game and can't be used\)

{% tabs %}
{% tab title="Method" %}
```csharp
NetworkMessage ListenForNetworkMessagesOnChannel(int channel = 0)
```
{% endtab %}

{% tab title="Example" %}
```csharp
// Choose a unique ID for the channel id to not interfer with other mods.
NetworkMessage netMessage = RAPI.ListenForNetworkMessagesOnChannel(30);
if (netMessage != null)
{
  CSteamID id = netMessage.steamid;
  Message message = netMessage.message;
  // Here we use 5000 because we can't modify an enum, you can use any values 
  // as long as its not in the Messages enum already. Bigger than 1000 is perfect.
  if(message.Type == (Messages)5000){
    // Do your stuff with the message now that you know 
    // its yours and its the wanted type.
    YourMessageClass msg = message as YourMessageClass;
  }
}
```
{% endtab %}
{% endtabs %}

Send a network message to all players.

{% tabs %}
{% tab title="Method" %}
```csharp
void SendNetworkMessage(Message message, int channel = 0, EP2PSend ep2psend = EP2PSend.k_EP2PSendReliable, Target target = Target.Other, CSteamID fallbackSteamID = new CSteamID())
```
{% endtab %}

{% tab title="Example" %}
```csharp
// This will send your network message to all players.
RAPI.SendNetworkMessage(new YourMessageClass((Messages)5000));
```
{% endtab %}
{% endtabs %}

