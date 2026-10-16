---
name: stalker2-xbox-approval
description: >-
  Guide and execute the official GSC Game World / ZoneKit approval workflow to
  certify and cook S.T.A.L.K.E.R. 2 mods for Xbox console deployment. Use when
  the user wants to get a S.T.A.L.K.E.R. 2 mod approved on Xbox, submit a Cook Mod
  Request ticket, resolve "Pending approval" status on mod.io, or package a console mod.
---

# S.T.A.L.K.E.R. 2 Xbox Console Mod Approval & Packaging Guide

In S.T.A.L.K.E.R. 2: Heart of Chornobyl, publishing a mod for Xbox involves a **two-stage workflow**:
1. **Stage 1 — mod.io Staging**: Uploading the mod files to mod.io, tagging Xbox compatibility, and publishing with media.
2. **Stage 2 — GSC Console Cooking & Certification**: Submitting an official "Cook Mod Request for Xbox" ticket to GSC Game World so they can compile, test, and approve the build for Xbox Series X|S hardware.

---

## 1. Engine & Technical Constraints (Official GSC Rules)

According to GSC's official ZoneKit guides (Articles 62–64), the current game engine imposes strict restrictions:

* **No Blueprint Scripting**: Blueprint-based code/script mods are **not supported** in the game at this time. Data modifications in existing blueprints are allowed, but new custom blueprint logic will fail.
* **No Level / World Partition Mods**: Modifying or checking out levels, external actors, or World Partition objects is strictly unsupported and causes editor/game crashes.
* **Audio Restrictions**: Custom Wwise/audio projects cannot be added; only replacing existing game audio assets is supported.
* **Exact Content Hierarchy**: All config/asset files must replicate the exact folder structure from the game's `Content/` directory (e.g. `Content/GameLite/GameData/ItemPrototypes.cfg`).
* **Zero Native Binaries (FATAL Console Rule)**: Console packages must NEVER contain `.dll`, `.exe`, `.asi`, `.so`, or `.dylib` files.

---

## 2. Packaging Specifications

### ZoneKit Official Pipeline (Windows / SDK)
When packaged via ZoneKit ("Package Mod"):
* Files are output to: `s2gamebuild\SavedMods\Staged\<mod_name>`
* ZoneKit packages into UE5 IoStore format: `.pak`, `.utoc`, and `.ucas`.
* Verification tool: `UnrealPak <path_to_pak> -list` (located in `s2editor\Engine\Binaries\Win64`).

### Manual / Non-SDK Pipeline (Linux / macOS / Repak)
* When compiling outside the ZoneKit SDK, package the modified files into a single `.pak` file.
* Always append the `_P` suffix (e.g. `ModName_P.pak`) to enforce Unreal Engine patch precedence over base archives.

---

## 3. Stage 1: mod.io Upload & Staging (Article 65)

Follow GSC's official mod.io requirements:

1. **Logo File Requirements**:
   * Format: **PNG** (recommended by GSC guidelines) or JPEG.
   * Dimensions: **512×288 px or larger** (16:9 aspect ratio strongly recommended).
   * Maximum file size: **8 MB**.
2. **Platform Tagging Note (Crucial GSC Rule)**:
   * In the ZoneKit upload tool, always select **"Create mod for PC"**, *regardless of which platform the mod was created for*.
   * After upload, navigate to the mod page on the mod.io website $\rightarrow$ **Edit Mod** $\rightarrow$ select both **PC** and **Xbox** checkboxes to tag supported platforms.
3. **Private Testing Flow**:
   * Set visibility to **Private** (`visible: 0`) first.
   * Subscribe to your own mod via mod.io to verify mounting and stability.
   * Once validated, switch visibility to **Public** (`visible: 1`).
4. **Third-Party Attribution**:
   * If porting or referencing another creator's mod, the description MUST include an explicit "Author Permissions & Credits" section citing the original creator and open repository/Nexus page.

---

## 4. Stage 2: GSC Xbox Cook Request Ticket

Uploading to mod.io tags the file for Xbox but leaves it in `status: 0` (**Pending approval**). To get the mod approved and live on Xbox console, a formal request must be submitted to GSC Game World:

### Step A: Prepare the Google Drive Archive
1. Compress your mod's packaged files into a **single `.zip` file**.
2. Upload the `.zip` file to Google Drive.
3. Set sharing permissions to **"Anyone with the link can view"**.
4. **CRITICAL GSC RULE**: The Google Drive link must point directly to the **single `.zip` file**. If the link points to a folder containing sub-folders, GSC will automatically reject the submission.

### Step B: Submit the Help Center Ticket
1. Go to the [S.T.A.L.K.E.R. 2 Support Portal](https://support.stalker2.com).
2. Click **Submit a request** (top right) $\rightarrow$ choose category **"New mod / Mod update"**.
3. Fill in the form fields:
   * **Email**: Creator's contact email.
   * **Subject**: Must strictly begin with `New mod: <Exact Mod Name>` or `Update mod: <Exact Mod Name>`.
   * **Description**: Enter the exact mod title registered on mod.io and a short summary.
   * **URL to Google Drive with a mod**: The direct file link from Step A.
   * **URL to mod's page on mod.io**: The full mod URL (`https://mod.io/g/stalker2/m/...`).
4. **Required Checkbox Confirmations**:
   * Check: *"Mod does not contain references to external assets that are not natively included within the mod."*
   * Check: *"Files are safe, clean, and comply with GSC Fan Content Guidelines."*

---

## 5. Status Tracking via mod.io API

Monitor the moderation progress using curl:

```bash
curl -s -H "Authorization: Bearer $MODIO_ACCESS_TOKEN" \
  "https://g-5761.modapi.io/v1/games/5761/mods/$MODIO_MOD_ID/files"
```

* `status: 0` — **Pending Approval** (In GSC's console test queue)
* `status: 1` — **Approved / Live** (Visible and downloadable in Xbox in-game mod browser)
* `status: 2` — **Denied** (Check email for GSC feedback/reasons)

---

## 6. Official Knowledge Base References
* [Article 62: Getting Started: First Steps in the Zone's Code](https://support.stalker2.com/en-US/kb/article/62/getting-started-first-steps-in-the-zones-code)
* [Article 63: Shaping the Zone: Modifying Existing Game Content](https://support.stalker2.com/en-US/kb/article/63/shaping-the-zone-modifying-existing-game-content)
* [Article 64: Configuration, Modding and Packaging your Mod](https://support.stalker2.com/en-US/kb/article/64/configuration-modding-and-packaging-your-mod)
* [Article 65: Sharing Your Creations: Uploading and Testing Mods](https://support.stalker2.com/en-US/kb/article/65/sharing-your-creations-uploading-and-testing-mods)
* [How to create a cook mod request for Xbox](https://support.stalker2.com/hc/en-us/articles/how-to-create-a-cook-mod-request-for-xbox)
