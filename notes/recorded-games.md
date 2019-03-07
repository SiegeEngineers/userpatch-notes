# Recorded Game notes

## Map Terrains

UserPatch 1.5 adds "original terrain" data to map tiles. This changes how recorded games and save files should be read.

Previously, tiles looked like:
```c
struct Tile {
  char terrain;
  char elevation;
}
```

Now, tiles look like:
```c
struct Tile {
  char marker; // always 0xFF (-1)
  char terrain;
  char elevation;
  char original_terrain;
}
```

You can use the `marker` to check if the tile has `original_terrain` data.

```c
char terrain = read_byte();
char elevation = -1;
char original_terrain = -1;
if (terrain != -1) {
  // No original terrain data, this is userpatch < 1.5 or AoC or AoE etc
  elevation = read_byte();
} else {
  terrain = read_byte();
  elevation = read_byte();
  original_terrain = read_byte();
}
```

## Additional Player data

> **Note:** I'm not sure if this was added in UP1.5 or earlier. Please send a PR if you find that this applies to earlier versions as well.

UserPatch 1.5 recorded games contain an additional data block for each player, used to store UP-specific AI targeting information. In particular, it's a buffer of size 0x1FF0 right before the PlayerTech structure.

If your rec game reader hardcodes a number of bytes to skip for each player somewhere between the player color data and the diplomacy block (which is variable size, so needs to be read partially anyway), make sure to skip 0x1FF0 more bytes for UP 1.5 recorded game files.

## Postgame Data

UserPatch 1.4 and up add multiplayer postgame data to recorded game files. This is the data that is sent to a lobby application (like the old MSN Zone or Voobly) after a game has completed, so the lobby application can handle ratings and generate match statistics pages and such.

The postgame data structure is stored as a rec game body action of type `0xFF`. The structure is as follows:

```c
struct AgeZoneScoreInfoMilitary
{
  short score;
  short units_killed;
  short hit_points_killed;
  short units_lost;
  short buildings_razed;
  short hit_points_razed;
  short buildings_lost;
  short units_converted;
};

struct AgeZoneScoreInfoEconomy
{
  short score;
  int food_collected;
  int wood_collected;
  int stone_collected;
  int gold_collected;
  short tribute_sent;
  short tribute_received;
  short relic_gold;
};

struct AgeZoneScoreInfoTech
{
  short score;
  int feudal_time;
  int castle_time;
  int imperial_time;
  char map_exploration;
  char research_count;
  char research_percent;
};

struct AgeZoneScoreInfoSociety
{
  short score;
  char total_wonders;
  char total_castles;
  char relics;
  short villager_high;
};

struct AgeZoneScoreInfoPlayer
{
  char name[16];
  short total_score;
  short total_scores[8];
  char victory;
  char civilization;
  char color;
  char team;
  char ally_count;
  char random_civ;
  char mvp;
  int game_outcome;
  AgeZoneScoreInfoMilitary military;
  char padding0[32];
  AgeZoneScoreInfoEconomy economy;
  char padding1[16];
  AgeZoneScoreInfoTech tech;
  AgeZoneScoreInfoSociety society;
  char padding2[84];
};

struct AgeZoneScoreInfo
{
  char scenario_filename[32];
  char num_players;
  char num_computer_players;
  int duration;
  char allow_cheats;
  char complete;
  int db_checksum;
  int code_checksum;
  float version;
  char map_size;
  char map_id;
  short pop_limit;
  char victory;
  char starting_age;
  char resource_level;
  char all_techs;
  char team_together;
  char reveal_map;
  char is_death_match;
  char is_regicide;
  char starting_units;
  char teams_locked;
  char speed_locked;
  AgeZoneScoreInfoPlayer players[8];
  char padding0[4];
};
```

The byte alignment in each of these structs is to 4 bytes. So, a `short` followed by an `int` will have 2 empty padding bytes. Two `short`s followed by an `int` have none, because they add up to 4 bytes.

There is a lot of additional padding in the AgeZoneScoreInfoPlayer structure, as far as I can tell nothing is being stored there.
