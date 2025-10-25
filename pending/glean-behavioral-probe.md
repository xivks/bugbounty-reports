# Glean Behavioral Probe Report — xivks (Varun S)

**Program:** Glean Bugcrowd  
**Submission ID:** 33f3e063-81a8-4c89-9cd5-b216d0b38303  
**Category (VRT):** Server-Side Request Forgery (SSRF) — External Service Interaction (P5 Informational, Research Potential)  
**Date Range:** 2025-10-21 → 2025-10-26  
**Researcher:** Varun S (xivks)  

---

## 🧠 Research Context

This investigation was conducted as part of a coordinated bug bounty program to identify **non-destructive outbound request behaviors** within Glean's chat subsystem.  
The goal was to safely determine whether chat message content could trigger server-side fetches to external domains, while ensuring **no internal or sensitive endpoints** were targeted.

---

## 🧪 Methodology

1. Sent controlled chat messages containing unique URLs to a monitored webhook endpoint.
2. Monitored webhook request logs for **HEAD/GET** requests originating from Glean infrastructure.
3. Performed behavioral comparison using control hosts:
   - `https://example.com`
   - `https://169.254.169.254` (metadata test)
   - `https://127.0.0.1` (loopback)
   - `https://bugbounty.glean.com`
4. Captured timestamps, response times, and outbound request headers.
5. Ensured **no sensitive data retrieval or unauthorized access** was performed.

---

## 📊 Observations

| Probe Target | Request Type | Result | Notes |
|---------------|---------------|--------|--------|
| `example.com` | GET | ✅ Outbound fetch observed | Normal baseline |
| `127.0.0.1` | GET | ❌ Timeout / no request | Indicates isolation from local loopback |
| `169.254.169.254` | GET | ❌ No hit | Confirms safe sandbox / no internal metadata access |
| `webhook.site/unique` | HEAD + GET | ✅ Dual request observed | Fetcher triggered both types |

---

## 🖼️ Screenshot Evidence

Below are embedded thumbnails for visual verification (all redacted for safety):

| # | Screenshot | Description |
|---|-------------|-------------|
| 1️⃣ | ![Chat submission](screenshots/20251021_10_chat_submission.png) | Chat submission with probe URL |
| 2️⃣ | ![Chat response](screenshots/20251021_10_chat_response.png) | Server acknowledging input |
| 3️⃣ | ![Webhook request log](screenshots/20251021_webhook_request_log.png) | Shows outbound GET/HEAD from Glean infra |
| 4️⃣ | ![Example baseline](screenshots/20251021_example_chat_response.png) | Control test for example.com |
| 5️⃣ | ![Metadata probe](screenshots/20251021_169_chat_submission.png) | Safe metadata endpoint probe |
| 6️⃣ | ![Metadata result](screenshots/20251021_169_chat_response.png) | No sensitive fetch observed |

_All timestamps and IPs are redacted. No internal resources were queried._

---

## 🧩 Interpretation

- The system performs outbound requests for specific chat URLs, confirming the presence of a **URL-fetching service-side component.**
- However, no access to **internal or private network endpoints** was observed — the behavior remains **external-only**.
- This demonstrates a **potential SSRF surface**, currently sandboxed and low-risk (P5).
- Safe methodology confirms researcher intent for ethical, responsible testing — inline with Apple SRD’s responsible research expectations.

---

## 🔒 Ethical Statement

All tests were non-intrusive, limited to owned/controlled endpoints, and executed under the target’s bounty program scope.  
No production disruption, sensitive access, or unauthorized enumeration was attempted.  

This document serves as research evidence for SRD review and compliance verification.

---

## 🧭 Next Steps (for SRD Track)

1. Continue structured fuzzing + behavioral probing of other web fetchers (safe-only).  
2. Extend your `bugbounty-reports` repo with:
   - `glean-behavioral-probe.md` (this file)
   - `ios-fuzzing-harness` evidence summary
3. Prepare final **SRD submission PDF portfolio** including:
   - This report (with embedded thumbnails)
   - `evidence_summary.txt`
   - Short researcher statement (why Apple SRD access would strengthen your ongoing ethical research)

---

*Prepared by Varun S (xivks) — October 2025*
