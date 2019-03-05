# Installation notes

## Installer flags

With `SetupAoC.exe -i:FLAGS`, you can programmatically specify which features to install. The `FLAGS` part contains 0/1 flags for disable/enable, or 2 to use the default (or already-existing when installing UP onto an existing UP version).

The order of flags is, left to right:

1. Use widescreen command bar style
2. Enable window mode (does not work in Wine)
3. Enable automatic port forwarding with upnp (does not work in Wine and may cause issues when playing on LAN)

---

4. Use alternate red minimap colour
5. Use alternate purple minimap colour
6. Use alternate gray minimap colour
7. Extend population caps (up to 1k)
8. Replace snowy terrains with grass
9. Enable water animation
10. Enable precision scrolling, instead of half-tile scrolling in plain AoC
11. Enable appending to unit groups when holding Shift
12. Activate hotkeys on keydown

---

13. Use new savegame file name format `rec.yyyy-mm-dd-hh-mm-ss.mgz`
14. Enable building multi-queue
15. Use original patrol delay
16. Disable water movement
17. Disable weather
18. Disable custom terrains
19. Disable underwater terrain
20. Enable numeric age display in the game score area ('1' instead of 'Dark Age')
21. Enable touch-screen support
22. Store spec addresses [TODO what?]
23. Use system default mouse cursor

---

25. Delink in-game volume from system volume, the Master Volume in-game slider will not affect your system volume
26. Enable custom chatbox (for Wine)
27. Use low quality environment
28. Use low 1.0c framerate in singleplayer
29. Disable extended hotkeys
30. Force enable gameplay features like MQ [TODO which exactly?]
31. Display the 5th 'ore' resource in the top bar
32. Disable multiplayer anticheat
33. Set default background mode [TODO what?]
34. Use slow multiplayer speed in single player too
35. Enable scenario/rms debug logging
36. Statistics font style [TODO what?]
37. Background audio playback [TODO what?]
38. Disable civilian attack switch, for game mods that add custom civilian units. Prevents an issue where they turn into standard villagers in some situations
39. Handle small farm selections, for mods that have 2x2 farms
40. Enable research events in chat in spec mode
