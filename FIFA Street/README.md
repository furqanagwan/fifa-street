<p align="center"><img src="https://raw.githubusercontent.com/xenia-manager/x360db/main/titles/4541097B/artwork/banner.png" alt="EA SPORTS FIFA Street marketplace banner"></p>

# EA SPORTS FIFA Street

The 2012 FIFA Street for Xbox 360, statically recompiled from its executables
into a native Windows PC game with
[ReXGlue](https://github.com/furqanagwan/rexglue-sdk).

Street football from the makers of EA SPORTS FIFA: small-sided games in more
than 35 locations around the world, with Street Ball Control, aerial skills,
wall play and over 50 skill moves.

**Status: In-game.** It recompiles, boots and plays a match on its first
run; the crowd draws solid magenta, and the rest hasn't been validated yet.

## The game

| | |
| --- | --- |
| Released | 16 March 2012 |
| Developer | EA Canada |
| Publisher | Electronic Arts |
| Genre | Sports (street football) |
| Players | Up to 4 locally, co-operative; online play needed EA's servers, now closed |
| Rating | ESRB E (Everyone) |

## The release this is built from

| | |
| --- | --- |
| Disc (Redump) | `FIFA Street (USA, Europe) (En,Fr,De,Es,It)`, one disc |
| Region | USA and Europe |
| Languages | English, French, German, Spanish, Italian |
| Title ID | `4541097B` |
| Media ID | `78AA9072` |
| Executables | `default.xex` v0.0.0.3, a launcher (built 2012-01-17), and `fifadllzf.xex.dll` v0.0.0.3, the game (built 2012-02-05); the disc also carries `dlc/.../footballcompengzf.xex.dll` |
| `default.xex` SHA-256 | `f4902ab0a2515ca64675544852a0263ba429863dad13dc19b7c2d989c97aa2b1` |
| `fifadllzf.xex.dll` SHA-256 | `9d2fb47c3caf279d55962f671afc5b8eb7a597717d44ccb5995dd002a3c26201` |
| Title update | None applied. |

Check that your disc's title and media IDs match: the recompiled code is only
valid for these executables.

## What works

Tested on an NVIDIA RTX 5080 Laptop GPU ([RG-FS-001](../docs/RG-FS-001.md)).

- **Recompiles without title hints:** both DLLs as modules (`rexglue init
  --scan-dll`). It needed two SDK fixes to start: finding a DLL loaded by its
  bare name, and moving the launcher's dispatch table off the game DLL's image
  (rexglue-sdk#181).
- **Plays a match** from the first run: the loading screens, the controls
  screen and the opening 5-a-side match at about 60 fps at 3840 x 2160.

**Known problem:** the crowd around the pitch draws solid magenta.

**Not validated yet:** the menus, saving and loading, a full match, input
beyond the first match, AMD and Intel GPUs, and the standard (non-GDK) build.

## Configuration

[`fifastreet.toml`](fifastreet.toml) is this game's codegen configuration. It
needs no title-specific settings yet, so it only identifies the executables.
Building is described in [docs/building.md](../docs/building.md).

## Sources

- [LaunchBox Games Database](https://gamesdb.launchbox-app.com/games/details/16298-fifa-street):
  release date, developer, players, rating.
- [x360db](https://github.com/xenia-manager/x360db/tree/main/titles/4541097B)
  (Xenia Manager's database): the marketplace description and the banner above.
- The disc and its XEX headers: everything under "The release this is built
  from".
