# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.3-0969DA?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon%20%2B%20Intel-111111?style=for-the-badge&logo=apple)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-GAP--01..21%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified cross-platform releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.3` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2) and [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked immediately at startup (`Hard Security Gate`)** whenever `agent_version.json` publishes a newer mandatory version. Running an older build automatically initiates a SHA-256 verified self-upgrade to `v2.1.3` or exits (`Exit Code 1`) if declined.

| Platform | Artifact | Format | Size | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `2,995,200` B | `ada0b1ec6676f64a0453ced7e8472852e990438cdce78589b6560ee0ddbd557b` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.3/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `615,083` B | `8726b3a9b1a9ca52923d6955005ab237bf21319860534b3dc7ecfaee19f43857` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.3/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(Universal)* | **macOS Installer** *(Recommended)* | `.pkg` | `744,837` B | `caa4cff3a7e03ed89e34601e575b87b42ff26697888139bc35aaa80aa919b2bb` | [Download `SkillifyProctoringAgent-macOS.pkg`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.3/SkillifyProctoringAgent-macOS.pkg) |
| **macOS** *(Universal)* | **Portable `.app` Bundle** | `.zip` | `743,145` B | `c4166151c818c869405c38b38cdb75894ebdfee12063fa2ba653ea4ab032e330` | [Download `SkillifyProctorAgent-macOS.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.3/SkillifyProctorAgent-macOS.zip) |

---

## 🛡️ Security Architecture Overview (`v2.1.3`)

| Security Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Mandatory Version Lockout** | Pre-login `agent_version.json` enforcement compares semantic version against `kCurrentAgentVersion` (`2.1.3`). Older builds (`v2.1.2`, `v2.1.1`, etc.) cannot enter login/exam setup and either SHA-256 upgrade to `v2.1.3` or terminate immediately. |
| **Safe Exam Browser & Downloader** | Rejects cached/unverified `Downloads\*SEB*` installers via `ETH Zürich` Authenticode + PE `ProductVersion` checks; built-in WinHTTP TLS 1.2/1.3 progress downloader pulls `SEB_3.10.3.960_SetupBundle.exe` directly even under WFP lockdown (`3.7.2` on macOS). |
| **Anti-Decompilation & Binary Armor** | FNV-1a salted 64-bit SplitMix64 + bit-rotation cipher behind `volatile` anti-folding barriers (`OBF_STR`/`OBF_WSTR`), per-build random 64-bit encrypted `proctor_config.json` with immediate RAM zero-wipe, `/GR-`, `/guard:cf`, `/LTCG`, `/DEBUG:NONE` (zero `.pdb`/`RSDS`) and macOS `strip -x -S` + `-Wl,-dead_strip -Wl,-x`. |
| **Kernel & Syscall Anti-Tamper** | Direct x64 syscalls (`syscalls_x64.asm`) + Halo's Gate SSN resolution; `.text` integrity hashing for agent & `ntdll.dll`; pre-main TLS callback debugger checks; macOS `_dyld_image_count` / `RTLD_DEFAULT` / timing checks; 150ms watchdog & `NtTerminateProcess` / `PT_DENY_ATTACH`. |
| **Network, DNS & TLS Lockdown** | Default-deny WFP (`svchost.exe`-scoped DNS) / macOS `pfctl` firewall rules; `/etc/hosts` + Cloudflare/Google DoH DNS integrity checks; TLS 1.2/1.3 SPKI pinning + mTLS; `clientVersion` reported on session claim & every heartbeat. |
| **Cryptographic Telemetry & Self-Update** | AES-256-GCM encrypted & SHA-256 hash-chained audit seal, constant-time HMAC-SHA256 cloud manifest & remote kill-switch verification with replay nonce cache, `FILE_SHARE_READ` anti-TOCTOU lock, and verified silent upgrade (`posix_spawn` on macOS). |

---

## 🧪 Cross-Platform Upgrade Verification (`v2.1.2` / `v2.1.1` → `v2.1.3`)

1. Launch any installed older build (**[`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2)** or **[`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)**) on Windows or macOS → immediately blocked before login, displays mandatory update to `v2.1.3`, downloads & SHA-256-verifies `v2.1.3`, and upgrades cleanly (or exits `1` if declined).
2. Launch **[`v2.1.3` (Latest)](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.3)** → matches `agent_version.json` (`2.1.3 == 2.1.3`) and proceeds cleanly into examination setup.

Copyright © 2026 **Skillify AI**. All rights reserved.
