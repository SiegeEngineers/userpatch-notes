# Modding notes
Interesting bits and bobs for mods: features you can toggle, new things you can do, etc.

See [Installation notes](./installation.md) for the modding related flags in the installer.

## Town Center Pack command

UserPatch 1.4 completely disabled the unused TC Pack command. It can be reenabled by applying a patch to the exe as described [in the referenced post](#source-1).

**References**

1. <a id="source-1" href="https://forums.aiscripters.com/viewtopic.php?f=3&t=2318&p=61658#p61658">https://forums.aiscripters.com/viewtopic.php?f=3&t=2318&p=61658#p61658</a>

   <details>
   <summary>scripter64 wrote:</summary>

   This update prevents the pack command from packing town centers, which could
   sometimes improperly happen especially with multiplayer lag. If a mod
   developer wishes to undo this fix, please change 0x00127125 (file space)
   to 8B0DA0127900.

   </details>
