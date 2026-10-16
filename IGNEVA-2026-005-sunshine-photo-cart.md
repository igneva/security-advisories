# IGNEVA-2026-005 — Sunshine Photo Cart (WordPress) — Unauthenticated disclosure of restricted gallery images

- **Advisory ID:** IGNEVA-2026-005
- **CVE:** [CVE-2026-109689](https://www.cve.org/CVERecord?id=CVE-2026-109689)
- **Status:** Public (vendor fix released; coordinated disclosure complete)
- **Product:** [Sunshine Photo Cart](https://wordpress.org/plugins/sunshine-photo-cart/) — WordPress plugin
- **Affected versions:** <= 3.7.4
- **Patched in:** 3.7.5
- **Vulnerability class:** Broken Access Control / Missing Authorization (CWE-862 / CWE-639), OWASP A1
- **Privileges required:** None (unauthenticated)
- **Severity:** CVE-2026-109689 is published at **CVSS 5.3** (Patchstack). Igneva's own assessment is **CVSS 7.5** (`AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`) for the `share` hook, which discloses the full-resolution original image; the other two affected hooks are 5.3 (`C:L`).
- **Credit:** Rodrigo Cabo (Igneva)
- **References:** [Patchstack database entry](https://patchstack.com/database/wordpress/plugin/sunshine-photo-cart/vulnerability/wordpress-sunshine-photo-cart-plugin-3-7-4-broken-access-control-vulnerability) · related: CVE-2026-16561 (the incomplete prior fix)

## Summary

Sunshine Photo Cart <= 3.7.4 exposes restricted gallery contents to unauthenticated visitors through three AJAX hooks that are missing an authorization check. This is a **distinct, still-exploitable attack surface and an incomplete fix of CVE-2026-16561**: that earlier issue (unauthenticated private-gallery comment disclosure) was fixed in 3.6.12 by adding a `can_access()` check to the `comments` hook, but three sibling hooks of the same dispatcher never received the same guard.

## Root cause

The AJAX dispatcher `wp_ajax_nopriv_sunshine_modal_display` (`includes/functions/modal.php`) validates only a **public nonce** (`sunshinephotocart`), which is emitted on every front-end gallery page and is therefore readable by any anonymous visitor. After the nonce check it dispatches to `sunshine_modal_display_<hook>`. Three of those hooks load a gallery/image from a request-supplied ID **without calling `can_access()`**:

| Hook | File | Discloses |
|---|---|---|
| `share` | `share.php` | the gallery permalink and the **full-size image URL** (`get_image_url('full')`), which is directly downloadable |
| `add_to_cart` | `add-to-cart.php` | the thumbnail, image name, and pricing/UI |
| `store_product` | `store.php` | product name, description, price, and options |

The sibling `comments` hook (`comments.php`) **does** call `can_access()` — the guard added by the CVE-2026-16561 fix. That guard was simply not extended to these three hooks.

## Impact

An unauthenticated attacker, using the public nonce, can read the contents of galleries the owner marked as **private** or **password-protected**. Through the `share` hook the response exposes the gallery permalink and the **full-resolution original image URL**, which can then be downloaded without any authentication — i.e. a photographer's private client images. This drives the confidentiality impact to High (Igneva assessment: CVSS 7.5 on `share`).

## Reproduction (against an affected version, in an isolated lab)

Reproduced end-to-end in an isolated local lab (WordPress 7.1.3, PHP 8.2, Sunshine Photo Cart 3.7.4) with fictitious data; nothing was tested against any third-party site.

1. As an anonymous visitor, open any public gallery page and read the public nonce from the response (`sunshine_photo_cart.security`).
2. Send unauthenticated POST requests (no cookies) to `admin-ajax.php`:

```
POST /wp-admin/admin-ajax.php
Content-Type: application/x-www-form-urlencoded

action=sunshine_modal_display&hook=share&security=<public_nonce>&imageId=<RESTRICTED_IMAGE_ID>
```

- `hook=share` with a restricted `imageId` returns the permalink and the full-resolution original URL; fetching that URL returns the original image (HTTP 200, image/jpeg) with no authentication.
- `hook=add_to_cart` with a restricted `imageId` returns the thumbnail, name, and pricing.
- `hook=store_product` with a restricted `galleryId`/`productId` returns the product name, description, and price.

**Controls:** the patched `comments` hook returns *Access denied* on the same restricted image; an invalid nonce returns *Failed security check* (confirming the nonce is the only gate); the normal front-end path to the private/password gallery denies access.

## Distinction from CVE-2026-16561

CVE-2026-16561 (fixed in 3.6.12) covered the `comments` hook of this dispatcher, which now enforces `can_access()`. This advisory concerns three **other** hooks (`share`, `add_to_cart`, `store_product`) of the same dispatcher that remained unguarded in 3.7.4 — a distinct, still-exploitable surface and an incomplete fix of the same class, not a re-report of the already-patched hook.

## Remediation

Update Sunshine Photo Cart to **3.7.5** or later, which adds the missing authorization check to the affected hooks.

## Timeline

- 2026-10-10 — Reported to the vendor via Patchstack (coordinated disclosure).
- 2026-10-12 — Validated by Patchstack.
- 2026-10-15 — CVE-2026-109689 published; fix available in 3.7.5.

## Disclosure

Coordinated disclosure, completed. Technical details are published here now that the vendor fix (3.7.5) is available and the CVE is public.

---

*Discovered by Rodrigo Cabo — Igneva ([igneva.com](https://igneva.com)). Security contact: security@igneva.com.*
