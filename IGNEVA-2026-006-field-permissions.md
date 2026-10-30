# IGNEVA-2026-006 — Field Permissions (Drupal) — Private field fail-open on ownerless entities

- **Advisory ID:** IGNEVA-2026-006
- **CVE:** [CVE-2026-118584](https://www.cve.org/CVERecord?id=CVE-2026-118584)
- **Vendor advisory:** [SA-CONTRIB-2026-289](https://www.drupal.org/sa-contrib-2026-289) (Drupal Security Team)
- **Status:** Public (fix released; coordinated disclosure complete)
- **Product:** [Field Permissions](https://www.drupal.org/project/field_permissions) — Drupal contributed module (`field_permissions`)
- **Affected versions:** < 1.6.0 (all releases before 8.x-1.6)
- **Fixed in:** 8.x-1.6 (for Drupal 9.5, 10 and 11)
- **Vulnerability class:** Broken Access Control / missing authorization — fail-open (CWE-284), OWASP A1
- **Privileges required:** None (unauthenticated) to read a restricted field on an otherwise viewable ownerless entity
- **Severity:** Drupal Security Team: **Moderately critical, 13/25** (AC:None/A:None/CI:Some/II:None/E:Theoretical/TD:Uncommon). Igneva CVSS v3.1 estimate: **5.3** (`AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N`).
- **Credit:** Rodrigo Cabo (Igneva) — [@rodrigo-cabo](https://www.drupal.org/u/rodrigo-cabo). Credited by the Drupal Security Team as both **Reporter** and **Fixer** (fix reviewed and verified in an isolated lab).
- **References:** [SA-CONTRIB-2026-289](https://www.drupal.org/sa-contrib-2026-289) · [CVE-2026-118584](https://www.cve.org/CVERecord?id=CVE-2026-118584) · [fixed release 8.x-1.6](https://www.drupal.org/project/field_permissions/releases/8.x-1.6)

## Summary

The Field Permissions module lets a site builder mark an individual field as **"Private"**, so its value is visible only to the entity's author and to users holding the *access private fields* permission. On entities that have **no concept of ownership** (for example taxonomy terms, custom blocks or menu links), that restriction silently failed open: the Private field was readable by **any user who could view the entity, including anonymous visitors**.

## Root cause

Access for a Private field is decided by `PrivateAccess::hasFieldAccess()` (`src/Plugin/FieldPermissionType/PrivateAccess.php`). The method grants access to users with the *access private fields* permission, and for owner-aware entities it compares the entity owner to the current user. Its **final branch**, reached for entities that implement neither `UserInterface` nor `EntityOwnerInterface`, returned `TRUE` — fail-open.

As a result, a field marked "Private" on an ownerless entity (a public taxonomy-term page, for example) was disclosed to everyone, rather than being restricted. The sibling `CustomAccess` plugin already failed closed in the same situation; `PrivateAccess` had never been given the equivalent guard (the fail-open branch dated back to the Drupal 8 port in 2016 and was never revisited).

## Impact

An unauthenticated visitor able to view an ownerless entity could read the contents of a field the site owner had marked **Private** on that entity. Confidentiality impact depends on what the site stored in such fields.

## Fix

Released in **8.x-1.6**: the final branch of `PrivateAccess::hasFieldAccess()` now returns `FALSE` (fail-closed), matching `CustomAccess`. Only users with the *access private fields* permission (and, on owner-aware entities, the owner) can see a Private field.

**Behaviour change to note when upgrading:** a site that previously relied on the old default to show these fields to everyone on ownerless entities will now hide them unless the *access private fields* permission is granted. This is the intended hardening and is documented in the Drupal advisory.

## Verification (isolated lab, fictitious data)

Reproduced and the fix verified end to end in an isolated local lab (Drupal 11.4.10, PHP 8.5.11, Field Permissions 8.x-1.5), with fictitious data; nothing was tested against any third-party site.

- **Before the fix:** as an anonymous visitor, a "Private" field on a taxonomy term was disclosed through both the rendered term page and the JSON:API representation.
- **After the fix:** both paths deny the field; the public term page itself remains accessible (only the Private field disappears).
- **No regression:** a user holding *access private fields* still sees the field, and on owner-aware entities the owner still sees their own field.

## Remediation

Update Field Permissions to **8.x-1.6** or later.

## Timeline

- 2026-10-16 — Reported privately to the Drupal Security Team (coordinated disclosure).
- 2026-10-20 — Report validated; fix (MR) prepared by the maintainers.
- 2026-10-21 — Fix reviewed and verified by the reporter in an isolated lab.
- 2026-10-28 — Fix released (8.x-1.6) and advisory published (SA-CONTRIB-2026-289); CVE-2026-118584 assigned.

## Disclosure

Coordinated disclosure, completed. Drupal acts as the CVE Numbering Authority for its contributed projects.

---

*Discovered by Rodrigo Cabo — Igneva ([igneva.com](https://igneva.com)). Security contact: security@igneva.com.*
