# Spectating notes

## Spectator streams

UserPatch starts a spectator server for the game host. It's a very barebones TCP server that spews rec game data at you when you connect to it.

The spec server is started on localhost (127.0.0.1) on port `53754`.
Depending on the UP configuration, up to 32 clients can connect to it simultaneously. When you connect to the spec server, you first receive a short spectator header, followed by the recorded game file. As the game progresses, you receive more parts of the recorded game file body (the player actions list).

The spectator header is 256 bytes long, and looks like:

```c
struct SpectateHeader {
  // The name of the game/mod used, eg "age2_x1". The official UP spectate.exe
  // client uses this to determine which game exe to start.
  char game_name[32];
  // The rec file extension to use, eg "mgz". This determines the file name for
  // the temporary spec file. It's necessary because mods may have custom
  // extensions, and the extension needs to be correct for AoC to play the file.
  char file_type[32];
  // The name of the player you are receiving the spec stream from.
  char player_name[192];
}
```

## spectate.exe

You can start UP's official spectate.exe with an IP as the first command line parameter to watch a game without using the GUI:

```
.\Age2_x1\spectate.exe 123.45.67.89
```

This will attempt to connect to `123.45.67.89:53754` and start the game if a correct spectator header was received.
