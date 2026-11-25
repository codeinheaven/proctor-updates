# Skillify Proctoring Agent — Official Distribution & Integrity Channel

[![Release](https://img.shields.io/badge/Latest%20Release-v2.1.2-0969DA?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Platform](https://img.shields.io/badge/Windows-10%20%2F%2011%20x64-0078D6?style=for-the-badge&logo=windows)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Security Audit](https://img.shields.io/badge/Security%20Hardening-Passes%201--9%20Verified-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)
[![Validation Suite](https://img.shields.io/badge/Automated%20Checks-GAP--01%20to%20GAP--21%20PASS-1A7F37?style=for-the-badge)](https://github.com/codeinheaven/proctor-updates/releases/latest)

Official distribution channel, cryptographic SHA-256 update manifest (`agent_version.json`), and verified binary releases for **Skillify Proctoring Agent**.

---

## 📥 Official Downloads (`v2.1.2` — Latest Production Release)

> **Mandatory Version Policy:** Outdated releases (including [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1)) are **blocked at startup and pre-login (`Hard Security Gate 0`)** whenever a newer release is published in `agent_version.json`. Attempting to run an older build automatically initiates SHA-256 verified self-upgrade or terminates execution (`Exit Code 1`).

| Platform | Artifact | Format | SHA-256 Checksum | Download |
| :--- | :--- | :--- | :--- | :--- |
| **Windows x64** | **1-Click Installer** *(Recommended)* | `.exe` | `ef5b6159908ac0788efb87bf8441869924483261b49b55f1ea17357361a9c6a2` | [Download `SkillifyProctorSetup.exe`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.2/SkillifyProctorSetup.exe) |
| **Windows x64** | **Standalone Agent Bundle** | `.zip` | `24c6fa75d8c3a33e5d695c8ff362235e42a5c932f3efc24ca494dff75fe96594` | [Download `SkillifyProctoringAgent-Windows.zip`](https://github.com/codeinheaven/proctor-updates/releases/download/v2.1.2/SkillifyProctoringAgent-Windows.zip) |
| **macOS** *(arm64 + x86_64)* | **Universal Installer** | `.pkg` / `.zip` | *Built via macOS CI Runner* | Track [`Releases`](https://github.com/codeinheaven/proctor-updates/releases) |

---

## 🛡️ Security Architecture Overview (Audited Passes 1–9)

| Security Layer | Implemented Engineering Controls |
| :--- | :--- |
| **Kernel & Syscall Anti-Tamper** | Direct x64 syscalls (`syscalls_x64.asm`) + Halo's Gate SSN resolution; `.text` runtime integrity hashing for both agent and `ntdll.dll`; pre-main TLS callback debugger checks; 150ms watchdog & direct `NtTerminateProcess` termination. |
| **Process Containment & ACLs** | `SetProcessMitigationPolicy` (`DynamicCode`, `Signature`, `ImageLoad`, `ExtensionPointDisable`, `StrictHandleCheck`), restricted own-process DACL stripping `VM_WRITE` / `CREATE_THREAD`, Job Object breakaway containment, and EAT honeypot traps. |
| **Anti-Decompilation & Binary Armor** | Compile-time FNV-1a salted 64-bit SplitMix64 + bit-rotation cipher behind `volatile` anti-folding barriers (`OBF_STR`/`OBF_WSTR`), per-build random 64-bit encrypted `proctor_config.json` with immediate RAM zero-wipe, `/GR-`, `/guard:cf`, `/LTCG`, `/DEBUG:NONE` (zero `.pdb`/`RSDS` metadata). |
| **Network, DNS & TLS Lockdown** | Default-deny WFP (`Fwpuclnt`) / macOS `pfctl` firewall policy permitting only SEB & agent; loopback/public-IP hosts-file + DoH DNS integrity checks; TLS 1.2/1.3 SPKI pinning + mTLS support; `clientVersion` correlation on both session claim and heartbeats. |
| **Safe Exam Browser & Anti-Cheat** | Canonical path & publisher verification (Windows Authenticode + macOS Team ID `6F38DNSC7X`), Safe Exam Browser minimum latest-version check, screen capture / stealth overlay (`kCGWindowSharingNone` / layered hooks) neutralization, VM & hardware TPM 2.0 attestation. |
| **Cryptographic Telemetry & Self-Update** | AES-256-GCM encrypted & SHA-256 hash-chained local audit seal, constant-time HMAC-SHA256 cloud manifest/kill-switch verification with replay nonce cache, HTTPS-only `agent_version.json` enforcement, `FILE_SHARE_READ` anti-TOCTOU handle locking, and verified silent upgrade + UAC-safe relaunch. |

---

## 🔄 Auto-Update & Legacy Version Lockout Flow

1. Every launch checks [`agent_version.json`](https://raw.githubusercontent.com/codeinheaven/proctor-updates/main/agent_version.json) (over HTTPS-only WinHTTP / `curl --proto =https`).
2. If `currentVersion < version` (`mandatory: true`), login and Safe Exam Browser launch are **strictly blocked**.
3. The agent downloads the target installer, verifies its exact **SHA-256 digest** before locking the file against TOCTOU modification, silently replaces the outdated binary, and relaunches the latest version (`v2.1.2`).
4. If the upgrade download, hash verification, or UAC elevation fails, the outdated binary refuses entry (`Mandatory Update Required`) and terminates (`exit code 1`).

---

## 🧪 Version Verification Test Channel

Two active releases are intentionally published for end-to-end verification:
- [`v2.1.1`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.1) — Baseline build used to verify **automatic rejection & forced upgrade** when launching an older client.
- [`v2.1.2`](https://github.com/codeinheaven/proctor-updates/releases/tag/v2.1.2) *(Latest)* — Verified current build that passes startup & pre-login version gates cleanly.

---

Copyright © 2026 **Skillify AI**. All rights reserved. Distributed for proctored examination integrity.
