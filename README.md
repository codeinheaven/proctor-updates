# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.7-10A34A?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon%20%2B%20Intel-111111?style=for-the-badge&logo=apple)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-GAP--01..21%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified cross-platform releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.7` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.6`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.6), [`v2.1.5`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.5), [`v2.1.4`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.4), [`v2.1.3`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.3), [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2), and [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked immediately at startup (`Hard Security Gate`)** whenever `agent_version.json` publishes a newer mandatory release. Running an older build automatically initiates a SHA-256 verified self-upgrade to `v2.1.7` or exits (`Exit Code 1`) if declined.

| Platform | Artifact | Format | Size | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `3,008,512` B | `f67271d05cc300c0e80bd4a143d5d8d211fa45d4f02834633e23ac69614132d5` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.7/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `618,463` B | `b0826b5ed85a31ec5028ade93ac7d0126c7657c787ef2f93bb239f599af299bd` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.7/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(Universal)* | **macOS Installer** *(Recommended)* | `.pkg` | `758,367` B | `2136dfa86e7b6cbc4df211fe98af2b8d554155fabe29aaf8c321827ec92aadc7` | [Download `SkillifyProctoringAgent-macOS.pkg`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.7/SkillifyProctoringAgent-macOS.pkg) |
| **macOS** *(Universal)* | **Portable `.app` Bundle** | `.zip` | `755,316` B | `651c7d66a71b6937dc871462be9dc262aab42b90deed4cebf0ed57a1af713409` | [Download `SkillifyProctorAgent-macOS.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.7/SkillifyProctorAgent-macOS.zip) |

---

## 🛡️ Security & UI Overview (`v2.1.7`)

| Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Windows SEB `RuntimeWindow` Fix** | Excludes `SafeExamBrowser.exe`'s borderless WPF `RuntimeWindow` (`"SEB is running."`, `850x500`) from `sebHwnd` foreground locking, dismisses it (`HWND_BOTTOM` + `SW_HIDE`) immediately once `SafeExamBrowser.Client.exe` opens its browser window, focuses the exam browser cleanly, and runs a fast `250ms` initial startup sweep during SEB launch handoff. |
| **Zero-Loophole macOS Daemon Gate** | Uses macOS Security.framework `SecCodeCopyGuestWithAttributes` (`anchor apple`) + `sysctl(KERN_PROC_PID)` (`SZOMB` & `cr_uid < 500`) to permit authentic idle Apple system daemons (`/usr/libexec/replayd`, `ContextStoreAgent`, `fairplayd`, `AirPlayUIAgent`, `AirPlayXPCHelper`, WebKit XPC workers) while strictly blocking imposter binaries, `screensharingd`, `screencapture`, `screencaptureui`, `Screenshot`, `QuickTime`, active AirPlay Mirroring displays, and established remote streams (`remotePort > 0`). |
| **Mandatory Version Lockout** | Pre-login `agent_version.json` enforcement compares semantic version against `kCurrentAgentVersion` (`2.1.7`). Older builds (`v2.1.6` and earlier) cannot enter login/exam setup and either SHA-256 upgrade to `v2.1.7` or terminate immediately. |
| **Updated Cross-Platform UI** | Emerald green **`Quit Agent`** buttons across both the Status Window (`RGB(34, 165, 88)` owner-drawn on Windows; matching layer green on macOS) and Candidate Login modal. |
| **Safe Exam Browser & Downloader** | Rejects cached/unverified `Downloads\*SEB*` installers via `ETH Zürich` Authenticode + PE `ProductVersion` checks; built-in WinHTTP TLS 1.2/1.3 progress downloader pulls `SEB_3.10.3.960_SetupBundle.exe` directly even under WFP lockdown (`3.7.2` on macOS). |
| **Anti-Decompilation & Kernel Armor** | SplitMix64 `OBF_STR`/`OBF_WSTR` cipher, per-build random 64-bit encrypted `proctor_config.json` with immediate RAM zero-wipe, direct x64 syscalls + Halo's Gate, WFP / `pfctl` firewall + DoH DNS integrity, AES-256-GCM hash-chained telemetry & zero-TOCTOU updater. |

---

## 🧪 Cross-Platform Upgrade Verification (`v2.1.6` → `v2.1.7`)

1. Launch any installed older build (**[`v2.1.6`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.6)** or earlier) on Windows or macOS → blocked pre-login, upgrades cleanly to `v2.1.7` with SHA-256 verification (or hard-exits `exit code 1` if declined).
2. Launch **[`v2.1.7` (Latest)](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.7)** → matches `agent_version.json` (`2.1.7 == 2.1.7`), cleanly dismisses SEB's `"SEB is running."` RuntimeWindow as soon as the SEB browser window renders on Windows, and retains full `v2.1.6` macOS protections.

Copyright © 2026 **Skillify AI**. All rights reserved.
