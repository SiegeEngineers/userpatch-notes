# Multiplayer notes

## Lobby Preset Data

When starting the game using DirectPlay Lobbying, the lobby application can configure some settings for the game room. On boot, the game sends a `DPLMSG_GETPROPERTY` message, with GUID `GUID_AGEPresetData`:

```c
// {B3F2E132-FE6A-11D2-8DEE-00A0C90832B4}
static const GUID GUID_AGEPresetData = { 0xB3F2E132, 0xFE6A, 0x11D2, { 0x8D, 0xEE, 0x00, 0xA0, 0xC9, 0x08, 0x32, 0xB4 } };
```

The lobby application can then send a `DPLMSG_GETPROPERTYRESPONSE` message to the game with the same GUID and an AGEPresetData structure.
The AGEPresetData structure has a `GameFilename` field that's used to restore games. In AoC 1.0c, it's 260 bytes. In UserPatch 1.4 and up, it's 240 bytes, and the 20 newly freed bytes are used for whitelisting up to 4 spectator IPs, and for passing in "flags" (specifically to enable Hidden Civs). <sup>[(source)](#source-1)</sup>

With that in mind, the full structure is as follows:

```c
struct AGEPresetData {
  char m_bCivAvail[20];
  char m_bLockCivAvail;
  char m_bGameType;
  char m_bLockGameType;
  union {
    char m_sGameFilename[260];
    struct {
      char m_sGameFilenameUP[240];
      DWORD m_dSpecIps[4];
      DWORD m_dReservedFlags;
    };
  };
  char m_bMapTypeAvail[39];
  char m_bLockMapTypeAvail;
  char m_bMapSize;
  char m_bLockMapSize;
  char m_bDifficulty;
  char m_bLockDifficulty;
  char m_bResources;
  char m_bLockResources;
  int m_dPopulation;
  char m_bLockPopulation;
  char m_bGamespeed;
  char m_bLockGamespeed;
  char m_bStartingAge;
  char m_bLockStartingAge;
  char m_bVictory;
  char m_bLockVictory;
  int m_bVictoryAmount;
  char m_bLockVictoryAmount;
  char m_bTeamTogether;
  char m_bLockTeamTogether;
  char m_bSetTeams;
  char m_bLockSetTeams;
  char m_bLockedGamespeed;
  char m_bLockLockedGamespeed;
  char m_bRecordGame;
  char m_bLockRecordGame;
  char m_bVisibility;
  char m_bLockVisibility;
  char m_bAllTechs;
  char m_bLockAllTechs;
  char m_bAllowCheats;
  char m_bLockAllowCheats;
  char m_bMyCiv;
  char m_bLockMyCiv;
  char m_bMyPlayernum;
  char m_bLockMyPlayernum;
  char m_bMyTeam;
  char m_bLockMyTeam;
  char m_bTeamBonuses;
  char m_bLockTeamBonuses;
  char m_bVersion;
  int m_dRandomMapSeed;
};
```

A sample implementation of this process could look like:

```cpp
// Assuming these variables are initialized:
// IDirectPlayLobby3A* dpLobby;
// DWORD dwAppId;
// DPLMSG_GETPROPERTY receivedMessage;
if (IsEqualGUID(receivedMessage->guidPropertyTag, GUID_AGEPresetData)) {
  AGEPresetData preset;
  memset(preset, 0, sizeof(preset));
  // Enable hidden civs
  preset->m_dReservedFlags |= 1;
  auto responseSize = sizeof(DPLMSG_GETPROPERTYRESPONSE) - sizeof(DWORD) + sizeof(AGEPresetData);
  auto response = static_cast<DPLMSG_GETPROPERTYRESPONSE*>(malloc(responseSize));
  response->dwType = DPLSYS_GETPROPERTYRESPONSE;
  response->dwRequestID = receivedMessage->dwRequestID;
  response->guidPlayer = receivedMessage->guidPlayer;
  response->guidPropertyTag = receivedMessage->guidPropertyTag;
  response->hr = DP_OK;
  response->dwDataSize = sizeof(AGEPresetData);
  memcpy(response->dwPropertyData, preset, sizeof(AGEPresetData));
  dpLobby->SendLobbyMessage(0, dwAppId, response, responseSize);
}
```

**References**

1. <a id="source-1" href="https://www.aoezone.net/threads/repost-userpatch-v1-4-rc-installer-preinstalled.97719/#post-321800">https://www.aoezone.net/threads/repost-userpatch-v1-4-rc-installer-preinstalled.97719/#post-321800</a>

   <details>
   <summary>scripter64 wrote:</summary>

   Lobby launch preset structure changes since 1.4:
   - the GameFilename was reduced by 20 bytes, making it 240 bytes in size instead of 260
   - this creates space for 5 new DWORD values after it:
   - the first 4 DWORD values are for allowed spectator IPs, 1 address per 4 bytes in DWORD form (default: 0)
   - the 5th DWORD value is for the ReservedFlags (default: 0)

   </details>
