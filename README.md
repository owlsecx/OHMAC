# 🦉 OHMAC

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-OCipher%20%2F%20Message%20Authentication-cyan?style=flat-square"/>
  <img src="https://img.shields.io/badge/No%20Dependencies-Stdlib%20Only-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-1.0-cyan?style=flat-square"/>
</p>

> **OHMAC** is an HMAC generator and verifier — compute, verify, and compare HMAC signatures across 10 algorithms, bulk-sign entire directories into integrity manifests, and verify file integrity with timing-safe comparison.

---

## 📌 Overview

OHMAC wraps Python's `hmac` module with a full interactive interface — supporting all standard hash algorithms, multiple key and message input formats, three output formats (hex, Base64, Base64url), per-byte hex dump, and JSON report export to `ohmac_output/`.

All comparisons use `hmac.compare_digest()` — constant-time comparison immune to timing side-channel attacks.

---

## 🖥️ Modules

| # | Module | Description |
|---|--------|-------------|
| **[1] Generate HMAC** | Compute HMAC for a key + message — outputs hex, Base64, Base64url, and hex dump with computation time |
| **[2] Verify HMAC** | Timing-safe comparison of a provided expected HMAC against the computed value — clear VALID / INVALID verdict |
| **[3] Compare Algorithms** | Compute HMAC for the same key and message across all 10 algorithms simultaneously — with per-algorithm timing |
| **[4] Bulk File Sign** | Compute HMAC for every file in a directory (optionally recursive) — saves a signed JSON manifest |
| **[5] Verify Manifest** | Re-compute HMACs from a manifest and compare — flags each file as OK, TAMPERED, or MISSING |

---

## 🔑 Supported Algorithms

| # | Algorithm | Digest Size | Security Rating |
|---|-----------|-------------|----------------|
| 1 | MD5 | 128-bit | 🔴 LEGACY |
| 2 | SHA-1 | 160-bit | 🔴 LEGACY |
| 3 | SHA-224 | 224-bit | 🟡 ACCEPTABLE |
| 4 | SHA-256 | 256-bit | 🟢 RECOMMENDED |
| 5 | SHA-384 | 384-bit | 🟢 STRONG |
| 6 | SHA-512 | 512-bit | 🟢 STRONG |
| 7 | SHA3-256 | 256-bit | 🔵 MODERN |
| 8 | SHA3-512 | 512-bit | 🔵 MODERN |
| 9 | BLAKE2b | 512-bit | 🔵 MODERN |
| 10 | BLAKE2s | 256-bit | 🔵 MODERN (32-bit optimised) |

---

## 📥 Input Formats

### Key
| Source | Description |
|--------|-------------|
| Text | UTF-8 string |
| Hex | Raw hex bytes |
| Generated | `secrets.token_bytes(n)` — configurable length |
| File | Auto-detects hex, Base64, or text from file |

### Message
| Source | Description |
|--------|-------------|
| Text | UTF-8 string |
| Hex | Raw hex bytes |
| File | Binary file |

### Expected HMAC (Verify module)
Accepts Hex or Base64 format.

---

## 📤 Output Formats

Every HMAC result is shown in three formats simultaneously:

| Format | Description |
|--------|-------------|
| **Hex** | Standard lowercase hex string |
| **Base64** | Standard Base64 encoding |
| **Base64url** | URL-safe Base64 encoding |
| **Hex dump** | Per-byte hex + ASCII dump at 16 bytes/row |

---

## 📁 Bulk File Signing & Manifest Verification

**[4] Bulk File Sign** computes HMAC for every file in a directory and saves a manifest to `ohmac_output/ohmac_manifest_*.json`:

```json
{
  "tool": "OHMAC",
  "algorithm": "sha256",
  "key_sha256": "<SHA-256 of the key>",
  "ts": "...",
  "file_count": 12,
  "files": [
    { "file": "path/to/file", "size": 1024, "hmac_hex": "..." },
    ...
  ]
}
```

**[5] Verify Manifest** reloads the manifest, re-computes HMACs with the same key, and reports per file:

| Status | Meaning |
|--------|---------|
| **OK** | File is intact — HMAC matches |
| **TAMPERED** | File has been modified |
| **MISSING** | File no longer exists at the recorded path |

---

## 📤 JSON Reports

All outputs saved to `ohmac_output/`:

| Module | Filename | Key fields |
|--------|----------|-----------|
| Generate | `ohmac_<algo>_YYYYMMDD_HHMMSS.json` | algorithm, key_hex, message_hex, hmac_hex, hmac_b64 |
| Compare All | `ohmac_compare_YYYYMMDD_HHMMSS.json` | All 10 algorithms, hmac_hex, hmac_b64, timing (ms) |
| Bulk Sign | `ohmac_manifest_YYYYMMDD_HHMMSS.json` | algorithm, key SHA-256, file count, per-file HMAC |

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable
- **No external dependencies** — stdlib only

---

## 🚀 Usage

```bash
./OHMAC
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
