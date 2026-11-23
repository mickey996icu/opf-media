# Médias OpenPackagedFood (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 Base produits (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 Outils & Spec (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

Ce dépôt est le **stockage WebP adressé par contenu pour les recadrages d'étiquettes** d'**OpenPackagedFood (`opf` v0.2)**. Séparer les binaires image de [`opf-data`](https://github.com/mickey996icu/opf-data) protège l'historique Git et le quota GitHub Pages.

---

## Organisation & règles strictes d'admission

- **Chemin adressé par hachage** : `media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp` (référencé dans `opf-data` via `opf-media://sha256/<sha256>`).
- **Zones textuelles fonctionnelles uniquement** : tableau nutritionnel, liste d'ingrédients, mentions allergènes, bloc fabricant/usine et code-barres/poids net. **Ne soumettez jamais de visuel complet de face avant** (droit d'auteur et lisibilité OCR).
- **Portes techniques & confidentialité (`opf validate-media`)** :
  - **WebP uniquement**, plus grand côté **`≤ 1000px`**, taille **`≤ 512 Ko`** ;
  - Orientation appliquée aux pixels (`exif_transpose`), **aucune métadonnée EXIF / XMP**, SHA-256 exact vérifié.

---

## ⚠️ Ordre de fusion des doubles PR (`opf-media` EN PREMIER, `opf-data` ENSUITE)

La Phase 1 utilise le flux développeur en Git pur (`opf submit --photo ... --media-out-dir opf-media` → `opf validate-media` → `git push` ; voir [CONTRIBUTING.md](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.md)).

1. **Fusionnez d'abord la PR `opf-media`** afin de publier le WebP sur `main` ;
2. **Fusionnez ensuite la PR `opf-data`** (`opf check-media-refs` bloque toute référence orpheline).

Automatisation optionnelle (`uvx` + `gh`) :
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```
