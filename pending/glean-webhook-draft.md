# Glean SSRF Behavioral Probe (Bugcrowd ID: 33f3e063-81a8-4c89-9cd5-b216d0b38303)

**Program:** Glean (Bugcrowd)  
**Researcher:** Varun S (xivks)  
**Report ID:** 33f3e063-81a8-4c89-9cd5-b216d0b38303  
**Status:** Triaged — Vendor Requested Additional Proof (Behavioral Evidence in Progress)  
**Date Reported:** October 2025  
**Platform:** https://app.glean.com  

---

## 🧠 Summary
User-supplied URLs in Glean Chat appear to trigger **server-side fetches** (HEAD/GET) to external resources.  
This indicates potential SSRF behavior where the backend retrieves URLs provided by chat users.

The vendor confirmed outbound fetches but classified the initial report as **P5 (informational)** because internal reachability wasn’t demonstrated.  
This updated draft documents our non-destructive behavioral testing plan and contains the available triage correspondence.

---

## ⚙️ Testing Setup

**Environment:**  
- Account: xivks (Bugcrowd test account)  
- Browser: Firefox Developer Edition  
- OS: Windows 10/11  
- Tooling: PowerShell + webhook.site unique URLs

**Test commands & notes:**
```powershell
# example: capture a precise timestamp (UTC)
Get-Date -Format o

# save a page for offline review
curl.exe -s "https://bug-bounty-be.glean.com/search?q=xivks_probe" -o resp.html
start resp.html


## Attachments (collected)
- evidence/evidence_summary.txt (probe-by-probe CSV)
- evidence/webhook_headers.txt (redacted)
- evidence/20251021_webhook_chat_submission.png
- evidence/20251021_webhook_chat_response.png
- evidence/20251021_webhook_request_log.png

**Status:** Behavioral probes completed; evidence summary and redacted headers added. Awaiting triage re-evaluation.

