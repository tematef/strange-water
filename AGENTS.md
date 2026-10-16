# Strange Water — S.T.A.L.K.E.R. 2 Xbox Mod Pipeline

## Project Goal

Inspect, sanitize, and stage PC mods for **S.T.A.L.K.E.R. 2: Heart of Chornobyl** (Unreal Engine 5)
for Xbox console deployment via [mod.io](https://mod.io/g/stalker2).

The pipeline takes a Nexus Mods `.7z` archive, validates it against Xbox console restrictions,
repackages it as a mod.io-compatible `.zip`, and uploads it as a hidden/private mod for testing.

## Architecture

```
Input (.7z from Nexus)
  → Extract (7zz)
  → Console Security Audit (no .dll/.exe/.asi/.so)
  → PAK Inspection (repak, optional)
  → Rename with _P suffix (Unreal patch precedence)
  → Package as .zip
  → Upload to mod.io REST API (hidden, Xbox Series X platform)
  → Submit "Cook Mod Request for Xbox" ticket to GSC Help Center (support.stalker2.com)
```

## Key Conventions

- **PAK naming**: Always use `_P.pak` suffix for Unreal Engine patch loading precedence
- **mod.io API domain**: Use `https://g-{game_id}.modapi.io/v1` (NOT deprecated `api.mod.io`)
- **Game ID**: S.T.A.L.K.E.R. 2 = `5761` on mod.io
- **Visibility**: Always upload as `visible=0` (hidden) first, publish manually after Xbox testing
- **Platform header**: Include `X-Modio-Platform: xboxseriesx` on file uploads
- **GSC Console Approval**: Uploading to mod.io leaves Xbox in `status: 0` (Pending). You MUST submit a "Cook Mod Request for Xbox" ticket via [support.stalker2.com](https://support.stalker2.com) with a direct Google Drive link to the single `.zip` file. See local skill `stalker2-xbox-approval`.


## Console Safety Rules (FATAL)

1. **Zero native code** — NO `.dll`, `.exe`, `.asi`, `.so`, `.dylib` files. Abort immediately if found.
2. **PAK required** — Archive must contain at least one `.pak` file.
3. **Size limit** — Final `.zip` must be under 100 MB for mod.io.

## Tools

| Tool | Purpose | Install |
|------|---------|---------|
| `7zz` | Extract .7z archives | `brew install 7zip` |
| `repak` | Inspect .pak internals (optional) | `cargo install repak` (needs Rust) |
| `zip` | Create mod.io distribution package | Pre-installed on macOS |
| `curl` | mod.io REST API calls | Pre-installed on macOS |
| `python3` | JSON parsing, logo generation | Pre-installed on macOS |

## Sensitive Data

All credentials and tokens are stored in `.env` (gitignored). Load with:
```bash
source .env
```

## Workspace Layout

```
workspace/
├── raw_mod/          # Extracted archive contents
├── staged_xbox/      # Renamed _P.pak ready for packaging
├── *.zip             # Final mod.io distribution package
└── upload_to_modio.sh  # Upload automation script
```
