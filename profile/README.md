<div align="center">

```
 ██████╗ ██╗     ███████╗ ██████╗ ██████╗ ███████╗████████╗
 ██╔══██╗██║     ██╔════╝██╔════╝██╔═══██╗██╔════╝╚══██╔══╝
 ██████╔╝██║     █████╗  ██║     ██║   ██║███████╗   ██║
 ██╔═══╝ ██║     ██╔══╝  ██║     ██║   ██║╚════██║   ██║
 ██║     ███████╗███████╗╚██████╗╚██████╔╝███████║   ██║
 ╚═╝     ╚══════╝╚══════╝ ╚═════╝ ╚═════╝ ╚══════╝   ╚═╝
```

**Fully async · Zero-interaction · Built for professionals**

[![CI](https://github.com/Plecost/plecost/actions/workflows/ci.yml/badge.svg)](https://github.com/Plecost/plecost/actions)
[![PyPI](https://img.shields.io/pypi/v/plecost.svg?color=blue&label=PyPI)](https://pypi.org/project/plecost/)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11%2B-3776AB?logo=python&logoColor=white)](https://python.org)
[![Docker](https://img.shields.io/badge/Docker-ghcr.io%2Fplecost-2496ED?logo=docker&logoColor=white)](https://ghcr.io/plecost/plecost)
[![License: PolyForm NC](https://img.shields.io/badge/License-PolyForm%20NC-blue)](https://polyformproject.org/licenses/noncommercial/1.0.0/)

</div>

---

## What is Plecost?

Plecost is a **professional-grade WordPress security scanner** built from the ground up for automation, integration, and speed.

It detects vulnerabilities in WordPress **core, plugins, and themes** — enumerates users, identifies misconfigurations, detects WAFs, and correlates everything against a **daily-updated CVE database** (NVD).

Unlike other scanners, Plecost runs as a **Python library**, a **CLI tool**, or inside **Celery workers** — with a consistent, automation-friendly JSON output that fits directly into your pipelines.

---

## Key Capabilities

| | Feature | Detail |
|---|---|---|
| ⚡ | **Fully async** | Built on `httpx` + `asyncio` — all 15 modules run in parallel |
| 🔍 | **15 detection modules** | Fingerprint, WAF, plugins, themes, users, CVEs, misconfigs, headers, SSL/TLS, and more |
| 🛡️ | **Daily CVE database** | NVD-backed, incremental JSON patch system — always current |
| 🐍 | **First-class library API** | `from plecost import Scanner, ScanOptions` — Celery-compatible |
| 🐳 | **Docker native** | `ghcr.io/plecost/plecost` — multi-arch (amd64/arm64) |
| 🏷️ | **Stable finding IDs** | `PC-CVE-001`, `PC-MCFG-009` — safe to reference in dashboards and tickets |
| 🗄️ | **PostgreSQL support** | For production and team deployments |
| 🕵️ | **Stealth & aggressive modes** | Adapt to rate-limited or internal targets |

---

## Demo

```
$ plecost scan https://example.com

  Plecost v4.0 — WordPress Security Scanner
  Target: https://example.com

  [+] WordPress detected: 6.4.2
  [+] WAF detected: Cloudflare

  Plugins discovered (3)
  ┌─────────────────────┬─────────┬──────────────┐
  │ Plugin              │ Version │ Status       │
  ├─────────────────────┼─────────┼──────────────┤
  │ woocommerce         │ 8.2.1   │ ⚠ Vulnerable │
  │ contact-form-7      │ 5.8     │ ✓ OK         │
  │ elementor           │ 3.17.0  │ ✓ OK         │
  └─────────────────────┴─────────┴──────────────┘

  Findings (7)
  ┌───────────────┬──────────────────────────────────────────┬──────────┐
  │ ID            │ Title                                    │ Severity │
  ├───────────────┼──────────────────────────────────────────┼──────────┤
  │ PC-CVE-001    │ WooCommerce SQLi (CVE-2023-28121)        │ CRITICAL │
  │ PC-SSL-001    │ HTTP does not redirect to HTTPS          │ HIGH     │
  │ PC-HDR-001    │ Missing Strict-Transport-Security        │ MEDIUM   │
  │ PC-USR-001    │ User enumeration via REST API            │ MEDIUM   │
  │ PC-XMLRPC-001 │ XML-RPC interface accessible             │ MEDIUM   │
  │ PC-MCFG-009   │ readme.html discloses WordPress version  │ LOW      │
  │ PC-REST-001   │ REST API user data exposed               │ LOW      │
  └───────────────┴──────────────────────────────────────────┴──────────┘

  Summary: 1 Critical  1 High  3 Medium  2 Low
  Duration: 4.2s
```

---

## Quick Start

```bash
# Install
pip install plecost

# Update CVE database (first time)
plecost update-db

# Scan a target
plecost scan https://target.com

# JSON output for pipelines
plecost scan https://target.com --output report.json

# Docker (no install needed)
docker run --rm ghcr.io/plecost/plecost scan https://target.com
```

**Use as a Python library:**

```python
from plecost import Scanner, ScanOptions

result = await Scanner(ScanOptions(url="https://target.com")).run()

for finding in result.findings:
    print(f"[{finding.severity.value}] {finding.id}: {finding.title}")
```

---

## Repositories

<table>
<tr>
<td width="50%" valign="top">

### 🔬 [plecost](https://github.com/Plecost/plecost)

The scanner itself — CLI, Python library, and Docker image.

- 15 async detection modules
- Typer CLI with `scan`, `explain`, `update-db`
- Rich terminal output + JSON reporter
- 100+ unit, integration, contract & property tests

</td>
<td width="50%" valign="top">

### 🗄️ [plecost-db](https://github.com/Plecost/plecost-db)

CVE database builder and daily sync engine.

- Pulls from NVD API v2.0
- Jaro-Winkler fuzzy matching against 50k+ plugin slugs
- Incremental JSON patch system (daily GitHub Actions)
- SQLite and PostgreSQL support

</td>
</tr>
</table>

---

## Why Plecost?

| Feature | Plecost v4 | WPScan | Wordfence | ScanTower |
|---------|:---:|:---:|:---:|:---:|
| Python library API | ✅ | ❌ | ❌ | ❌ |
| Fully async (httpx) | ✅ | ❌ | ❌ | ❌ |
| No external API dependency | ✅ | ❌ | ❌ | ❌ |
| Stable finding IDs | ✅ | ❌ | ❌ | ❌ |
| Celery / automation compatible | ✅ | ❌ | ❌ | ❌ |
| PostgreSQL support | ✅ | ❌ | ❌ | ❌ |
| WAF detection (7 providers) | ✅ | ✅ | ❌ | ✅ |
| Daily CVE updates | ✅ | ✅ (API key) | ✅ | ✅ |
| Docker native | ✅ | ✅ | ❌ | ❌ |
| Content / skimmer analysis | ✅ | ❌ | ✅ | ❌ |

---

## License

Plecost is distributed under the **[PolyForm Noncommercial License 1.0.0](https://polyformproject.org/licenses/noncommercial/1.0.0/)**.

**Free for:** personal security research, internal corporate audits, academic use, open source projects, government and charitable organizations.

**Commercial use** (SaaS, paid products, revenue-generating services) requires a commercial license.
📧 Contact: [cr0hn@cr0hn.com](mailto:cr0hn@cr0hn.com)

---

<div align="center">

Made with ❤️ by **[Dani (cr0hn)](https://github.com/cr0hn)**

</div>
