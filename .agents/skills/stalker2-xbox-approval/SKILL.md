---
name: stalker2-xbox-approval
description: >-
  Guide and execute the official GSC Game World / ZoneKit approval workflow to
  certify and cook S.T.A.L.K.E.R. 2 mods for Xbox console deployment. Use when
  the user wants to get a S.T.A.L.K.E.R. 2 mod approved on Xbox, submit a Cook Mod
  Request ticket, resolve "Pending approval" status on mod.io, or package a console mod.
---

# S.T.A.L.K.E.R. 2 Xbox Console Mod Approval Guide

Xbox console deployment for S.T.A.L.K.E.R. 2 mods is **not automated via mod.io alone**. GSC Game World operates a mandatory, ticketed cooking and certification process. This skill guides the preparation, packaging, compliance audit, and ticket submission.

---

## 1. Console Pre-Flight Audit

Before submitting to GSC, verify the mod package strictly satisfies Xbox Requirements (XRCs):

1. **Zero Native Binaries (Fatal Reject)**:
   - Must NOT contain `.dll`, `.exe`, `.asi`, `.so`, or `.dylib`.
   - Mod must consist purely of Unreal Engine `.pak` file(s) or config patches.
2. **Patch Precedence Naming**:
   - Primary pak must end in `_P.pak` (e.g. `ModName_P.pak`) to override base game files in UE5.
3. **No Unbundled External References**:
   - All referenced meshes, textures, or configs must be bundled inside the pak. Dangling asset references cause instant rejection.
4. **Third-Party Attribution & Permissions**:
   - If porting or referencing another creator's mod, the mod.io description MUST contain an explicit "Author Permissions & Credits" section citing the original author and open license.
5. **Listing Media**:
   - mod.io listing must have a 16:9 banner (minimum 512×288, under 8 MB).

---

## 2. Packaging for GSC Submission

GSC enforces strict archive formatting:

1. **Single File Archive**:
   - The distribution package must be a single `.zip` file containing only the `.pak` file(s).
2. **Google Drive Staging**:
   - Upload the `.zip` archive to Google Drive.
   - Set sharing permissions: **"Anyone with the link can view"**.
   - **CRITICAL**: The link must point directly to the **single `.zip` file**, NOT to a Google Drive folder or directory. Links to folders are automatically rejected.

---

## 3. GSC Ticket Submission Workflow

Submit the official **Cook Mod Request for Xbox** through the GSC Help Center:

1. **Portal**: Navigate to [support.stalker2.com](https://support.stalker2.com).
2. **Action**: Click **Submit a request** (top right) $\rightarrow$ select category **"New mod / Mod update"**.
3. **Form Fields**:
   - **Email**: Creator's contact email.
   - **Subject**: Must strictly begin with `New mod: <Exact Mod Name>` or `Update mod: <Exact Mod Name>`.
   - **Description**: The exact mod name as registered on mod.io, plus a brief description of what the mod modifies.
   - **URL to Google Drive with a mod**: The direct Google Drive link to the single `.zip` archive.
   - **URL to mod's page on mod.io**: The full mod.io profile URL (`https://mod.io/g/stalker2/m/<mod-name>`).
4. **Required Confirmations**:
   - Check confirmation: "Mod does not contain references to external assets not included within the mod."
   - Check confirmation: "Files are clean, safe, and adhere to GSC Fan Content Guidelines."

---

## 4. Verification & Status Tracking

After submission, GSC's console team tests the build on Xbox devkits. Track approval via the mod.io REST API:

```bash
# Query file platform status on mod.io
curl -s -H "Authorization: Bearer $MODIO_ACCESS_TOKEN" \
  "https://g-5761.modapi.io/v1/games/5761/mods/$MODIO_MOD_ID/files"
```

### mod.io Platform Status Codes:
* `status: 0` — **Pending Approval** (Awaiting GSC review)
* `status: 1` — **Approved / Live** (Available in Xbox in-game browser)
* `status: 2` — **Denied** (Check email for rejection notes from GSC)
* `status: 3` — **Targeted / Superseded**

---

## 5. Official References
* [ZoneKit Modder's Zone (support.stalker2.com)](https://support.stalker2.com/hc/en-us/sections/36534422256017-The-Modder-s-Zone)
* [How to create a cook mod request for Xbox](https://support.stalker2.com/hc/en-us/articles/how-to-create-a-cook-mod-request-for-xbox)
* [mod.io S.T.A.L.K.E.R. 2 Hub](https://mod.io/g/stalker2)
