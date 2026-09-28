# D3D12-Wallhack with ImGui Overlay

D3D12 DLL that intercepts Direct3D 12 draw calls with an ImGui overlay and highlights player models (ESP / chams) for graphics-research and testing purposes. Fork of [DrNseven/D3D12-Wallhack](https://github.com/DrNseven/D3D12-Wallhack) with additional controls, diagnostics and configuration.

## Features

- Hooks `DrawIndexedInstanced` / `ExecuteIndirect` via MinHook and renders an ImGui overlay
- **Wallhack toggle** — depth test trick to draw player models through geometry
- **Color / chams toggle** — forces material constants so highlighted models render flat red
- **Probe system** — dumps draw-call histograms (stride, render targets, root constants, index counts, PSO ids, input layout) to `d3d12wh_probe.txt` for identifying player-model draws
- All settings persist to `d3d12wh.ini` next to the process working directory
- Process gate: the DLL only becomes active inside a process named `RainbowSix.exe` (it loads harmlessly elsewhere via system-wide loader configurations)

## Hotkeys

| Key | Action |
|-----|--------|
| `1` | Toggle wallhack (through-walls) |
| `2` | Toggle red player color (chams) |
| `F9` | Toggle probe sampling — writes `d3d12wh_probe.txt` on toggle-off |
| `F8` (hold) | Sweep `coloroffset` 0 → 104 step 4 every 150 ms; saves config on release |

## Build

Requirements: Visual Studio 2022 (or Build Tools) with the v143 toolset and Windows 10/11 SDK.

```
msbuild d3d12h.sln /p:Configuration=Release /p:Platform=x64
```

Output: `x64\Release\d3d12h.dll`

## Usage

1. Build the DLL (or download a release).
2. Run the target game in windowed or borderless-windowed mode.
3. Load the DLL into the target process using your preferred loader, **in environments you own or where you have explicit permission to test**.
4. Press `F9` in a training-vs-bots match for ~60 seconds to collect probe data, then use the histograms to set the identification values in `d3d12wh.ini`.
5. Toggles `1` / `2` work once identification values match the target build.

## Configuration (`d3d12wh.ini`)

| Key | Meaning |
|-----|---------|
| `whOn` | Wallhack master toggle (also bound to `1`) |
| `enablecolor` | Chams toggle (also bound to `2`) |
| `coloroffset` | Offset of the material color constant (target-build specific) |
| `reversedDepth` | Depth convention (0 = normal, 1 = reversed Z) |
| `countfindrendertarget` | How many consecutive frames a draw must hit the RTV filter to be cached |
| `enabletemporaryids` | Allow temporary per-session identification values |
| `DisableOcclusionCulling` | Force-disable occlusion culling while wallhack is on |
| `cri0` … `cri10` | Per-frame material constant overrides (driven by `enablecolor`) |

## Code signing policy

Free code signing provided by [SignPath.io](https://about.signpath.io), certificate by [SignPath Foundation](https://signpath.org)

Team roles and their members:

- Authors: repository maintainers (see [repository people](../../people))
- Reviewers: repository maintainers
- Approvers: repository owners (see [repository people](../../people?query=role%3Aowner))

Privacy policy: This program will not transfer any information to other networked systems unless specifically requested by the user or the person installing or operating it.

## Credits

- [DrNseven/D3D12-Wallhack](https://github.com/DrNseven/D3D12-Wallhack) — original project
- Sh0ckFR, UtterlyTV — upstream credits
- [Dear ImGui](https://github.com/ocornut/imgui), [MinHook](https://github.com/TsudaKageyu/minhook)

## License

MIT — see [LICENSE](LICENSE). All components are covered by OSI-approved licenses.
