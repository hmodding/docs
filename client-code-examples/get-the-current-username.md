# Get the current username

To retrieve the current user name we use the `GetPersonaName` method from the `SteamFriends` class.  
We also need to use the `Steamworks`  namespace by adding `using Steamworks;` at the top of your class.

```csharp
using Steamworks;

string username = SteamFriends.GetPersonaName();
```

