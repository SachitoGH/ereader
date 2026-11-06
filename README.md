# E-readers workspace

Plans and logs for modding two e-readers. Each device folder has:

- `SETUP.md`: the plan, step by step, with a status checklist and a log at the bottom. **Read this first when resuming.**
- `backup/`: device backups (local only, **gitignored**, since they contain personal books)
- `downloads/`: firmware and packages (local only, **gitignored**; the URLs and sha256 checksums are in SETUP.md)

## Status (2026-11-06)

| Device | Firmware | Status |
|---|---|---|
| Kindle Paperwhite 11th gen (PW5) | 5.19.2 | ✅ **Done.** Jailbroken (SpiderCat), OTA blocked (Jobman), KOReader and KOmpanion installed and configured. Remaining: Progress sync with the Kobo |
| Kobo Touch 2.0 | latest 4.38 (updated over Wi-Fi) | ⏸️ **Blocked: no working USB data cable.** Then: backup, then NickelMenu and KOReader, then debloat |

Plans: [kindle/SETUP.md](kindle/SETUP.md) · [kobo/SETUP.md](kobo/SETUP.md)

## Notes for Claude

- Original PC: WSL2 on Windows. USB devices aren't visible to Linux. Use `powershell.exe` (`Get-PnpDevice`, `Get-Volume`, `robocopy`, `Copy-Item`). WSL paths from Windows: `\\wsl.localhost\<distro>\home\...`
- **Kindle PW5 mounts as USB mass storage (FAT32, got drive E:)**, not MTP. Wake and unlock it first, or the disk shows "No Media".
- Kobo = USB mass storage, label `KOBOeReader`, after tapping **Connect** on the device.
- Eject via PowerShell: `(New-Object -ComObject Shell.Application).NameSpace(17).ParseName('E:').InvokeVerb('Eject')`
- Always back up before modifying. Never factory-reset the Kindle (it loses the jailbreak).
- Only use trusted sources: kindlemodding.org, the official KPM repo (repo.kindlemodding.org), github.com/koreader, github.com/pgaskin. The user leaves out the ad-removal and piracy tools.
