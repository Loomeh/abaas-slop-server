# Client Configuration

Config file: `abaas.ini`  

The default config looks like this:

```

; Configure ABaaS service
[abaas]
Enable=1

ServerHost=
ServerPort=80

NetworkAsLocal=0

GsServerHost=

AllnetOverride=auto

StoreName=
StoreNickname=
RegionName=
LocationId=0

KeychipId=auto

CountryCode=

LocalIp=

ApmDll=

Log=1

CrashLog=1


; Optional game-specific patches to improve compatibility
[patches]
GgstAllnetStatus=off
GgstButtonFix=0
GgstForceLinkSetup=0
GgstDebug=0
```

Here's a table explaining what each command does:

| Setting | Default | Meaning |
|---|---|---|
| `Enable` | `1` | Master switch. `0` = leave the game untouched |
| `ServerHost` | empty | The server's address, IP or host name (required). Avoid names containing `test-` (GGST treats them as a development server) |
| `ServerPort` | `80` | Matching API port (the server's `http_port`); patches the game's `:80` |
| `NetworkAsLocal` | `0` | `1` = this cabinet only matches cabinets at its own location, NETWORK mode included. |
| `GsServerHost` | empty | Value for `AllnetAuth_getAbaasGsServerName` (unused by GGST/UNI as far as known). Empty = apm.dll's |
| `AllnetOverride` | `auto` | ALL.Net authentication result (`AllnetAuth_isGood`): `auto` = always succeeded (needed under emulators), `off` = the real result |
| `StoreName` | empty | Store name shown in game. Empty = apm.dll's |
| `StoreNickname` | empty | Store nickname shown in game. Empty = apm.dll's |
| `RegionName` | empty | Region name shown in game. Empty = apm.dll's |
| `LocationId` | `0` | ALL.Net location id; the same number for every cabinet in a store. `0` = derive from `StoreName`/`StoreNickname`, else apm.dll's. If all are 0 the server groups by public IP |
| `KeychipId` | `auto` | Keychip reported to the game and sent as the cabinet's serial. `auto` = unique per machine (the `ABAAS_KEYCHIP` environment variable varies it per copy). Every cabinet needs its own |
| `CountryCode` | empty | 3-letter country code. Empty = apm.dll's (fixed if broken, as with JConfig), else `JPN` |
| `LocalIp` | empty | This cabinet's IPv4 for in-store matching. Empty = apm.dll's if valid, else the adapter that reaches `ServerHost` |
| `ApmDll` | empty | Load another apm.dll (e.g. JConfig) beside the game's own, path relative to the ini. Currently only needed for UNI as JConfig crashes that game |
| `Log` | `1` | Write `abaas.log` next to the DLL |
| `CrashLog` | `1` | Log crashes (faulting address, call chain) to `abaas.log` |

Here's what each game patch does:

| Setting | Default | Meaning |
|---|---|---|
| `GgstAllnetStatus` | `off` | Network icon: `online` = always Online (emulators show Warning otherwise), `warning`, `off` = the real status |
| `GgstForceLinkSetup` | `0` | Set up ABaaS even when ALL.Net reports Offline |
| `GgstDebug` | `0` | Copy the ABaaS client's own debug messages into `abaas.log` (`1`–`3` = more detail) |
| `GgstButtonFix` | `0` | JConfig: Buttons 1–8 map straight to game buttons 1–8 (JConfig otherwise sends 1–6 to 1,2,3,5,6,9 and drops 7–8). JConfig v1.3 and the 2025 build, via `ApmDll` or as `apm.dll` |
