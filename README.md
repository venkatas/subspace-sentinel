# Subspace Sentinel

`email_auth_audit.py` is a standalone Python CLI for auditing email authentication and mail security posture for:

- a domain
- an email address
- a raw `.eml` or header file
- a bulk list of targets

It performs deterministic checks first, then can optionally add AI-assisted commentary from Ollama or cloud APIs.

## Features

- SPF validation, nested include depth, lookup-limit risk, softfail and `+all` detection
- DMARC policy, alignment, reporting, and external report authorization checks
- DKIM selector probing with provider-aware hints
- MX hygiene and optional SMTP STARTTLS probing
- DNSSEC, MTA-STS, TLS-RPT, and BIMI checks
- Delivered-message analysis from `.eml` or raw headers
- ARC-aware message analysis for forwarded/relayed mail
- Freeform AI questions for uploaded message files
- Bulk portfolio review for many domains

## Install

```bash
git clone <your-repo-url>
cd subspace-sentinel
python3 -m pip install -r requirements.txt
cp .env.example .env
```

`dnspython` and `certifi` are recommended. If `dnspython` is not installed, the tool can still work via `dig` when available.

## AI Providers

Configure AI in `.env`.

Supported providers:

- `ollama`
- `claude`
- `openai`
- `xai`
- `gemini`
- `openai-compatible`
- `none`

Default local model in `.env.example`:

- `qwen3-coder-64k:latest`

## Usage

```bash
python3 email_auth_audit.py example.com --ai-provider none
python3 email_auth_audit.py user@example.com --json
python3 email_auth_audit.py example.com --smtp-probe
python3 email_auth_audit.py --message-file sample.eml
python3 email_auth_audit.py --message-file sample.eml --question "Why did this land in spam?"
python3 email_auth_audit.py --targets-file domains.txt --json
```

## Notes

- DNS findings are the source of truth.
- AI commentary is advisory only.
- DKIM discovery is heuristic unless you know the real selector names.
- For message-file analysis, a sent/archive copy may not contain enough receiver-side headers to judge SPF, DKIM, and DMARC outcomes.

