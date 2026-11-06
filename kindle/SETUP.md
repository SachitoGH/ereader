# Kindle Paperwhite 11th gen (PW5): setup plan

**Device:** Kindle Paperwhite 11th generation (2021), KindleModding codename `PW5`
**Current firmware:** 5.19.2 (as of 2026-11-06)
**Status:** ⚠️ **Keep in airplane mode until the jailbreak is done.** An Amazon update past 5.19.5 could close every current method.
**Goal:** jailbroken, updates blocked, KOReader for anything that isn't Amazon content, Amazon side untouched.

## Which jailbreak (checked 2026-11-06)

Checked against `kindlemodding.org/jailbreaks.json`, the data behind the official Jailbreak Wizard.

| Method | PW5 firmware range | Fits 5.19.2? | Notes |
|---|---|---|---|
| **SpiderCat** | 5.16.3 – 5.19.5 | ✅ **Wizard's first pick** | Book-based (`spidercat.azw3`), needs Wi-Fi during the run |
| Véra | 5.17.1 – 5.20.2 | ✅ fallback | Via the Kindle browser, needs Wi-Fi |
| SpringBreak | 5.18.1 – 5.19.2.0.1 | ✅ (only if registered) | Needs PC and cable, fills storage with thousands of folders. Last-resort fallback |
| Nosebleed / Sanctuary / AdBreak / WinterBreak | up to 5.18.x | ❌ | Too old for 5.19.2 |

**Plan: SpiderCat.** If it fails, try Véra, then SpringBreak.

## Important changes in the modern toolchain (2026)

- **KUAL is obsolete** on modern jailbreaks, per KindleModding. Homebrew is now **scriptlets** (`.sh` files that show up as books) installed through **KPM**, the Kindle Package Manager, which the jailbreak installs.
- **Updates are blocked automatically** by the jailbreak.
- **Jobman** (KindleTweaks) manages update blocking, the jailbreak environment, battery health, SSH and recovery mode.
- KPM commands are typed into the **home screen search bar**: `;kpm update`, `;kpm install <pkg>`, `;kpm launch <pkg>`, `;kpm uninstall <pkg>`.

## Steps

### 0. Before starting
- [x] Kindle stays in airplane mode
- [x] Battery ≥ 50% (46% and charging, OK because the jailbreak is light)
- [ ] Note the serial number and exact build: Settings → Device Options → Device Info
- [x] Backup: full drive copied to `backup/2026-11-06/` (429 files, 171 MB). Note: this PW5 mounts as **USB mass storage (FAT32, drive E:)**, not MTP. Wake and unlock it first, or it shows "No Media"
- [x] Re-ran the wizard check on 2026-11-06: data unchanged
- [x] Storage filled: `fill_disk/fill1.bin` + `fill2.bin` (6.0 GiB total), about 70 MB left free

### 1. Jailbreak with SpiderCat
Guide: https://kindlemodding.org/jailbreaking/SpiderCat/
- [x] Download `https://kindlemodding.org/spidercat/spidercat.azw3` to `downloads/` (done 2026-11-06, 484 KB, valid Mobipocket file. Re-download right before use in case it's been updated)
- [x] Copied to `E:\documents\spidercat.azw3` (sha256 verified), then ejected
- [x] Turn Wi-Fi **on** (required, and keep it on until the end)
- [x] Open "SpiderCat" from the Library. Wait: blank page, then scrolling text. Ignore "application error" popups
- [x] It shows "Restarting Kindle" and reboots jailbroken. ✅ 2026-11-06: `documents/JAILBROKEN.txt` says "You are jailbroken! (jb.sh v2.6.0)", uid=0
- [ ] If it shows `abort:no-firm`, the firmware or model doesn't match. Recheck, or try Véra

### 2. Cleanup (right after)
- [x] Delete any `update*.bin` / `update.bin.tmp.partial` in the Kindle's root
- [x] Delete filler files (fill_disk removed, 6.07 GB free again). Also removed spidercat.azw3 and .sdr

### 3. Homebrew basics (on the device, Wi-Fi on, after one reboot)
- [x] Home search bar: `;kpm update`
- [x] `;kpm install jobman`, then open Jobman and confirm **updates are blocked**. ✅ Jobman shows "OTA update currently disabled"
- [ ] (Skip Jobman's recovery mode for now. It's marked "highly experimental")

### 4. KOReader
- [x] `;kpm install koreader` (official KindleModding repo, `repo.kindlemodding.org`)
- [x] Launch it from the new scriptlet icon or with `;kpm launch koreader` (✅ opened 2026-11-06, the icon was slow to appear)
- [ ] Check for a "no framework" launch option, which stops Amazon's interface while reading for more RAM and speed. Use it if available

### 5. KOReader configuration
Same as the Kobo (see `../kobo/SETUP.md` step 7): open the last file at start, refresh interval, turn off unused plugins, gestures.
- [ ] Turn on **Progress sync** with the same account as the Kobo

### 6. Extras: the most widely used (from KindleTweaks/Awesome-Kindle and KindleModding)
Installed:
- [ ] **Jobman**: jailbreak manager (update blocking, battery health, SSH)
- [ ] **KOReader** and its built-in plugins: Calibre wireless, Statistics, Cover browser, Progress sync, OPDS, Wallabag
- [ ] **App Store plugin** for KOReader (`omer-faruq/appstore.koplugin`): optional, so check it's maintained first

Also in the official KPM repo (the full list is KPM, Jobman, KOReader, kTerm, KOmpanion):
- [ ] (skipped by the user) **kTerm** (`;kpm install kterm`): terminal, a safety net for older add-ons
- [x] **KOmpanion** (`;kpm install kompanion`): EPUBs show in the Kindle home library and open in KOReader

Optional, ask the user:
- [ ] **UsbNetLite**: SSH over USB/Wi-Fi so Claude can manage the device directly (Jobman may already cover this)
- [ ] **KindleForge**: on-device tweak downloader
- [ ] **HotfixUpdater**: keeps the hotfix up to date
- [ ] Fun or productivity: KAnki (flashcards), KPomo (pomodoro timer), Textadept (text editor with a Bluetooth keyboard), games

## Rules after jailbreak

- ✅ Wi-Fi can be used normally once Jobman confirms updates are blocked
- ✅ Amazon store, Kindle books, sync and Goodreads keep working. Kindle store books (DRM) open in Amazon's reader only
- ❌ Never factory reset (it removes the jailbreak)
- ❌ Never install Amazon updates manually. Wait for community confirmation first
- Only install homebrew from KPM or the trusted sources listed above

## Log

- 2026-11-06: Research done. Firmware 5.19.2, airplane mode on. Jailbreak planned after the Kobo.
- 2026-11-06: spidercat.azw3 downloaded. Firmware 5.19.2 is likely the final version for the PW5, and Véra covers up to 5.20.2 as a fallback.
- 2026-11-06: ✅ Jailbroken with SpiderCat (jb.sh v2.6.0). Filler removed. Next: reboot, then `;kpm update`, `;kpm install jobman`, then KOReader.
- 2026-11-06: Jobman (OTA disabled) and KOReader installed via KPM. Recommended kTerm and KOmpanion.
- 2026-11-06: ✅ Kindle done. KOmpanion installed and indexing EPUBs. KOReader set to start with the last file, home /mnt/us/documents, refresh every 6 pages, no UI flashes. Leftovers removed. Settings backed up to backup/2026-11-06-after-setup/. Remaining: Progress sync (with the Kobo).
