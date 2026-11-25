# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.4-10A34A?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon%20%2B%20Intel-111111?style=for-the-badge&logo=apple)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-GAP--01..21%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified cross-platform releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.4` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.3`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.3), [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2), and [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked immediately at startup (`Hard Security Gate`)** whenever `agent_version.json` publishes a newer mandatory release. Running an older build automatically initiates a SHA-256 verified self-upgrade to `v2.1.4` or exits (`Exit Code 1`) if declined.

| Platform | Artifact | Format | Size | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `2,997,248` B | `ecc58dde2016484df70d6fa8831945832c6b88e4dfbf27318a4e8feb9241ff42` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.4/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `615,735` B | `a75415cc346d8f974368e967a883de8fd93c6276b4ae8bbd1c75273111b5cd00` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.4/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(Universal)* | **macOS Installer** *(Recommended)* | `.pkg` | `745,038` B | `7483ba2cb1a42cabcbb59cd48ab6445bf3901889910735df7d45a9d4c7efbdaf` | [Download `SkillifyProctoringAgent-macOS.pkg`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.4/SkillifyProctoringAgent-macOS.pkg) |
| **macOS** *(Universal)* | **Portable `.app` Bundle** | `.zip` | `743,267` B | `b1280ea73c5e695e59924d0d82789bd65fac41439ff56a0cdb1ca1b546a7c45e` | [Download `SkillifyProctorAgent-macOS.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.4/SkillifyProctorAgent-macOS.zip) |

---

## 🛡️ Security & UI Overview (`v2.1.4`)

| Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Mandatory Version Lockout** | Pre-login `agent_version.json` enforcement compares semantic version against `kCurrentAgentVersion` (`2.1.4`). Older builds (`v2.1.3`, `v2.1.2`, `v2.1.1`) cannot enter login/exam setup and either SHA-256 upgrade to `v2.1.4` or terminate immediately. |
| **Updated Cross-Platform UI** | Emerald green **`Quit Agent`** buttons across both the Status Window (`RGB(34, 165, 88)` owner-drawn on Windows; matching layer green on macOS) and Candidate Login modal for instant visual update verification. |
| **Safe Exam Browser & Downloader** | Rejects cached/unverified `Downloads\*SEB*` installers via `ETH Zürich` Authenticode + PE `ProductVersion` checks; built-in WinHTTP TLS 1.2/1.3 progress downloader pulls `SEB_3.10.3.960_SetupBundle.exe` directly even under WFP lockdown (`3.7.2` on macOS). |
| **Anti-Decompilation & Binary Armor** | FNV-1a salted 64-bit SplitMix64 + bit-rotation cipher behind `volatile` anti-folding barriers (`OBF_STR`/`OBF_WSTR`), per-build random 64-bit encrypted `proctor_config.json` with immediate RAM zero-wipe, `/GR-`, `/guard:cf`, `/LTCG`, `/DEBUG:NONE` (zero `.pdb`/`RSDS`) and macOS `strip -x -S` + `-Wl,-dead_strip -Wl,-x`. |
| **Kernel & Syscall Anti-Tamper** | Direct x64 syscalls (`syscalls_x64.asm`) + Halo's Gate SSN resolution; `.text` integrity hashing for agent & `ntdll.dll`; pre-main TLS callback debugger checks; macOS `_dyld_image_count` / `RTLD_DEFAULT` / timing checks; 150ms watchdog & `NtTerminateProcess` / `PT_DENY_ATTACH`. |
| **Network, DNS & Telemetry** | Default-deny WFP / macOS `pfctl` firewall rules, `/etc/hosts` + Cloudflare/Google DoH DNS integrity checks, TLS 1.2/1.3 SPKI pinning + mTLS, AES-256-GCM hash-chained audit seal, constant-time HMAC-SHA256 cloud manifest verification & zero-TOCTOU updater. |

---

## 🧪 Cross-Platform Upgrade Verification (`v2.1.3` / `v2.1.2` → `v2.1.4`)

1. Launch any installed older build (**[`v2.1.3`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.3)**, [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2), or [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) on Windows or macOS → blocked pre-login, upgrades cleanly to `v2.1.4` with SHA-256 verification (or hard-exits `exit code 1` if declined).
2. Launch **[`v2.1.4` (Latest)](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.4)** → matches `agent_version.json` (`2.1.4 == 2.1.4`) and visibly renders the **green `Quit Agent` button** on both Status Window and Login dialog.

Copyright © 2026 **Skillify AI**. All rights reserved.
