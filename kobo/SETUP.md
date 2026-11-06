# Kobo Touch 2.0: setup plan

**Device:** Kobo Touch 2.0 (model N587, Mark 6, 800×600, no front light)
**Current firmware:** latest 4.38 (expected 4.38.23852), updated over Wi-Fi by the user on 2026-11-06. Confirm the exact version from `.kobo/version` on the first USB connection.
**Goal:** debloated and fast. The user wants **the Kobo interface (Nickel) as the main experience** ("KOReader as default is ugly"), with **KOReader as an app** in NickelMenu. No auto-boot into KOReader. Everything reversible.

> ⚠️ **Blocker (2026-11-06): no working USB data cable.** With 3 different cables, the Kobo shows "connected" after tapping Connect, but Windows sees nothing at all (no device, no PnP events). Cleaning the port didn't help. The Kindle works on this PC. Next: try a known data cable (one that came with a phone, or a "sync" cable), the same port the Kindle used, and another PC to tell a bad Kobo socket from a bad cable. Success = a drive labeled `KOBOeReader` appears.

## Decisions (and why)

- **Update the firmware to the latest 4.38.** The user chose this. 4.38 is the final branch for older Kobos. 4.38.23684 is confirmed for the Touch 2.0, and 4.38.23828/23852 (Sept 2026) went to "all touch Kobos except Mini". Check what's actually offered for N587 on the pgaskin firmware page.
- **Launcher: NickelMenu only.** Recommended by the KOReader wiki and survives firmware updates.
  - **No KFMon.** Users report it breaks after every firmware update, and two launchers can conflict.
  - **No KSM.** Deprecated, and broken on firmware 4.25+.
- **No auto-boot into KOReader.** There's no supported method. Put the device to sleep from KOReader instead of powering off, so it wakes straight into KOReader. An optional boot-script hack is possible later, but it's unsupported and a firmware update wipes it.
- **KOReader package:** use `koreader-kobo-*.zip`, **not** `kobov5`, which is for firmware 5.x devices.

## Versions (checked 2026-11-06)

| Component | Version | Source |
|---|---|---|
| KOReader | v2026.07.1 (`koreader-kobo-v2026.07.1.zip`) | https://github.com/koreader/koreader/releases |
| NickelMenu | v0.6.0 (`KoboRoot.tgz`, Dec 2025) | https://github.com/pgaskin/NickelMenu/releases |
| Firmware | **4.38.23852** (Oct 2026). Kobo's update server offers this to the Touch 2.0 (device ID `...372`, `kobo6`) for both the `kobo` and `fnac` affiliates | `https://ereaderfiles.kobo.com/firmwares/kobo6/Oct2026/kobo-update-4.38.23852.zip` |

### Downloads (local only, gitignored; re-download on a new PC)
- `koreader-kobo-v2026.07.1.zip`: `https://github.com/koreader/koreader/releases/download/v2026.07.1/koreader-kobo-v2026.07.1.zip`, sha256 `0f36a62ce73b12516f969e4ad7862cc06920afb03c7bd4db29a0d990bdd84b2f`
- `nickelmenu-v0.6.0/KoboRoot.tgz`: `https://github.com/pgaskin/NickelMenu/releases/download/v0.6.0/KoboRoot.tgz`, sha256 `322ff9aa863860e8f5f7e0b55cae561c54bf95983b9bce1d19819d1225d064af`
- The firmware zip was deleted, since the device updated over Wi-Fi instead

Recheck for newer releases right before installing (GitHub API: `repos/<owner>/<repo>/releases/latest`; Kobo API: `https://api.kobobooks.com/1.0/UpgradeCheck/Device/00000000-0000-0000-0000-000000000372/<affiliate>/0.0/N0`).

## Steps

### 0. Connect
- [ ] Battery ≥ 20% (≥ 30–40% before the firmware update)
- [ ] User plugs in the Kobo and taps **Connect**
- [ ] Detect the drive (label `KOBOeReader`) via `powershell.exe Get-Volume`, then mount it under `/mnt/<letter>`
- [ ] Read `.kobo/version` (serial, firmware, model ID) and `.kobo/Kobo/Kobo eReader.conf`, and record them here

### 1. Backup (before touching anything)
- [ ] Copy the entire drive, **including hidden `.kobo/`**, to `backup/YYYY-MM-DD/`
- [ ] Make sure the copy includes `.kobo/KoboReader.sqlite` (library and reading database) and `.kobo/Kobo/Kobo eReader.conf`
- [ ] Verify the file count and size match the device

### 2. Firmware update to 4.38
- [x] Done over Wi-Fi by the user (2026-11-06). The USB method isn't needed
- [ ] Confirm the version in `.kobo/version` on the first connection

### 3. Skip the Kobo account (if it's stuck on setup or signed out)
- [ ] Back up `KoboReader.sqlite`, then insert a dummy user:
      `INSERT INTO user (UserID, UserKey) VALUES ('1', '');`
- [ ] Eject. It should go straight to the home screen without login

### 4. NickelMenu
- [ ] Copy `KoboRoot.tgz` (NickelMenu) to `.kobo/KoboRoot.tgz`
- [ ] Eject and unplug. It installs and reboots
- [ ] Reconnect. `.adds/nm/` should exist

### 5. KOReader
- [ ] Unzip `koreader-kobo-*.zip`, then copy the `koreader/` folder to `.adds/koreader/`
- [ ] Create `.adds/nm/koreader` containing:
      `menu_item:main:KOReader:cmd_spawn:quiet:exec /mnt/onboard/.adds/koreader/koreader.sh`
- [ ] Stop Nickel from indexing hidden folders, which is **required** on firmware 4.17+. In `.kobo/Kobo/Kobo eReader.conf`, under `[FeatureSettings]`, add:
      `ExcludeSyncFolders=(\\.(?!kobo|adobe).+|([^.][^/]*/)+\\..+)`
- [ ] Eject. Test: main menu → KOReader

### 6. Debloat and cleanup
- [ ] Turn off store recommendations, sync and analytics in Kobo settings (Nickel), and leave Wi-Fi off when not needed
- [ ] Remove preloaded sample books, unused dictionaries and leftover junk (check the backup first)
- [ ] Keep the library small and tidy, since a big library slows Nickel's start and indexing

### 7. KOReader configuration (speed and comfort)
- [ ] Start with the last opened file, so it's reading in about a second
- [ ] Tune the full-refresh interval (every 6–8 pages is a good balance on this panel)
- [ ] Turn off unused plugins (fewer plugins = lighter)
- [ ] Gestures: tap zones for page turns, swipe to open the menu
- [ ] Sleep behavior: sleep via the power slider from inside KOReader

### 8. Extras (the most widely used, built in unless noted)
- [ ] **Calibre wireless connection** (built-in): send books from Calibre over Wi-Fi
- [ ] **Statistics** (built-in): reading time and pages per day
- [ ] **Cover browser** (built-in): file manager with covers
- [ ] **Progress sync** (built-in): sync reading position with the Kindle or a phone running KOReader
- [ ] **OPDS catalogs** (built-in): Project Gutenberg and Standard Ebooks, downloaded directly
- [ ] **Wallabag** (built-in, optional): read web articles later
- [ ] **SSH** (built-in): lets Claude manage the device over Wi-Fi later
- [ ] **App Store plugin** (community, `omer-faruq/appstore.koplugin`): install and update other plugins from the device. Optional, so check it's maintained first
- [ ] Offline dictionaries (StarDict format) for the user's languages
- [ ] On the PC: Calibre (optional), to manage the library and convert to KEPUB

## Log

- 2026-11-06: Research done. Device charging from deep discharge (was at 5%). Waiting to reach ~20%.
- 2026-11-06: Firmware 4.38.23852, KOReader v2026.07.1 and NickelMenu v0.6.0 downloaded and verified. Waiting for 30% battery.
- 2026-11-06: Firmware updated over Wi-Fi by the user. USB blocked: 3 cables, Kobo says connected but Windows sees nothing. The user will find a data cable. Firmware zip deleted.
