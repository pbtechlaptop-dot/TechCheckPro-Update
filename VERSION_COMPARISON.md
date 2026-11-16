# TechCheck Pro - Version Comparison

v2.8.8 / v2.8.9 (WinForms) vs v3.0.7 (WPF)

**Same as old:** 43 | **New in v3.0.x:** 21 | **Missing:** 0 | **Changed:** 6


## UI & Layout

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 1 | Framework | WinForms (.NET 4.8), dark theme, single window | WPF (.NET 4.8), dark / light theme, tabbed navigation | Changed |
| 2 | Navigation | All content in one scrollable window; Testing / Diagnostics / System cards always visible side by side | Bottom nav bar: Overview / Testing / Diagnostics / System Setting tabs; only one tab visible at a time | Changed |
| 3 | Hero / header bar | Top bar: TECHCHECK PRO + version + Battery + Model + SN + Time all as labels; status row below (Speaker / Mic / Secure Boot / Wi-Fi / BT / Internet / TPM / USB etc.) | Blue hero card (fixed 92 px): logo + v3.0.7 badge, model, Serial, Battery (green when charging / AC, red when discharging), CPU / NVMe temp, time; Shutdown / Restart / Refresh / UEFI / theme buttons on the title row | Changed |
| 4 | Theme | Dark (navy/slate), Cyan accent, single palette | Dark by default (navy/slate, same bright per-row colours as the old tool) or light (white cards, blue accent); switch with the moon / sun button | Changed |

## Data Display

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 5 | Model name | Header label (top bar) | Hero card (large bold text) | Same |
| 6 | Serial number | Header label | Hero card meta row | Same |
| 7 | Battery status | Shows %, charging state (Charging / AC Connected / Discharging / Full) | Same — now also shows ⚡ for Charging and handles code 11 (Partially Charged) | Same |
| 8 | CPU temperature | Top status row: "CPU: 30 °C  NVMe: -- °C" | Hero meta row: "CPU: 30 °C   NVMe: -- °C"; shows both CPU and NVMe | Same |
| 9 | NVMe temperature | ✓ Shown in top bar alongside CPU | ✓ Shown alongside CPU in hero meta row (via MSFT_StorageReliabilityCounter) | Same |
| 10 | NZ time (clock) | Updates every 1 second via clockTimer | Updates every 1 second via _clockTimer + Dispatcher.Invoke | Same |
| 11 | Windows OS / activation | Shown in OS activation card | Shown in Windows status tile (Activated / Not Activated) | Same |
| 12 | Internet status | Top status row label | Status tile (Connected / No Internet) | Same |
| 13 | Secure Boot | Top status row label | Status tile (On / Off) | Same |
| 14 | Driver issues | Driver label clickable — opens driver issue list | Drivers status tile (All OK / N issues) — clickable; opens Driver Issues tab (red/yellow rows by error code) + full Windows Update tab (Scan / Select All / Install Selected, ported from v2.8.8) | Same |
| 15 | Hardware specs panel | RichTextBox (Battery, Processor, RAM, Graphics, Resolution, Storage, OS) in dark colours | Hardware & System card, font 12: Battery first, then CPU, RAM, GPU, Storage, Display, Camera, OS, Serial. RAM / GPU / Display / OS text and per-row colours ported from v2.8.8; Battery Good (≥70%) or red Fail; long values collapse with a ▾ expand arrow; rows fill in as each group loads | Same |
| 16 | Devices & Peripherals | Top status row labels; refreshed every 5 s | Dedicated 2-column card (left: TPM, Speaker, Microphone, Wi-Fi, Camera; right: Enrollment, Bluetooth, USB Drive, USB Keyboard, USB Mouse); Speaker/Mic show Enabled / Disabled via ported AudioCore (default endpoint, mute, registry state); Enrollment shows last scan result; refreshed every 5 s | Same |
| 17 | User account label | ✓ Shown in status row ("OEM" / domain user) | Nav band right shows username | Same |
| 18 | Last Sync time | Top status row label | Nav band right label | Same |
| 19 | Monitor hours | ✓ "Monitor Hours" button in Diagnostics | ✓ "Monitor Hours" button in Diagnostics (DDC/CI) | Same |

## Auto-Refresh & Timers

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 20 | Status refresh interval | Every 5 seconds (refreshTimer) | Every 5 seconds (_refreshTimer) | Same |
| 21 | What refreshes automatically | Internet, USB disk/KB/mouse, Wi-Fi, BT, speaker, mic, TPM, temp, battery, secure boot, drivers | Internet, USB disk/KB/mouse, Wi-Fi, BT, speaker, mic, TPM, camera, temp, battery + full Devices & Peripherals panel rebuild | Same |
| 22 | Clock update | Every 1 second | Every 1 second (via Dispatcher.Invoke) | Same |
| 23 | Auto time sync on startup | ✓ Silently syncs NZ time 2s after startup | ✓ SyncAucklandTimeOnStartup() called on Window_Loaded (silent, background) | Same |

## Testing Features

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 24 | Speaker test | Button + volume slider (TrackBar, default 50) | Button + volume slider (default 80); sub-window with status label | Same |
| 25 | Microphone test | 3-second mic test button + mic monitor window | Quick tile: records 3s then auto-plays back (MCI waveaudio, same as v2.8.8; tile shows Recording… / Playing back… / Done ✓ / Failed); Testing tab opens full mic monitor window | Same |
| 26 | Keyboard test | Full visual keyboard layout (WinForms Canvas), low-level hook, keys light up | Full visual keyboard layout (WPF Canvas), low-level hook (key-down + key-up, PrtSc works), keys flash dark green then stay green-grey; mouse panel placed below the space-bar row; swallows keys so Enter / arrows don't trigger main-window buttons; light theme | Same |
| 27 | Camera test | Camera preview window | Quick test tile — ported DirectShow preview window (Next Camera / Refresh / Windows Camera), single reusable window, resizes with window; falls back to Windows Camera app if preview fails | Same |
| 28 | USB test | USB test sub-window | Quick test tile — USB Test ported from v2.8.8: devices on the same physical port (incl. USB2/USB3 companion port) merge into one row; Debug Info button writes %TEMP%\TechCheckPro_usb_debug.txt; light theme | Same |
| 29 | Time sync | Button in Testing + auto silent sync on startup | Quick test tile + Time Sync button in Testing tab (manual only) | Same |
| 30 | Enrollment check | Button in Testing; result shown as label | Quick Tests tile + Diagnostics button — full v2.8.8 logic plus the local Autopilot profile file (tested on a company-bound PC). When offline it asks to connect the Assembly Wi-Fi first (No = check anyway, result marked offline / partial); detail window only when a binding is found | Same |
| 31 | Export SN (USB) | ✓ Appends to SN_List.txt in the folder the exe was opened from (row no., SN, TOUCH, battery, date; skips duplicates; newest on top) | Same behaviour and file name (SN_List.txt, same columns); battery column uses Good / Fail (70%); Quick test tile + button in Testing tab | Same |
| 32 | Export SN (SMB) | ✓ Export to SMB network share | ✓ ExportSmbBtn in Testing tab; ExportSnToSmb() with mapped drive | Same |
| 33 | Dead Pixel Test | ✓ Full-screen dead pixel form | Dead Pixel Test in Testing tab | Same |

## Diagnostics

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 34 | Health Check | ✓ NVMe health, power-on hours, wear, temp, storage speed | Split in two (same data as v2.8.8): NVMe / SSD Health (SMART, power-on hours, read/written, life used, wear, temp) and Battery Health (design / full capacity, health %, cycle count, legend) — each as a Quick Tests tile and a Diagnostics button | Changed |
| 35 | Storage speed test | ✓ StorageTestForm | ✓ "Storage Speed Test" in Testing tab (light theme) | Same |
| 36 | Winget upgrade | ✓ RunWingetUpgrade button | ✓ Quick Tests tile (row 3) + button in System tab; 2 passes, admin; finds winget.exe path, warns if missing; command window closes after Enter like v2.8.8 | Same |
| 37 | Test Checklist | ✓ ChecklistForm | Quick Tests → Tools tile + Testing tab card; Pass / Fail per item, click again to clear, Reset All, summary line (ported) | Same |
| 38 | Mic Monitor (standalone) | ✓ "Start Mic Monitor" in Diagnostics | Accessible via Mic test flow | Same |
| 39 | Web Tests (UFO/KB/Webcam) | ✓ Opens browser links | ✓ "Open Web Tests" in Testing tab | Same |
| 40 | Share Settings | ✓ SMB share config UI | ✓ Share & network settings in System tab; saved to share_settings.txt next to the exe and reloaded at startup (old tc_settings.json is not read) | Same |

## System Setting

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 41 | Shutdown / Restart / UEFI | Buttons in top header (powerRow) | Buttons in hero card (Shutdown / Restart / ⟳ Refresh / UEFI) | Same |
| 42 | Scan Drivers | ✓ System Setting card | System Setting tab | Same |
| 43 | Wi-Fi Settings / Connect / Forget | ✓ All three in System Setting | System Setting tab | Same |
| 44 | Windows Update / Date & Time / Device Manager | ✓ System Setting card | System Setting tab | Same |
| 45 | Sysprep / Show SID | ✓ System Setting card; Sysprep asks Reboot / Shutdown | System tab; Sysprep asks Cancel / Reboot / Shutdown and runs /generalize /oobe … /quiet (ported); removes the local copy and the Assembly Wi-Fi profile first | Same |
| 46 | Check for Update | ✓ Auto-checks 4 s after startup, then when the network comes up (event + 30 s poll); manual button; copies EXE to LocalAppData and cleans up old versions | Same flow with its own files on GitHub: version_v3.txt, changelog_v3.txt, TechCheckPro.exe. Checks 4 s after start, keeps watching until the PC is online (network events + 15 s poll, stops after the version file is read), 3-part version compare, only a real exe may replace the USB copy, writes back to the original launch folder | Same |

## New in v3.0.x (not in v2.8.8)

| # | Feature | v2.8.8 / v2.8.9 | v3.0.7 | Status |
|---|---|---|---|---|
| 47 | UI framework | WinForms | WPF — XAML styles, smooth gradients, hero card, animated nav | New |
| 48 | Tab navigation | All in one window | Bottom nav bar (Overview / Testing / Diagnostics / System Setting) | New |
| 49 | Hero card | Plain label row | Blue gradient card with ECG icon, computer art, action buttons | New |
| 50 | Quick Tests grid | Buttons in Testing card | Overview page split in two panels: Quick Tests (Speaker, Microphone, Keyboard, Camera, USB Test, Enrollment) and Tools (Export SN, Time Sync, Connect Wi-Fi, Winget Upgrade, NVMe / SSD Health, Forget Wi-Fi, Test Checklist, Battery Health, Device Manager) — 15 tiles. Click once → tile turns green (tested), click again → reset | New |
| 51 | Status tile row | Labels in header | 4 inline tiles: Internet · Windows · Drivers · Secure Boot | New |
| 52 | Periph badge panel | Labels in status row | Devices & Peripherals card with colored dot badges | New |
| 53 | EXE icon | Default / custom .ico | Blue rounded icon with ECG waveform (16/32/48px) | New |
| 54 | Scroll layout | Fixed TableLayoutPanel rows | ScrollViewer per tab — content scrolls independently | New |
| 55 | Version label | Footer label bottom-right | v3.0.7 badge inside hero card; exe file version 3.0.7 | New |
| 56 | Compact hero for small screens | — | Hero banner fixed at 92 px, art scaled to fit, title and 4 buttons on one row; tighter page margins so 1080p @150% fits with less scrolling | New |
| 57 | Expandable hardware rows | — | Multi-line values in Hardware & System show one line + ▾ button to expand/collapse | New |
| 58 | Battery pass / fail colour | "Good" ≥70%, else "Fair" (always green) | "Good" ≥70% (green), "Fail" <70% (red) | New |
| 59 | Sub-window theme | Dark windows | All sub-windows (USB, Storage, Camera, Keyboard, Health, Checklist, Drivers…) follow the chosen theme (dark by default); check boxes, tab headers, list headers and disabled buttons are readable in both themes | New |
| 60 | Dark / light theme | — | Dark by default; moon / sun button in the hero toggles; choice saved in share_settings.txt on the USB; windows are themed before they are shown (no white flash); Dead Pixel window is never themed | New |
| 61 | Page layout | One scrolling window | Testing page 3 columns; Diagnostics and System Setting 2 columns; page side margins 8 px — no scrolling on 1080p @150% | New |
| 62 | Startup speed | All hardware shown at once, about 8 s after the window appears | Parallel loading, each Hardware row fills in as it is ready (CPU / RAM about 0.4 s, OS about 2 s); one shared device-list query; TPM queried once; peripherals checked in parallel | New |
| 63 | Nothing left on the PC | ✓ Local copy deleted on close | Runs from a local copy; on close, Shutdown, Restart, UEFI, Sysprep or logoff it deletes the copy, temp files and the Assembly Wi-Fi profile (if Connect Wi-Fi was used); delete-on-reboot as backup | New |
| 64 | USB Drive eject | — | Click USB Drive when it shows Connected ⏏ → confirm → lock / dismount / eject; shows Not found afterwards until the USB is unplugged and plugged back in | New |
| 65 | Windows status tile | Label only | Click the Windows tile to open Settings → System → Activation | New |
| 66 | Device Manager tile | Button in System Setting | Also a Tools tile on the Overview page | New |
| 67 | Model name (Lenovo) | Friendly name from ComputerSystemProduct.Version (e.g. Lenovo V14 G5 IRL) | Same logic ported (was showing the machine code 83HD) | Same |
| 68 | Bottom bar text | CPU / NVMe temperature, user, last sync | "Powered by Assembly", "User: <name>", last sync (temperature stays in the hero) | Changed |
| 69 | Old tool → v3 hand-over (v2.8.9) | v2.8.8: updates itself only | v2.8.9 offers to download TechCheck Pro v3 as TechCheckPro.exe next to the old exe (old tool keeps working); also offers newer v3 builds later. v2.8.9 exe, version.txt and changelog.txt are in GitHub_Upload\old_v2.8.9 | New |
| 70 | Camera detection (Devices & Peripherals) | Matches camera keywords in device name and description | Same matching, plus anything Windows lists under "Cameras" (fixes names like "ACER HD User Facing" showing Not found) | Same |
