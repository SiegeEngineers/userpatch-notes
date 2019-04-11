# Custom Scenario Notes
This is the documentation from CustomScenario.txt in the UserPatch reference.

## Effect Amount Notes
To use the rms `effect_amount` in an scx, please note the following:

- create a Display Instructions effect
- set Number to 9 and Timer to 99999 (minimum)
- for Message, provide a list of up-effect commands, one per line
- this only affects the specified type/class id when applied to objects
- if an object has been modified by Change effects, this has no effect
- please see UserPatchConst.rms for a list of ids and constants

`up-effect player,effect,item,attr,value,style`

- `player`: 0 for default (all) or 1-8 for a specific player
- `effect`: the id of an Effect such as `SET_ATTRIBUTE` (0)
- `item`: the id of the Item to modify such as `VILLAGER_CLASS` (904)
- `attr`: the id of the Attribute such as `ATTR_HITPOINTS` (0)
- `value`: the value to set
- `style`: 2 if value should be divided by 100; otherwise 1

Scx to Rms Equivalents:

`up-effect 0,0,904,0,100,1`
- `effect_amount SET_ATTRIBUTE VILLAGER_CLASS ATTR_HITPOINTS 100`
`up-effect 0,0,904,0,100,2`
- `effect_percent SET_ATTRIBUTE VILLAGER_CLASS ATTR_HITPOINTS 100`
`up-effect 0,1,3,1,10,1`
- `effect_amount MOD_RESOURCE AMOUNT_GOLD ATTR_ADD 10`

## Object Attribute Notes
To modify object tech attributes in an scx, please note the following:

- create a Change Object Name effect
- set Number to 1
- for Message, provide a list of up-attribute commands, one per line
- all matching objects will be modified by the up-attribute commands
- use existing Change effects whenever possible for attack, armor, etc.
- this will separate objects from normal techs like all Change effects
- this cannot modify `ATTR_LINE_OF_SIGHT` (Change Range effect can adjust LOS)
- this cannot modify `ATTR_GARRISON_ARROWS`
- this can only modify category 70+ objects (living objects and buildings)
- attributes are modified without checks, so please be careful (Set is safest)
- this can be computationally expensive, so please consider performance
- please see UserPatchConst.rms for a list of ids and constants

`up-attribute mode,attr,value,style`

- `mode`: 0 to Set the value, 1 to Add, 2 to Multiply
- `attr`: the id of the Attribute such as `ATTR_HITPOINTS` (0)
- `value`: the value to set
- `style`: 2 if value should be divided by 100; otherwise 1

Example Commands:

`up-attribute 0,50,5083,1`
- set `ATTR_NAME_ID` for matching objects to 5083 (Archer)

`up-attribute 1,5,50,2`
- add 0.5 to `ATTR_MOVE_SPEED` for matching objects

`up-attribute 0,40,4,1`
- set `ATTR_HERO_STATUS` by 4 to enable only auto-healing

## Terrain Override Notes
To override the terrains for a scenario, please note the following:

- you must create a zip file with any terrains between 15000.slp and 15049.slp
- there should be no folder paths for files within the zip file structure
- the zip file must be placed in the Scenario folder with your scx or cpx file
- you can reuse a zip file for multiple scx files, including those inside a cpx file

To set your scx to load your terrain zip file, please note the following:

- in the scenario editor, the description of the first trigger sets the zip file
- for YourScenarioNameHere.zip, the description would include the following line:
  ```
  up-terrain: YourScenarioNameHere
  ```

To generate the zip with the 7zip command line tool, 7z.exe:
- `7z.exe a -mx0 -tzip "YourScenarioNameHere.zip" 15000.slp 15002.slp`

## Weather System Notes
To control weather in an scx, please note the following:

- gaia resource 230 must be set to the extension opt-in value: -53754
- gaia resource 231 controls the precipitation style: 2=rain, 3=thunderstorm, 4=snow, 0=none
- if 231 is negated, precipitation will fall toward the left instead of toward the right
- gaia resource 232 controls the fog color override palette value from 0 to 255
- gaia resource 233 controls the live color override palette value from 0 to 255
- gaia resource 234 controls the water movement direction: 0=random, 1=right: -1=left
- the palette in use for terrain colors is stored in resource #50500
- to set the required resources, you must tribute the value from gaia to gaia
- any weather state can be adjusted during gameplay with scenario triggers

## Extended Trigger Effect Notes
Create Object:
- Number:0 Create object
- Number:1 Enable object
- Number:2 Disable object

Research Technology:
- Number:0 Actually research the tech
- Number:1 Enable tech
- Number:2 Disable tech
- Number:3 Force enable tech within tech tree limits
- Number:5 Force enable tech without regard for limits

Change View:
- Number:0 Scroll the view to the new position
- Number:1 Jump to the new position without scrolling

Change Object HP:
- Number:0 Change HP
- Number:1 Set HP
- Number:2 Add HP until maximum HP (healing)

Change Object Attack:
- Number:0 Change attack
- Number:1 Set attack

Freeze Unit:
- Number:0 Freeze (set no-attack stance and stop the unit)
- Number:1 Set aggressive stance
- Number:2 Set defensive stance
- Number:3 Set stand-ground stance
- Number:4 Set no-attack stance

Change Ownership effect:
- Number:0 Change owner
- Number:1 Flash object on the minimap

Declare Victory:
- Number:0 Set player victory
- Number:1 Set player defeated

Stop Unit effect:
- Number:0 Stop unit
- Number:1-9 Set units to Ctrl Group #1-9

Task Object:
- Number:0 Task object
- Number:1 Teleport object

Play Sound:
- Number:0 Play sound
- Number:1-8 Change sound volume (1=max, 8=min)
- Number:9 Stop sound

Change Object Name:
- Number:0 Change name
- Number:1 Apply up-attribute list

Display Instructions:
- Number:9 + Timer:99999999 Apply up-effect list
