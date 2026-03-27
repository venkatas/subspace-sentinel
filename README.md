```
╔══════════════════════════════════════════════════════════════════════════════╗
║  ═══  LCARS INTERFACE  ══════════════════════════════════  STARDATE 2026.3  ║
║                                                                              ║
║   ██████  ██  ██ ██████   ██████  ██████  ████   ██████  ████████           ║
║  ██      ██  ██ ██   ██ ██      ██   ██ ██  ██ ██      ██                  ║
║   █████  ██  ██ ██████   ██████ ██████  ██████ ██      ████                ║
║       ██ ██  ██ ██   ██      ██ ██      ██  ██ ██      ██                  ║
║  ██████   ████  ██████  ██████  ██      ██  ██  ██████ ████████            ║
║                                                                              ║
║  ███████  ███████ ███   ██ ████████ ██ ███   ██ ███████ ██                 ║
║  ██      ██      ████  ██    ██    ██ ████  ██ ██      ██                  ║
║  ███████ █████   ██ ██ ██    ██    ██ ██ ██ ██ █████   ██                  ║
║      ██  ██      ██  ████    ██    ██ ██  ████ ██      ██                  ║
║  ███████ ███████ ██   ███    ██    ██ ██   ███ ███████ ███████              ║
║                                                                              ║
║  ░░░  U.S.S. SUBSPACE SENTINEL  ·  NCC-8472  ·  MAIL SECURITY DIVISION  ░░║
╚══════════════════════════════════════════════════════════════════════════════╝
```

> *"Hailing frequencies open, Captain. We're detecting anomalous SPF emissions from Sector 47-Bravo."*
> — Ensign Authmailova, USS Subspace Sentinel

---

## 📡 MISSION BRIEFING

**Starfleet Intelligence has intercepted suspicious transmissions.** Someone out there is impersonating your mail domain — forging headers, bypassing authentication protocols, and delivering counterfeit communiqués to unsuspecting colonists across the quadrant.

**Subspace Sentinel** is your shipboard LCARS security module for auditing email authentication infrastructure. It sweeps every layer of your mail defence grid — from SPF deflector shields to DMARC command directives — then files a complete tactical report, with optional AI-assisted analysis from your choice of holographic advisor.

It also tries to infer the **active mail provider** and the **DNS host / registrar control plane** from MX, SPF, NS, and SOA hints so the remediation output can tell you not just what MX records to publish, but where you are most likely to add them.

*No phaser required. Python 3 is sufficient.*

---

## 🛡️ DEFENSIVE SYSTEMS CHECKED

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  SYSTEM          STARFLEET ANALOGY              WHAT WE CHECK               │
├─────────────────────────────────────────────────────────────────────────────┤
│  SPF           · Deflector shield array         Record validity, +all risk, │
│                                                 lookup depth, ptr usage,    │
│                                                 /0 network catastrophes     │
│  DMARC         · Command authority protocol     Policy level, pct coverage, │
│                                                 alignment, rua reporting,   │
│                                                 subdomain policy gaps       │
│  DKIM          · Cryptographic handshake key    Selector probing, key size, │
│                                                 provider-aware heuristics,  │
│                                                 RSA/Ed25519 hardening       │
│  MX            · Transporter pad registry       MX hygiene, null MX,       │
│                                                 A/AAAA fallback risk,      │
│                                                 provider-aware examples    │
│  DNS Host Hint · Starbase operations console    Likely DNS control panel   │
│                                                 for MX/TXT/CNAME changes   │
│  SMTP STARTTLS · Secure hailing channel         Live TLS negotiation,       │
│                                                 cipher and version probe    │
│  DNSSEC        · Navigation beacon integrity    DS + DNSKEY presence        │
│  MTA-STS       · Forced-warp lane policy        Policy body fetch, mode,    │
│                                                 mx patterns, max_age        │
│  TLS-RPT       · Battle damage report channel   rua configuration           │
│  BIMI          · Federation insignia registry   Logo URI, VMC assertion,    │
│                                                 DMARC prerequisite check    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚨 ALERT CONDITION REFERENCE

| Alert Condition | Severity | Typical Threat                                                |
|:---------------:|:--------:|---------------------------------------------------------------|
| 🔴 RED ALERT    | CRITICAL | SPF ends in `+all` — every Romulan warbird is authorized      |
| 🟠 RED ALERT    | HIGH     | No DMARC — the Borg can impersonate you at will               |
| 🟡 YELLOW ALERT | MEDIUM   | `p=none` — you see the attack but cannot stop it              |
| 🔵 BLUE ALERT   | LOW      | `~all` softfail — *technically* a shield, barely              |
| ⚪ SHIELDS NOMINAL | INFO  | Good hygiene; log it and set course for the next anomaly      |

---

## ⚙️ WARP CORE INSTALLATION

```bash
# Beam aboard the source code
git clone https://github.com/venkatas/subspace-sentinel.git
cd subspace-sentinel

# Power up the dilithium crystals
python3 -m pip install -r requirements.txt

# Configure your holographic advisor
cp .env.example .env
$EDITOR .env
```

**Recommended:** `dnspython` (installed via requirements.txt) for full warp capability.
**Fallback:** If `dnspython` is unavailable, the ship degrades gracefully to `dig` impulse drive.

---

## 🤖 HOLOGRAPHIC ADVISORS

Configure your AI analyst in `.env`. All advisors are **advisory only** — the DNS record is always the Prime Directive.

| Provider           | Identity                                          |
|--------------------|---------------------------------------------------|
| `ollama`           | Local holodeck (default: `qwen3-coder-64k:latest`)|
| `claude`           | Vulcan Science Officer (Anthropic)                |
| `openai`           | Starfleet Command AI (OpenAI)                     |
| `xai`              | Romulan defector model (xAI/Grok)                 |
| `gemini`           | Betazoid empath (Google Gemini)                   |
| `openai-compatible`| Any warp-capable relay endpoint                   |
| `none`             | Silent running — tactical scans only              |

---

## 📟 TACTICAL COMMANDS

### Single-target deep scan

```bash
# Scan a domain
python3 email_auth_audit.py example.com

# Scan from an email address (raises severity thresholds — real mail is at stake)
python3 email_auth_audit.py captain@enterprise.starfleet

# Full STARTTLS probe — attempt live contact with all MX hosts
python3 email_auth_audit.py example.com --smtp-probe

# JSON output for LCARS data relay
python3 email_auth_audit.py example.com --json

# Write report to captain's log
python3 email_auth_audit.py example.com --output report.txt
```

### Intercepted-message analysis

```bash
# Analyse a captured communiqué (.eml / raw headers)
python3 email_auth_audit.py --message-file suspicious.eml

# Ask your holographic advisor a specific question about it
python3 email_auth_audit.py --message-file suspicious.eml \
  --question "Why did this bypass our DMARC shields?"
```

### Fleet-wide sector sweep (bulk mode)

```bash
# Sweep a list of targets — now parallelised across 10 warp cores
python3 email_auth_audit.py --targets-file domains.txt --json

# Inline CSV sweep
python3 email_auth_audit.py --targets "enterprise.com,voyager.com,ds9.com"
```

### Custom DKIM selectors

```bash
# Provide known selectors to skip heuristic probing
python3 email_auth_audit.py example.com --selectors "google,selector1,dkim2024"

# Load from file
python3 email_auth_audit.py example.com --selector-file selectors.txt
```

### AI provider override

```bash
# Override provider at launch without editing .env
python3 email_auth_audit.py example.com \
  --ai-provider claude \
  --ai-model claude-opus-4-5 \
  --env-file /path/to/.env
```

### Provider and registrar-aware guidance

```bash
# See likely mail provider and DNS host / registrar hint
python3 email_auth_audit.py example.com --ai-provider none
```

When MX is missing and the tool can infer a provider, the remediation output now includes:

- provider-specific MX examples
- a DNS host hint such as `GoDaddy`
- operator steps showing where to add the records

---

## 📊 SAMPLE TACTICAL REPORT

```
Email Auth Audit
Target: enterprise.starfleet
Domain: enterprise.starfleet
Input type: domain
DNS backend: dnspython
Mail provider guess: Google Workspace
DNS host hint: GoDaddy
Overall risk: HIGH

Checks
  SPF        WARN  SPF record found; estimated DNS lookups: 3
  DMARC      WARN  DMARC policy: none
  DKIM       PASS  Found 1 DKIM selector(s) from 17 probes
  MX         PASS  Found 2 MX record(s)
  DNSSEC     WARN  DNSSEC not detected
  MTA-STS    INFO  No MTA-STS record found
  TLS-RPT    INFO  No TLS-RPT record found
  BIMI       INFO  No BIMI record found

Findings
  [DMARC] HIGH - DMARC is monitor-only (p=none)
    The domain collects reports but does not tell receivers to quarantine
    or reject spoofed mail.
    Fix: Move to quarantine and then reject once legitimate mail is aligned.

  [SPF] LOW - SPF uses softfail (~all)
    Softfail is better than no policy, but weaker than -all.
    Fix: Move toward -all once all legitimate senders are confirmed.

Remediation Plan
  [HIGH] Tighten DMARC enforcement
    Why: p=none gives Romulans full impersonation capability.
    Step: Add rua= for aggregate reports and monitor for 2-4 weeks.
    Step: Move to p=quarantine, then p=reject.
  [INFO] Provider-specific validation checklist
    Why: The domain appears to use Google Workspace; validate DNS settings against that provider's mail documentation.
    Step: DNS appears to be hosted at GoDaddy; use that control panel to add or update MX, TXT, and CNAME records.
    Step: Sign in to GoDaddy, open My Products, and choose DNS for the domain.
    Step: In the DNS records page, add or edit MX records at the root of the domain (@).
```

---

## 🔬 TECHNICAL SPECIFICATIONS

```
Hull designation  : Subspace Sentinel NCC-8472
Propulsion        : Python 3.8+  (no transwarp needed)
Primary sensors   : dnspython ≥ 2.6.1
Secondary sensors : dig (fallback, impulse only)
Shields           : certifi ≥ 2024.7.4 (SSL certificate authority bundle)
Holodeck suites   : 6 AI providers via unified dispatcher
Warp cores (bulk) : concurrent.futures.ThreadPoolExecutor — 10 parallel threads
Standard library  : argparse · email · json · socket · ssl · subprocess · re
                    ipaddress · urllib · base64 · dataclasses
```

---

## 📐 FLIGHT RULES

1. **DNS is the Prime Directive.** The live record is always ground truth. AI commentary is advisory.
2. **DKIM discovery is heuristic.** Subspace Sentinel probes 17 common selectors plus provider-aware hints. If the real selector is unusual, pass it via `--selectors`.
3. **Message-file analysis is header-bound.** A sent/archive copy may lack receiver-side `Authentication-Results` headers. Supply a fully delivered copy for complete forensics.
4. **Bulk mode is now concurrent.** Each domain gets its own DNS resolver thread. On large lists, DNS round-trip latency — not CPU — is the limiting factor.
5. **`--smtp-probe` makes live TCP connections.** Do not run against domains you are not authorised to test.

---

## 🌌 KNOWN ANOMALIES

- DKIM selector enumeration is probabilistic. Unknown custom selectors will not be found without explicit `--selectors`.
- DNSSEC validation checks for DS/DNSKEY presence only; full chain validation requires a validating resolver upstream.
- BIMI VMC assertion presence is noted but certificate chain validation is out of scope.
- ARC chain trust is evaluated structurally from headers; cryptographic ARC-Seal verification requires the signing keys.

---

## 📜 CAPTAIN'S LOG — CHANGELOG

**Stardate 2026.3 — Initial Commission**
- Full email authentication audit suite commissioned
- Parallel bulk scanning via `ThreadPoolExecutor`
- Six AI advisor integrations unified under single dispatcher
- ARC-aware delivered-message analysis
- Remediation plan generation with provider-aware DNS examples
- Post-review bug fixes committed before first voyage:
  - Variable shadowing neutralised in ARC header parser
  - Indentation anomaly resolved in AI HTTP error handler
  - Missing error handling added for unreadable targets file
  - Dead code jettisoned (`severity_badge`, `status_badge`)
  - Severity label consistency enforced (`"info"` throughout)
  - Explicit `mode` key added to all report types

---

## ⚖️ ARTICLES OF THE FEDERATION

MIT License — see [`LICENSE`](LICENSE).

*"The Prime Directive is not just a set of rules; it is a philosophy.*
*And for email authentication, that philosophy is: publish `-all`, enforce `reject`,*
*and rotate your DKIM keys before the Borg adapt."*

— Captain Jean-Luc Picard (probably)

---

```
═══════════════════════════════════════════════════════════
  LCARS TERMINAL OFFLINE  ·  END OF REPORT  ·  NCC-8472
  "TO BOLDLY AUDIT WHAT NO POSTMASTER HAS AUDITED BEFORE"
═══════════════════════════════════════════════════════════
```
