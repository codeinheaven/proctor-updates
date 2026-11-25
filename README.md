# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.8-111111?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon%20%2B%20Intel-111111?style=for-the-badge&logo=apple)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-GAP--01..21%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified cross-platform releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.8` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.7`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.7), [`v2.1.6`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.6), [`v2.1.5`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.5), [`v2.1.4`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.4), [`v2.1.3`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.3), [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2), and [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked immediately at startup (`Hard Security Gate`)** whenever `agent_version.json` publishes a newer mandatory release. Running an older build automatically initiates a SHA-256 verified self-upgrade to `v2.1.8` or exits (`Exit Code 1`) if declined.

| Platform | Artifact | Format | Size | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `3,008,512` B | `6fde78b56e14c14bf6879d91fb10cf1abd8492e5acd699238d8478b9fd4a6362` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.8/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `618,465` B | `247e98699b6fb674a091f27ff926ed5860dec209fcf8c517b9209db30800821c` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.8/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(Universal)* | **macOS Installer** *(Recommended)* | `.pkg` | `758,586` B | `14728e7b3d13f17234c933b6de3f5ec9e5476fac1f7d6f1f4e863c036be6e09d` | [Download `SkillifyProctoringAgent-macOS.pkg`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.8/SkillifyProctoringAgent-macOS.pkg) |
| **macOS** *(Universal)* | **Portable `.app` Bundle** | `.zip` | `755,557` B | `df01caeb458f9e2def781a5a6399cb52be7e86442d4b6898955e7f33cb662c15` | [Download `SkillifyProctorAgent-macOS.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.8/SkillifyProctorAgent-macOS.zip) |

---

## 🛡️ Security & UI Overview (`v2.1.8`)

| Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Matte Black `Quit Agent` UI** | Updated `Quit Agent` buttons across Windows Status/Login dialogs (`RGB(18, 18, 20)` with `RGB(72, 72, 78)` border, `RGB(36, 36, 40)` pressed, white text) and macOS Status HUD/Login `NSAlert` modals (`0.06..0.08` black bezel/fill, white bold text). |
| **Windows SEB `RuntimeWindow` Fix** | Excludes `SafeExamBrowser.exe`'s borderless WPF `RuntimeWindow` (`"SEB is running."`, `850x500`) from `sebHwnd` foreground locking, dismisses it (`HWND_BOTTOM` + `SW_HIDE`) immediately once `SafeExamBrowser.Client.exe` opens its browser window, focuses the exam browser cleanly, and runs a fast `250ms` initial startup sweep. |
| **Zero-Loophole macOS Daemon Gate** | Uses macOS Security.framework `SecCodeCopyGuestWithAttributes` (`anchor apple`) + `sysctl(KERN_PROC_PID)` (`SZOMB` & `cr_uid < 500`) to permit authentic idle Apple system daemons (`/usr/libexec/replayd`, `ContextStoreAgent`, `fairplayd`, `AirPlayUIAgent`, `AirPlayXPCHelper`, WebKit XPC workers) while strictly blocking imposter binaries, `screensharingd`, `screencapture`, `screencaptureui`, `Screenshot`, `QuickTime`, active AirPlay Mirroring displays, and established remote streams (`remotePort > 0`). |
| **Mandatory Version Lockout** | Pre-login `agent_version.json` enforcement compares semantic version against `kCurrentAgentVersion` (`2.1.8`). Older builds (`v2.1.7` and earlier) upgrade automatically over SHA-256 HTTPS or exit cleanly if declined. |

Copyright © 2026 **Skillify AI**. All rights reserved.
