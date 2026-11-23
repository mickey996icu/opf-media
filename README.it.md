# Archivio Media OpenPackagedFood (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 Database prodotti (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 Strumenti (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

Questo repository ospita l'**archivio WebP indirizzato per contenuto dei ritagli di etichette** per **OpenPackagedFood (`opf` v0.2)**. Mantenere i binari separati da [`opf-data`](https://github.com/mickey996icu/opf-data) preserva la leggerezza della cronologia Git e di GitHub Pages.

---

## Struttura e regole di ammissione

- **Layout per SHA-256**: `media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp` (riferito in `opf-data` come `opf-media://sha256/<sha256>`).
- **Solo regioni testuali funzionali**: ammessi solo ritagli precisi di tabella nutrizionale, ingredienti, allergeni, blocco produttore e codice a barre/quantità netta. **Vietato caricare la grafica frontale completa della confezione**.
- **Controlli tecnici e privacy (`opf validate-media`)**:
  - **Solo WebP**, lato massimo **`≤ 1000px`**, dimensione **`≤ 512 KB`**;
  - Orientamento applicato ai pixel (`exif_transpose`), **zero metadati EXIF o XMP**, corrispondenza esatta SHA-256.

---

## ⚠️ Ordine di merge per doppie PR (`opf-media` PRIMA, `opf-data` DOPO)

La Fase 1 utilizza il flusso Git puro per sviluppatori (`opf submit --photo ... --media-out-dir opf-media` → `opf validate-media` → `git push`; vedi [CONTRIBUTING.md](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.md)).

1. **Esegui prima il merge della PR `opf-media`** così l'immagine WebP è su `main`;
2. **Esegui poi il merge della PR `opf-data`** (`opf check-media-refs` verifica ogni riferimento `opf-media://`).

Automazione opzionale (`uvx` + `gh`):
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```
