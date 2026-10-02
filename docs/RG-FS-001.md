# RG-FS-001: First recompile of FIFA Street

| | |
| --- | --- |
| Disc | `FIFA Street (USA, Europe) (En,Fr,De,Es,It)` |
| Title / media | `4541097B` / `78AA9072` |
| Executables | `default.xex` v0.0.0.3, built 2012-01-17 04:09:06 UTC; `fifadllzf.xex.dll` v0.0.0.3, built 2012-02-05 03:39:47 UTC |
| `default.xex` SHA-256 | `f4902ab0a2515ca64675544852a0263ba429863dad13dc19b7c2d989c97aa2b1` |
| `fifadllzf.xex.dll` SHA-256 | `9d2fb47c3caf279d55962f671afc5b8eb7a597717d44ccb5995dd002a3c26201` |
| SDK | furqanagwan/rexglue-sdk `main` `d3e2846`, GDK Release |
| Hardware | NVIDIA RTX 5080 Laptop GPU, Windows 11 |
| Date | 2026-10-02 |

- **Extraction:** the image's game partition is at byte `0xFD90000` (XGD2);
  106 files, 4,659,464,494 bytes. `default.xex` (1.7 MB, image 82000000-821C0000)
  is a launcher; the game is `fifadllzf.xex.dll` (image 82300000-83EC0000),
  which it loads by its bare name. The disc also carries
  `dlc/dlc_footballcompeng/dlc/footballcompeng/footballcompengzf.xex.dll`. The
  `$SystemUpdate` folder is a console update.
- **Codegen:** `rexglue init --scan-dll` adds both DLLs as `[[modules]]`.
  `rexglue codegen` with no title hints: 849 files, 287 seconds. One
  unresolved branch is reported, `0x82EB0884` to `0x82EB07E0`, in the game
  DLL; it hasn't caused a fault so far.
- **SDK fixes needed to start** (rexglue-sdk#181):
  - The DLL's recompiled code wasn't found: its bare name was joined to the
    launcher's device path, and the lookup key kept the device.
  - The launcher's dispatch table, placed after its image, ran across the game
    DLL's image. Codegen now places it at `88230000`.
- **Run:** a GDK run with a throwaway user folder, `--mnk_mode=true`, Enter
  for Start and Space for A:
  - the loading screens and the controls screen;
  - the opening 5-a-side match (Chicago);
  - about 60 fps at 3840 x 2160 once loaded.

  The log's only error lines are `fifadllzf.xex.dll` resolved by bare name
  once and `E:\dvdini` (a development drive) not found.
- **Known problem:** the crowd around the pitch draws solid magenta.

Not checked: the menus, saves, a full match, AMD and Intel GPUs, the standard
(non-GDK) build.
