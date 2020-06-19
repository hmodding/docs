# Get the current SteamID

To retrieve the current user steamid we use the `GetSteamID` method from the `SteamUser` class.  
We also need to use the `Steamworks`  namespace by adding `using Steamworks;` at the top of your class.

```csharp
using Steamworks;

CSteamID steamid = SteamUser.GetSteamID();
```

