---
template_id: osint_passive_footprint_v1
mode: builder
audience: {{audience|beginner}}
uses_module: security-osint-pack
goal: "Lawful passive footprint of a domain/IP/organization"
---

## Summary
- What we will map ({{target}}), why it matters, expected outputs (e.g., subdomains, ASN, historical certs).

## Scope & Ethics (MUST READ)
- Lawful, consented OSINT only. No bypassing authentication/captchas/technical controls. Respect ToS and rate limits. (See pack guardrails.)
- Passive collection only (RDAP/CT logs/search engines/archival data). No active scanning without explicit written permission. [cite]

## Assumptions
- Jurisdiction: {{jurisdiction|unspecified}}. Tools: browser + optional CLI.
- OS: {{os|Windows/macOS/Linux}}.

## Plan / Steps
1) **Search refinement** — Form queries with operators: exact phrases, `site:`, `filetype:` to identify public docs & endpoints. — [cite]
2) **Registration data** — RDAP lookup for {{domain_or_ip}} to see registrar, contacts (if available), nameservers, and ASNs. Capture to case file. — [cite]
3) **Certificates & CT logs** — Search CT logs for historical subdomains (e.g., crt.sh). Export results. — [cite]
4) **Passive enumeration** — If needed, run Amass in **passive mode** only to correlate sources. Save JSON/CSV. — [cite]
5) **Optional infra context** — Use urlscan.io (search, not submit) for historical snapshots of target-related URLs. — [cite]
6) **Synthesis** — Normalize domains/subdomains, note first/last-seen dates, and highlight uncertainties.

## Commands (choose one per OS)
**Windows (PowerShell)**
```powershell
# Hash any exported evidence
Get-FileHash .\findings.csv -Algorithm SHA256

# Example: Fetch RDAP (replace with correct RDAP endpoint)
irm "https://rdap.apnic.net/ip/1.1.1.1" | ConvertFrom-Json | Select-Object handle,name
