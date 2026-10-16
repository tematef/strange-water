# Session Gains & Reference

## 2026-09-19: Initial Pipeline Run — "Remove Weird Water Debuff" (Nexus #382)

### What We Did
- Converted PC mod (Nexus Mod #382, v1.7) from `.7z` → Xbox-ready `.zip`
- Passed console security audit: single `.pak` file, zero native binaries
- Renamed `Weird Water Not Drunk.pak` → `WeirdWaterDebuffRemoved_P.pak` (added `_P` suffix)
- Uploaded to mod.io as hidden mod (visible=0) targeting Xbox Series X

### mod.io Results
- **Mod page**: https://mod.io/g/stalker2/m/weird-water-debuff-tweak-private-test
- **Mod ID**: 6393766
- **File ID**: 8230747
- **Version**: 1.0.0-test
- **Platform**: xboxseriesx (status: pending approval)

### Discoveries
- mod.io deprecated `api.mod.io` domain → must use `g-{game_id}.modapi.io`
- Homebrew `7zip` installs as `7zz` binary (not `7z`)
- `repak` v0.2.3 has no macOS ARM64 prebuilt — needs Rust toolchain to build from source
- mod.io email auth flow: `POST /oauth/emailrequest` → get 5-digit code → `POST /oauth/emailexchange` → receive JWT access token
- S.T.A.L.K.E.R. 2 game ID on mod.io = 5761 (queried via `?name_id=stalker2`)
- mod.io `Add Mod` requires a logo file (min 512×288 PNG/JPG) as multipart form field

### Pipeline Timings
- Total time: ~10 minutes (including brew install + mod.io auth flow)
- Archive: 8.2 KB compressed → 143 KB uncompressed → 8.6 KB re-zipped

## 2026-09-20: Update to 2.0-Compatible Mod

### What We Did
- Identified old 1.7 mod was a full replacement of `ArtifactPrototypes.cfg` (147KB), likely causing crashes in v2.0.
- Downloaded new 2.0-compatible mod from GitHub (`r-skrylnyk/stalker2-mods`).
- Re-packaged as `WeirdWater_Undrunked_P.pak` (710 bytes) - much safer as it likely uses UE config merging.
- Uploaded as v2.0.0-test to existing Mod.io mod ID 6393766.

### mod.io Results
- **New File ID**: 8234630
- **Version**: 2.0.0-test

## 2026-10-16: Console Approval Investigation & Author Credit Update

### Findings
- Investigated why Xbox Series X status is still "Pending approval" (status: 0 on File 8234630).
- Confirmed GSC Game World manually vets all console mod submissions in periodic batches.
- Identified potential curation delays: lack of screenshots, review backlog, and missing author permission note.

### Actions Taken
- Updated mod.io description via REST API to include explicit Author Permissions & Credits citing `r-skrylnyk` and `TheSilverFoxe` (Nexus #382) to satisfy moderation and copyright guidelines.
- Generated and uploaded high-resolution 16:9 listing banner (1376x768, 691KB) to replace the dark placeholder logo on mod.io via `POST /media`.


