# Building the game

Until releases are published, the game is built from source against the
ReXGlue SDK. Everything you create here stays in the game's folder, which git
ignores apart from its `README.md` and `.toml`.

## Requirements

- **Your own copy of the game:** the disc listed on its page. Check that its
  title and media IDs match.
- **Windows 11 x64 and a Direct3D 12 GPU.** Only NVIDIA has been tested.
- **The build tools:** Visual Studio 2026 with LLVM Clang, CMake and Ninja, as
  listed in the [SDK README](https://github.com/furqanagwan/rexglue-sdk#requirements).
- **The ReXGlue SDK** from [furqanagwan/rexglue-sdk](https://github.com/furqanagwan/rexglue-sdk),
  built and installed from `main`.
- **Optional:** Microsoft GDK 260404, for the GDK build.
- **Optional:** your console's dashboard system update (`$SystemUpdate`), so
  the Xbox guide is built in; see the SDK's
  [Xbox guide](https://github.com/furqanagwan/rexglue-sdk/blob/main/docs/xbox-guide.md#using-it).

| Game | Folder | Project name | Executables | Configuration |
| --- | --- | --- | --- | --- |
| FIFA Street | `FIFA Street/` | `fifastreet` | `default.xex`, `fifadllzf.xex.dll` | `fifastreet.toml` |

## 1. Extract the files

Extract your disc image into `FIFA Street/game/`, for example with
[extract-xiso](https://github.com/XboxDev/extract-xiso). The disc's
`$SystemUpdate` folder is a console update, not game code; it can stay.

## 2. Create the project

`default.xex` is a small launcher; the game's code is in
`fifadllzf.xex.dll`, which it loads. `--scan-dll` adds every `.xex.dll` on the
disc as a module, so both are recompiled:

```powershell
rexglue init --project-name fifastreet --xex-path "FIFA Street/game/default.xex" --game-root "FIFA Street/game" --project-root "FIFA Street/recompiled" --scan-dll
```

## 3. Add the game's configuration

In `FIFA Street/recompiled/fifastreet_manifest.toml`, under `[entrypoint]`:

```toml
includes = ["../fifastreet.toml"]
```

## 4. Generate and build

```powershell
cd "FIFA Street/recompiled"
rexglue codegen fifastreet_manifest.toml
cmake -S . -B out/build/release -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_PREFIX_PATH=<ReXGlue install prefix>
cmake --build out/build/release
```

For the GDK build, use the SDK's GDK install prefix
(`out/install/win-amd64-gdk`).

## Run it

```powershell
.\out\build\release\fifastreet.exe --game_data_root="<...>/FIFA Street/game" --gpu_plugin=xenos
```

Saves go to the per-user location the SDK documents
([data locations](https://github.com/furqanagwan/rexglue-sdk/blob/main/docs/data-locations.md));
`--user_data_root` puts them somewhere else.
