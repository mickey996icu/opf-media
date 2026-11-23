# OpenPackagedFood Medienspeicher (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 Produktdatenbank (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 Tooling & Spezifikation (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

Dieses Repository bildet den inhaltsadressierten **WebP-Belegspeicher für zugeschnittene Etikettfotos** von **OpenPackagedFood (`opf` v0.2)**. Durch die strikte Trennung von [`opf-data`](https://github.com/mickey996icu/opf-data) bleiben Git-Historie und GitHub-Pages-Artefakte kompakt.

---

## Struktur & Aufnahmeregeln

- **Inhaltsadressierter Pfad**: `media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp` (Referenz in `opf-data` als `opf-media://sha256/<sha256>`).
- **Nur funktionale Text-Zuschnitte**: Zugelassen sind ausschließlich eng zugeschnittene Textbereiche (Nährwerttabelle, Zutatenliste, Allergene, Hersteller/Werkscode, Barcode/Füllmenge). **Komplette Vorderseiten-Coverfotos sind strikt untersagt** (Urheberrechts- und OCR-Schutz).
- **Technisches & Datenschutz-Gate (`opf validate-media`)**:
  - **Nur WebP**, längste Kante **`≤ 1000px`**, Dateigröße **`≤ 512 KB`**;
  - Ausrichtung in Pixel eingebacken (`exif_transpose`), **keine EXIF- oder XMP-Metadaten**, Dateiname = SHA-256.

---

## ⚠️ Dual-PR-Reihenfolge (`opf-media` ZUERST, `opf-data` DANACH)

Phase 1 nutzt den reinen Git-Entwickler-Workflow (`opf submit --photo ... --media-out-dir opf-media` → `opf validate-media` → `git push`; siehe [CONTRIBUTING.md](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.md)).

1. **Zuerst den Companion-PR in `opf-media` mergen**, damit das WebP auf `main` liegt;
2. **Anschließend den PR in `opf-data` mergen** (`opf check-media-refs` prüft auf fehlende Medien).

Optionale Ein-Schritt-Automatisierung (`uvx` + `gh`):
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```
