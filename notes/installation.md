# Installation notes

## Installer flags

With `SetupAoC.exe -i:FLAGS`, you can programmatically specify which features to install. The `FLAGS` part contains 0/1 flags for disable/enable, or 2 to use the default (or already-existing when installing UP onto an existing UP version).

The order of flags is, left to right:

- Use widescreen command bar style
- Enable window mode (does not work in Wine)
- Enable automatic port forwarding with upnp (does not work in Wine and may cause issues when playing on LAN)

---

- Use alternate red minimap colour
- Use alternate purple minimap colour
- Use alternate gray minimap colour
- Extend population caps (up to 1k)
- Replace snowy terrains with grass
- Enable water animation
- Enable precision scrolling, instead of half-tile scrolling in plain AoC
- Enable appending to unit groups when holding Shift
- Activate hotkeys on keydown

---

- Use new savegame file name format `rec.yyyy-mm-dd-hh-mm-ss.mgz`
- Enable building multi-queue
- Use original patrol delay
- Disable water movement
- Disable weather
- Disable custom terrains
- Disable underwater terrain
- Enable numeric age display in the game score area ('1' instead of 'Dark Age')
- Enable touch-screen support
- Store spec addresses [TODO what?]
- Use system default mouse cursor

---

- Delink in-game volume from system volume, the Master Volume in-game slider will not affect your system volume
- Enable custom chatbox (for Wine)
- Use low quality environment
- Use low 1.0c framerate in singleplayer
- Disable extended hotkeys
- Force enable gameplay features like MQ [TODO which exactly?]
- Display the 5th 'ore' resource in the top bar
- Disable multiplayer anticheat
- Set default background mode [TODO what?]
- Use slow multiplayer speed in single player too
- Enable scenario/rms debug logging
- Statistics font style [TODO what?]
- Background audio playback [TODO what?]
- Disable civilian attack switch, for game mods that add custom civilian units. Prevents an issue where they turn into standard villagers in some situations
- Handle small farm selections, for mods that have 2x2 farms
- Enable research events in chat in spec mode
