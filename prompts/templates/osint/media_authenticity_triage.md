template_id: osint_media_authenticity_triage_v1
mode: builder
audience: {{audience|beginner}}
uses_module: security-osint-pack
goal: "First‑pass authenticity checks for images/video using metadata + cross‑checks"
---

## Summary
- Triage {{media_files}} for basic authenticity indicators; note limitations.

## Scope & Ethics
- Handle personal media lawfully and with consent. Metadata may contain PII (GPS, device IDs); minimize unnecessary retention. [cite]

## Assumptions
- OS: {{os|Windows/macOS/Linux}}; CLI available (ExifTool/FFprobe/MediaInfo) or GUI equivalents.

## Plan / Steps
1) **Preserve originals** — Work on copies; record hashes.
2) **Extract metadata**
   - Images: ExifTool (`-time:all -gps:all -a -G1 -s`).
   - Video: FFprobe (`-show_streams -show_format -print_format json`) and/or MediaInfo (JSON). — [cite][cite][cite]
3) **Normalize times** — Convert to UTC; check timezone consistency and device clock drift.
4) **Cross‑checks**
   - Compare encoding/bitrate/container vs expected for the claimed device.
   - If GPS present, corroborate with maps/CT/weather where relevant (outside scope here).
5) **Document** — Summarize findings with caveats; don’t over‑claim.

## Commands (examples)
**Windows (PowerShell)**
```powershell

# Image
.\exiftool.exe -time:all -gps:all -a -G1 -s .\image.jpg > meta_image.txt
# Video
.\ffprobe.exe -hide_banner -show_streams -show_format -print_format json .\clip.mp4 > meta_video.json
.\MediaInfo.exe --Output=JSON .\clip.mp4 > mediainfo.json
# Hash
Get-FileHash .\clip.mp4 -Algorithm SHA256
