---
template_id: osint_evidence_capture_v1
mode: builder
audience: {{audience|beginner}}
uses_module: security-osint-pack
goal: "High‑fidelity capture of web pages and preservation with hashes"
---

## Summary
- What to capture ({{urls}}) and why; output format (WARC/WACZ), hash algorithm, and storage location.

## Scope & Ethics
- Capture only publicly accessible content you’re allowed to save; respect ToS and robots directives. Avoid personal data unless consented/required. [cite]

## Assumptions
- OS: {{os|Windows/macOS/Linux}}; Browser available.

## Plan / Steps
1) **Plan** — Create a case directory and a simple capture log (time, URL, why).
2) **Capture** — Use ArchiveWeb.page (extension/app) to capture target pages; export **WACZ/WARC**. — [cite]
3) **Verify playback** — Open the export in ReplayWeb.page; record any rendering issues. — [cite]
4) **Hash & log** — Compute SHA‑256 for each artifact and record in the log.
5) **Backup** — Store artifacts + log in an immutable or versioned location.

## Commands (pick your OS)
**Windows (PowerShell)**
```powershell
Get-FileHash .\captures\example.wacz -Algorithm SHA256 | Tee-Object -FilePath .\captures\hashes.txt -Append
