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

Uploading to mod.io registers the file for Xbox but leaves it in `status: 0` (**Pending approval**). To have GSC's console engineering team cook and certify your mod for Xbox Series X|S, you must submit an official ticket through GSC's support portal.

### Step A: Google Drive Archive Preparation
1. Ensure your mod package is compressed as a **single `.zip` file** (e.g. `WeirdWater_Undrunked_Xbox.zip`).
2. Upload the `.zip` file to your Google Drive.
3. Right-click the file $\rightarrow$ **Share** $\rightarrow$ General access: set to **"Anyone with the link"** (Viewer).
4. **FATAL REJECTION RULE**: The Google Drive link must point directly to the **single `.zip` file** (URL format: `https://drive.google.com/file/d/<FILE_ID>/view?usp=sharing`).
   - ❌ **DO NOT** share a Google Drive folder containing files or sub-folders. GSC's automated intake will instantly reject folder links.
   - 💡 **Verification**: Test the link in a browser **Private/Incognito** window to ensure anyone can download it without logging into a Google account.

### Step B: Form Fields & Ticket Submission
1. Direct Ticket URL: **[https://support.stalker2.com/en-US/support/tickets/create](https://support.stalker2.com/en-US/support/tickets/create)**
2. Fill out every field exactly as required:

| Form Field | Value / Format | Notes |
| :--- | :--- | :--- |
| **Form Type / Category** | `New mod / Mod update` | Required category for console intake |
| **Name** | Your creator / author name | Must match your mod.io username |
| **Email** | Your active contact email | Where GSC sends ticket updates and status |
| **Subject** | `New mod: <Exact Mod Name>` *(or `Update mod: <Exact Mod Name>`)* | **STRICT PREFIX**: Must begin with either `New mod:` or `Update mod:` followed by the exact mod title on mod.io |
| **Description** | Detailed mod description (see template below) | Detail what the mod changes and game version |
| **URL to Google Drive with a mod** | Direct Google Drive file link | Link to the single `.zip` file from Step A |
| **URL to mod's page on mod.io** | `https://mod.io/g/stalker2/m/<mod-slug>` | Full mod.io profile URL |

#### Description Field Template:
```text
Mod Title: Weird Water Debuff Tweak
Target Game: S.T.A.L.K.E.R. 2: Heart of Chornobyl
Target Platform: Xbox Series X|S
Mod Version: 2.0.0
mod.io ID: 6393766

Summary:
Removes heavy visual distortion, blurring, and screen wobble effects applied to the player when equipping the "Weird Water" artifact.

Technical Details:
- Type: Config patch (ItemPrototypes.cfg bpatch)
- Dependencies: None (self-contained)
- Native Code: Zero binaries (.dll, .exe, .asi)
- Game Update Compatibility: Tested and fully compatible with Update 2.0
```

### Step C: Mandatory Checkbox Confirmations
Before submitting, check the required confirmation boxes:
1. ☑ **Asset References**: *"Confirm that your mod does not contain references to assets that aren't natively included within the mod itself."* (Ensures no dangling or unbundled third-party dependencies).
2. ☑ **Safety & Guidelines**: *"Confirm that files are clean, safe, and do not contain malicious code or inappropriate content."* (Adheres to GSC Fan Content Guidelines & Microsoft Xbox Content Policies).

### Step D: Submission & Confirmation
1. Click **Create** (`#create-ticket-submit-btn`).
2. An automated confirmation email with a Ticket ID (e.g. `#TICKET-12345`) will arrive from `support@stalker2.com`.
3. **Closed Beta Priority Note**: GSC currently prioritizes console mod cooking requests from participants of the **Closed Beta**. If your ticket receives a notice about Closed Beta prioritization, reach out to Community Manager **`Super_PropheT`** on the official S.T.A.L.K.E.R. Discord ([discord.gg/stalker](https://discord.gg/stalker)) with your Ticket ID and mod.io link to request inclusion in the Closed Beta.


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
