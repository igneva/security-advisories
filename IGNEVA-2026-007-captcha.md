# IGNEVA-2026-007 — CAPTCHA (Drupal) — Unauthenticated CAPTCHA bypass via session replay

- **Advisory ID:** IGNEVA-2026-007
- **CVE:** [CVE-2026-118582](https://www.cve.org/CVERecord?id=CVE-2026-118582)
- **Vendor advisory:** [SA-CONTRIB-2026-281](https://www.drupal.org/sa-contrib-2026-281) (Drupal Security Team)
- **Status:** Public (fix released; coordinated disclosure complete)
- **Product:** [CAPTCHA](https://www.drupal.org/project/captcha) — Drupal contributed module (`captcha`)
- **Affected versions:** < 1.18.0 and >= 2.0.0 < 2.0.11
- **Fixed in:** 2.0.11 (2.0.x) and 8.x-1.18 (8.x-1.x)
- **Vulnerability class:** Improper authentication / CAPTCHA bypass via session replay (CWE-287)
- **Privileges required:** None (unauthenticated)
- **Severity:** Drupal Security Team: **Moderately critical, 13/25**.
- **Credit:** Reported by **Rodrigo Cabo (Igneva)** — [@rodrigo-cabo](https://www.drupal.org/u/rodrigo-cabo) — together with James Gilliland (neclimdul) of the Drupal Security Team.
- **References:** [SA-CONTRIB-2026-281](https://www.drupal.org/sa-contrib-2026-281) · [CVE-2026-118582](https://www.cve.org/CVERecord?id=CVE-2026-118582) · related: CVE-2026-3214 (the prior, incompletely fixed issue)

## Summary

The CAPTCHA module could be bypassed by an unauthenticated attacker through **session replay**: a CAPTCHA challenge solved once could be reused to pass later submissions, so the challenge no longer guaranteed a fresh human interaction per request. Igneva reported this as an **incomplete fix of CVE-2026-3214**; the Drupal Security Team coordinated it together with a related cacheability issue in the same module and published a single advisory and fix.

## Impact

Automated clients (bots, spam/abuse tooling) could defeat the protection CAPTCHA is meant to provide — submitting forms, comments or other protected actions without solving a fresh challenge each time.

## Fix

Released in **2.0.11** and **8.x-1.18**, which harden the token/session handling so a solved challenge can no longer be replayed to satisfy a subsequent submission.

## Verification (isolated lab, fictitious data)

Reproduced in an isolated local lab with fictitious data; nothing was tested against any third-party site. An omitted/replayed challenge token allowed a protected submission to go through on an affected version; the patched releases reject the replay.

## Remediation

Update CAPTCHA to **2.0.11** (2.0.x) or **8.x-1.18** (8.x-1.x) or later.

## Timeline

- 2026-10-16 — Reported privately to the Drupal Security Team (coordinated disclosure).
- 2026-10-28 — Fix released (2.0.11 / 8.x-1.18) and advisory published (SA-CONTRIB-2026-281); CVE-2026-118582 assigned.

## Disclosure

Coordinated disclosure, completed. Drupal acts as the CVE Numbering Authority for its contributed projects.

---

*Discovered by Rodrigo Cabo — Igneva ([igneva.com](https://igneva.com)). Security contact: security@igneva.com.*
