# Random Map Scripting Notes
This is the documentation from ZipRms.txt in the UserPatch reference.

## Zip Random Map Script Notes
To create a valid zip-rms (ZR) file, please note the following:
- you must include 1 valid rms text file
- you can include 1 scx file if this will be a Real World Map
- you can include any terrain override slps between 15000.slp and 15049.slp
- add all of these files into a zip file with no compression (store)
- there should be no folder paths for files within the zip file structure
- name the zip file `ZR@YourMapName.rms` (change extension from .zip to .rms)
- the ZR@ prefix is required for the game to load a zip-rms file
- place the new rms file into your Random folder (or Script.RM for expansions)
- select it from the Custom map list and play the game as usual

To generate the rms zip with the 7zip command line tool, 7z.exe:
- `7z.exe a -mx0 -tzip "ZR@YourMapNameHere.rms" your-map.rms your-map.scx 15000.slp 15002.slp`

## Custom Real World Map Notes
To create a real world map zip-rms, please note the following:
- the rms file should not have `<LAND_GENERATION>` unless `direct_placement` is used
- the `direct_placement` system is provided by default for all custom real world maps
- if `direct_placement` is used, please see below for `<LAND_GENERATION>` details
- the scx file should have 1 town-center per player to set starting positions
- you can only generate the rms part of ZR@ maps in the scenario editor
- for King of the Hill, you can place a gaia monument on the scx map
- trees and other gaia objects like gold can be placed on the scx map
- other than town-centers, no player objects should be placed on the scx map
- players are placed randomly if the Team Together option is unchecked
- players are placed sequentially (first is random) with Team Together checked
- you must use Team Together if less than 8 starting positions are defined

To have proper team placement for 1v1, 2v2, 3v3, or 4v4 games:
- enable Team Together to place players sequentially (first location is random)
- alternate team numbers on the setup screen: 1, 2, 1, 2, 1, 2, 1, 2
- to support all team setups, place players in the scx like this:

  ```
      3
    5   7
  1       2
    8   6
      4
  ```

## Direct Placement Notes
To use `direct_placement` in the rms of a real world map:
- include a `<LAND_GENERATION>` section in your rms file
- do not include a town-center for each player in your scx file
- under `<LAND_GENERATION>`, add this: `base_terrain REAL_TERRAIN`
- use `assign_to_player` and `land_position` to place each player with `create_land`
- use `X_PLAYER_GAME` consts like `8_PLAYER_GAME` with `if` to adjust the layout

```
#const REAL_TERRAIN -1

<PLAYER_SETUP>
	direct_placement
	ai_info_map_type BLACK_FOREST 0 0 0

<LAND_GENERATION>
	base_terrain REAL_TERRAIN

if 2_PLAYER_GAME
	create_land
	{
		land_percent 2
		base_size 6
		assign_to_player 1
		land_position 25 25 /* toward the left of the map */
	}
	create_land
	{
		land_percent 2
		base_size 6
		assign_to_player 2
		land_position 75 75 /* toward the right of the map */
	}
endif
```
