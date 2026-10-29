# Igneva Solutions — Security Advisories

Coordinated vulnerability disclosures by **[Igneva Solutions, S.L.](https://igneva.com)**

Responsible-disclosure contact: **security@igneva.com**

Igneva follows **coordinated disclosure**: affected vendors are notified privately (where a contact
exists) and given a reasonable window to remediate before technical details are made public. Full
technical details and proof-of-concept for each advisory below are **withheld** until the end of that
window (or an earlier vendor fix).

| ID | Product | Class | Status | CVE |
|----|---------|-------|--------|-----|
| IGNEVA-2026-001 | DigiCom (`com_digicom`) — Joomla extension (ThemeXpert) | Unauthenticated SQL Injection (CWE-89) | Coordinated disclosure in progress — **details embargoed** | *pending* |
| IGNEVA-2026-002 | Firedrive (`com_firedrive`) — Joomla extension (G. Mansillo) | Unauthenticated Broken Access Control / IDOR (CWE-639) | Coordinated disclosure in progress — **details embargoed** | *pending* |
| IGNEVA-2026-003 | Custom Tables (`com_customtables`) — Joomla extension (I. Komlev) | Unauthenticated SQL Injection via LLM prompt injection (CWE-89 / CWE-1427) | Coordinated disclosure in progress — **details embargoed** | *pending* |
| IGNEVA-2026-004 | tool_opencast — Moodle plugin (Opencast-Moodle) | Unauthenticated-reachable SSRF (CWE-918 / CWE-862) + Broken Access Control (CWE-862/863) | Coordinated disclosure in progress — **details embargoed** | *pending* |
| IGNEVA-2026-005 | Sunshine Photo Cart — WordPress plugin | Unauthenticated Broken Access Control (CWE-862 / CWE-639) | **Published** (vendor fix in 3.7.5) | [CVE-2026-109689](https://www.cve.org/CVERecord?id=CVE-2026-109689) |
| IGNEVA-2026-006 | Field Permissions — Drupal module | Broken Access Control / fail-open (CWE-284) | **Published** (fix in 8.x-1.6; [SA-CONTRIB-2026-289](https://www.drupal.org/sa-contrib-2026-289)) | [CVE-2026-118584](https://www.cve.org/CVERecord?id=CVE-2026-118584) |

---

### IGNEVA-2026-001 — DigiCom (com_digicom) for Joomla
- **Affected:** through the latest release.
- **Status:** reported to the vendor (ThemeXpert) and to the Joomla VEL; CVE request to MITRE (CNA-LR) pending.
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-002 — Firedrive (com_firedrive) for Joomla
- **Affected:** through the latest release.
- **Status:** vendor unreachable (site offline, no security contact); CVE request to MITRE (CNA-LR) pending.
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-003 — Custom Tables (com_customtables) for Joomla
- **Affected:** through the latest release (ChatGPT integration configured).
- **Status:** reported privately to the vendor via GitHub Security Advisory.
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-004 — tool_opencast (Moodle plugin)
- **Affected:** through the latest release (v5.2-r1).
- **Status:** reported to the vendor (security@opencast.org), who is coordinating the advisory and CVE; CVE pending.
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-005 — Sunshine Photo Cart (WordPress plugin)
- **Affected:** <= 3.7.4. **Patched:** 3.7.5.
- **Status:** **Published** — vendor fix released, CVE-2026-109689 public.
- **Full advisory:** [IGNEVA-2026-005-sunshine-photo-cart.md](IGNEVA-2026-005-sunshine-photo-cart.md)

### IGNEVA-2026-006 — Field Permissions (Drupal module)
- **Affected:** < 1.6.0. **Fixed:** 8.x-1.6.
- **Status:** **Published** — Drupal advisory SA-CONTRIB-2026-289; CVE-2026-118584 assigned. Credited as reporter and fixer.
- **Full advisory:** [IGNEVA-2026-006-field-permissions.md](IGNEVA-2026-006-field-permissions.md)

---

*Discovered by Rodrigo Cabo — Igneva ([igneva.com](https://igneva.com)).*
