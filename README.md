# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.5-10A34A?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon%20%2B%20Intel-111111?style=for-the-badge&logo=apple)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-GAP--01..21%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified cross-platform releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.5` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.4`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.4), [`v2.1.3`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.3), [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2), and [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked immediately at startup (`Hard Security Gate`)** whenever `agent_version.json` publishes a newer mandatory release. Running an older build automatically initiates a SHA-256 verified self-upgrade to `v2.1.5` or exits (`Exit Code 1`) if declined.

| Platform | Artifact | Format | Size | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `3,002,368` B | `6d172df92a1875302cf85a7c9e64051ef247b691b343a32a1391aea296653c52` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.5/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `616,756` B | `0c7cd2ef52b7339e7b29eba8f2e473c5eec18d9fa5f6c44a863bf61e8e639e92` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.5/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(Universal)* | **macOS Installer** *(Recommended)* | `.pkg` | `754,458` B | `d9e8aedf00f255cdd0b7e60fda3d2efe9e37d48aa067fa16e50e362341e6ab79` | [Download `SkillifyProctoringAgent-macOS.pkg`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.5/SkillifyProctoringAgent-macOS.pkg) |
| **macOS** *(Universal)* | **Portable `.app` Bundle** | `.zip` | `751,955` B | `5e1bbab0060e472d3cae161b62e38c0bbedbaad03d60cf6ce84c068df0a4e8cb` | [Download `SkillifyProctorAgent-macOS.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.5/SkillifyProctorAgent-macOS.zip) |

---

## 🛡️ Security & UI Overview (`v2.1.5`)

| Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Zero-Loophole macOS Daemon Gate** | Uses macOS Security.framework `SecCodeCopyGuestWithAttributes` (`anchor apple`) + `sysctl(KERN_PROC_PID)` (`SZOMB` & `cr_uid < 500`) to verify authentic Apple background services (`ContextStoreAgent`, `fairplayd`, idle `AirPlayUIAgent`/`AirPlayXPCHelper`, WebKit XPC workers) without exempting imposter binaries, `screensharingd`, `screencapture`, active AirPlay Mirroring displays, or established AirPlay streams (`remotePort > 0`). |
| **Mandatory Version Lockout** | Pre-login `agent_version.json` enforcement compares semantic version against `kCurrentAgentVersion` (`2.1.5`). Older builds (`v2.1.4` and earlier) cannot enter login/exam setup and either SHA-256 upgrade to `v2.1.5` or terminate immediately. |
| **Updated Cross-Platform UI** | Emerald green **`Quit Agent`** buttons across both the Status Window (`RGB(34, 165, 88)` owner-drawn on Windows; matching layer green on macOS) and Candidate Login modal. |
| **Safe Exam Browser & Downloader** | Rejects cached/unverified `Downloads\*SEB*` installers via `ETH Zürich` Authenticode + PE `ProductVersion` checks; built-in WinHTTP TLS 1.2/1.3 progress downloader pulls `SEB_3.10.3.960_SetupBundle.exe` directly even under WFP lockdown (`3.7.2` on macOS). |
| **Anti-Decompilation & Kernel Armor** | SplitMix64 `OBF_STR`/`OBF_WSTR` cipher, per-build random 64-bit encrypted `proctor_config.json` with immediate RAM zero-wipe, direct x64 syscalls + Halo's Gate, WFP / `pfctl` firewall + DoH DNS integrity, AES-256-GCM hash-chained telemetry & zero-TOCTOU updater. |

---

## 🧪 Cross-Platform Upgrade Verification (`v2.1.4` / `v2.1.3` → `v2.1.5`)

1. Launch any installed older build (**[`v2.1.4`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.4)** or earlier) on Windows or macOS → blocked pre-login, upgrades cleanly to `v2.1.5` with SHA-256 verification (or hard-exits `exit code 1` if declined).
2. Launch **[`v2.1.5` (Latest)](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.5)** → matches `agent_version.json` (`2.1.5 == 2.1.5`), passes macOS pre-flight checks cleanly without flagging Apple daemons, and renders the green `Quit Agent` button.

Copyright © 2026 **Skillify AI**. All rights reserved.
