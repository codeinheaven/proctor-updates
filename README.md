# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.2-0969DA?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon%20%2B%20Intel-111111?style=for-the-badge&logo=apple)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-Passes%201--9%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified cross-platform releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.2` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked at startup and pre-login (`Hard Security Gate 0`)** whenever a newer release is published in `agent_version.json`. Attempting to run an older build automatically initiates SHA-256 verified self-upgrade or terminates execution (`Exit Code 1`).

| Platform | Artifact | Format | Size | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `2,995,200` B | `0a4f78f8d52f05d1f435cd568f90ef6c3f5585f9b3ca4ed69a9de23feae2ec7e` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.2/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `615,107` B | `1a699530cd774baf0a917d14f768ab6be3a89b0fce8e8756b8af05fada9337d6` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.2/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(Universal)* | **macOS Installer** *(Recommended)* | `.pkg` | `744,823` B | `55045d830ba3b939c10af88897a12a9ce7bf0c7b8fcf5088a5eac01472d58583` | [Download `SkillifyProctoringAgent-macOS.pkg`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.2/SkillifyProctoringAgent-macOS.pkg) |
| **macOS** *(Universal)* | **Portable `.app` Bundle** | `.zip` | `743,107` B | `a86cadf7749fc2fc64474df96a36471e3e6749c636050bb2ca9eb0b1144c72a5` | [Download `SkillifyProctorAgent-macOS.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.2/SkillifyProctorAgent-macOS.zip) |

---

## 🛡️ Security Architecture Overview (Audited Passes 1–9)

| Security Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Kernel & Syscall Anti-Tamper** | Direct x64 syscalls (`syscalls_x64.asm`) + Halo's Gate SSN resolution; `.text` integrity hashing for agent & `ntdll.dll`; pre-main TLS callback debugger checks; macOS `_dyld_image_count` / `RTLD_DEFAULT` / timing checks; 150ms watchdog & `NtTerminateProcess` / `PT_DENY_ATTACH`. |
| **Process Containment & Entitlements** | Windows `SetProcessMitigationPolicy` (`DynamicCode`, `Signature`, `ImageLoad`, `ExtensionPointDisable`, `StrictHandleCheck`), strict own-process DACL, Job Object containment, EAT honeypots; macOS Hardened Runtime `entitlements.plist` (forbids DYLD injection, unsigned libraries, `get-task-allow`) + `RLIMIT_CORE=0`. |
| **Anti-Decompilation & Binary Armor** | FNV-1a salted 64-bit SplitMix64 + bit-rotation cipher behind `volatile` anti-folding barriers (`OBF_STR`/`OBF_WSTR`), per-build random 64-bit encrypted `proctor_config.json` with immediate RAM zero-wipe, `/GR-`, `/guard:cf`, `/LTCG`, `/DEBUG:NONE` (zero `.pdb`/`RSDS`) and macOS `strip -x -S` + `-Wl,-dead_strip -Wl,-x`. |
| **Network, DNS & TLS Lockdown** | Default-deny WFP (`svchost.exe`-scoped DNS) / macOS `pfctl` firewall rules; `/etc/hosts` + Cloudflare/Google DoH DNS integrity checks; TLS 1.2/1.3 SPKI pinning + mTLS; `clientVersion` reported on session claim & every heartbeat. |
| **Safe Exam Browser & Anti-Cheat** | Canonical path & publisher verification (Windows Authenticode + macOS Team ID `6F38DNSC7X`), Safe Exam Browser latest-version enforcement (`SEB_3.10.3.960_SetupBundle.exe` / macOS `3.7.2`) with built-in WinHTTP TLS downloader + `Downloads` Authenticode/PE version gate, stealth overlay & `kCGWindowSharingNone` neutralization, screen capture / injected renderer DLL scanning, VM & TPM 2.0 attestation. |
| **Cryptographic Telemetry & Self-Update** | AES-256-GCM encrypted & SHA-256 hash-chained audit seal, constant-time HMAC-SHA256 cloud manifest & remote kill-switch verification with replay nonce cache, HTTPS-only `agent_version.json` enforcement, `FILE_SHARE_READ` anti-TOCTOU lock, and verified silent upgrade (`posix_spawn` on macOS). |

---

## 🧪 Cross-Platform Upgrade Verification (`v2.1.1` → `v2.1.2`)

1. Download & launch **[`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)** (Windows or macOS) → blocked immediately at startup before login, shows `Mandatory Update Required: v2.1.2`, downloads & SHA-256-verifies `v2.1.2`, and upgrades cleanly (or hard-exits `exit code 1` if upgrade/permissions are declined).
2. Download & launch **[`v2.1.2` (Latest)](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2)** → matches `agent_version.json` (`2.1.2 == 2.1.2`) and proceeds cleanly into examination setup.

Copyright © 2026 **Skillify AI**. All rights reserved.
