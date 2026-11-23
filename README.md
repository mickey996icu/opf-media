# OpenPackagedFood Media (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 Product Database (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 Tooling & Spec (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

This repository is the dedicated, content-addressed **cropped label WebP evidence store** for **OpenPackagedFood (`opf` v0.2)**. Keeping image binaries completely separate from [`opf-data`](https://github.com/mickey996icu/opf-data) protects the JSON data repository's Git history and GitHub Pages bundle quota.

---

## Layout & Strict Admission Rules

- **Content-Addressed Path Layout**: `media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp` (referenced in `opf-data` as `opf-media://sha256/<sha256>`).
- **Functional Label Regions Only**: Only accept tightly cropped factual text regions (**nutrition facts table, ingredients list, allergen statement, manufacturer/SC plant block, barcode/net content**). **Never submit full front-of-pack cover artwork**—keeping OCR legible and minimizing packaging artwork copyright surface.
- **Technical & Privacy Gates (Enforced by `opf validate-media` CI)**:
  - **WebP only**, maximum image edge **`≤ 1000px`**, maximum file size **`≤ 512 KB`**;
  - Orientation baked into pixels (`exif_transpose`) with **zero `EXIF` or `XMP` metadata chunks**, and filename SHA-256 must match exact bytes;
  - No faces, fingers, delivery labels, shopping receipts, prices, or day-precision lot/expiry stamps.

---

## ⚠️ Dual-PR Merge Order (`opf-media` FIRST, `opf-data` SECOND)

Phase 1 accepts developer contributions via the three-repo pure-Git workflow (everyday end-user submission opens in Phase 2 via Agent `OPF_SUBMIT_URL` relay; see [`CONTRIBUTING.md`](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.md)).

When a contribution creates both media and data PRs:
1. Generate EXIF-free `<= 1000px` WebP crops and product JSON locally (`./open-packaged-food/bin/opf submit --input draft.json --out-dir opf-data/products --photo nutrition_table=crop_nutrition.jpg --media-out-dir opf-media`), validate (`opf validate-media opf-media`), then `git commit -s && git push` both repos;
2. **Merge the companion `opf-media` PR FIRST** so the WebP blob lands on `main`;
3. **Merge the `opf-data` PR SECOND** (re-running CI if needed)—`opf-data` CI runs `opf check-media-refs` to ensure no broken `opf-media://sha256/...` references can reach production.

*Optional one-step automation (`uvx` + `gh`)*: developers with `uvx` and authenticated `gh` may also run:
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```

---

## Zero-Migration Upgrade to R2 / S3 CDN

Because `opf-data` editions only reference location-independent URIs (`opf-media://sha256/<hex>`), syncing `media/` to **Cloudflare R2, AWS S3, or any CDN** and updating `OPF_MEDIA_BASE_URL` at build time redirects every historical edition with **zero edits to any product JSON**.

## Licensing

Cropped label evidence images are licensed under [**Creative Commons Attribution-ShareAlike 4.0 International (`CC-BY-SA-4.0`)**](LICENSE).
