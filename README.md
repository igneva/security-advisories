# Igneva Solutions — Security Advisories

Coordinated vulnerability disclosures by **[Igneva Solutions, S.L.](https://igneva.com)**

Responsible-disclosure contact: **security@igneva.com**

Igneva follows **coordinated disclosure**: affected vendors are notified privately (where a contact
exists) and given a reasonable window to remediate before technical details are made public. Full
technical details and proof-of-concept for each advisory below are **withheld** until the end of that
window (or an earlier vendor fix).

| ID | Product | Class | Status | CVE |
|----|---------|-------|--------|-----|
| IGNEVA-2026-001 | DigiCom (`com_digicom`) — Joomla extension (ThemeXpert) | Unauthenticated SQL Injection (CWE-89) | Coordinated disclosure in progress — **details embargoed** | *requested (pending)* |
| IGNEVA-2026-002 | Firedrive (`com_firedrive`) — Joomla extension (G. Mansillo) | Unauthenticated Broken Access Control / IDOR (CWE-639) | Coordinated disclosure in progress — **details embargoed** | *requested (pending)* |
| IGNEVA-2026-003 | Custom Tables (`com_customtables`) — Joomla extension (I. Komlev) | Unauthenticated SQL Injection via LLM prompt injection (CWE-89 / CWE-1427) | Coordinated disclosure in progress — **details embargoed** | *requested (pending)* |
| IGNEVA-2026-004 | tool_opencast — Moodle plugin (Opencast-Moodle) | Unauthenticated-reachable SSRF (CWE-918 / CWE-862) + Broken Access Control (CWE-862/863) | Coordinated disclosure in progress — **details embargoed** | *requested (pending)* |

---

### IGNEVA-2026-001 — DigiCom (com_digicom) for Joomla
- **Affected:** through the latest release.
- **Status:** reported privately to the vendor; CVE requested from MITRE (CNA-LR).
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-002 — Firedrive (com_firedrive) for Joomla
- **Affected:** through the latest release.
- **Status:** vendor unreachable (site offline, no security contact); CVE requested from MITRE (CNA-LR).
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-003 — Custom Tables (com_customtables) for Joomla
- **Affected:** through the latest release (ChatGPT integration configured).
- **Status:** reported privately to the vendor via GitHub Security Advisory.
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

### IGNEVA-2026-004 — tool_opencast (Moodle plugin)
- **Affected:** through the latest release (v5.2-r1).
- **Status:** reported privately to the vendor (security@opencast.org); CVE requested from MITRE (CNA-LR).
- **Full advisory:** to be published here on or after the coordinated-disclosure date.

---

*Discovered by Rodrigo Cabo — Igneva Solutions, S.L.*
