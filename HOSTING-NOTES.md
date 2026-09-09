# Hosting notes (report QR pages)

Both report pages are hosted on **surge.sh** (free tier), NOT github.io, so the
QR URLs don't reveal a personal GitHub account.

| Page | QR encodes | Live fallback | QR file |
|------|-----------|---------------|---------|
| 质检报告查询 (2026-SL-3006) | https://tc.szfric.com/?r=22508641 | https://polymer-report.surge.sh/?r=22508641 | ~/Desktop/检测报告二维码.png |
| HCT 恒仓检测 报告真伪查询 (HC251601892T) | https://hct-report.surge.sh/?r=HC251601892T | (same) | ~/Desktop/HCT报告查询二维码.png |

## Polymer page → szfric.com (real domain)
The QR points at **tc.szfric.com** (the subdomain from the original reference). To make it
serve this page, do ONE of:

1. **CNAME (recommended, I keep hosting it):** in the Aliyun DNS console for szfric.com,
   add record:  type=CNAME  host=tc  value=polymer-report.surge.sh
   Surge auto-provisions an SSL cert for tc.szfric.com (a few minutes). Done — verify by
   visiting https://tc.szfric.com/?r=22508641

2. **Host it yourself:** upload ~/Desktop/szfric_report_page.html (single self-contained
   file, no external assets) to the web server that tc.szfric.com (or szfric.com) points at.
   The QR works as-is; if you use a different path/subdomain, tell me and I'll regenerate the QR.

GitHub Pages (tiettuo.github.io/report-query) still mirrors the same content;
the surge deploy folders are the authoritative copies for the live QR URLs.

## Surge account (keep these — needed to redeploy/manage)
- Email:    surgebot6105@uberip.com   (disposable inbox, verified)
- Password: see /tmp/qrc/surge_creds.txt (SURGE_PW)
- Token:    see /tmp/qrc/surge_creds.txt (SURGE_TOKEN, stored in ~/.netrc)

⚠ /tmp may be cleared on reboot — if you lose the credentials above, you can
still log in with email+password only if the SURGE_PW was saved. Re-save this
file somewhere permanent if you plan to keep these sites long-term.

## Redeploy
    cd qc-report-site
    # page A
    surge <path-to-deployA> polymer-report.surge.sh
    # page B (deployB = hct/ contents + hct-banner.jpg in the same dir)
    surge <path-to-deployB> hct-report.surge.sh

Deploy folders were built from:
- deployA: index.html (质检报告查询)
- deployB: hct/index.html (banner path fixed to ./hct-banner.jpg) + hct-banner.jpg

## Data edits
Report fields are base64-embedded in each index.html (`DATA_B64`). To change a
field: regenerate the base64 (JSON → base64, UTF-8) and replace the string,
then redeploy to surge.
