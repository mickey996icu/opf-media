# Repositorio de Medios OpenPackagedFood (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 Base de datos (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 Herramientas (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

Este repositorio almacena de forma direccionada por contenido los **recortes WebP de etiquetas alimentarias** de **OpenPackagedFood (`opf` v0.2)**, aislados de [`opf-data`](https://github.com/mickey996icu/opf-data) para proteger el historial de Git y el tamaño de GitHub Pages.

---

## Estructura y reglas de admisión

- **Ruta por hash SHA-256**: `media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp` (referenciado en `opf-data` como `opf-media://sha256/<sha256>`).
- **Solo regiones textuales funcionales**: únicamente recortes ajustados de tabla nutricional, lista de ingredientes, alérgenos, fabricante y código de barras/peso neto. **Queda estrictamente prohibido subir la portada frontal completa**.
- **Puerta técnica y de privacidad (`opf validate-media`)**:
  - **Solo WebP**, lado máximo **`≤ 1000px`**, tamaño **`≤ 512 KB`**;
  - Orientación horneada en píxeles (`exif_transpose`), **cero metadatos EXIF o XMP**, coincidencia exacta con SHA-256.

---

## ⚠️ Orden de fusión de PRs dobles (`opf-media` PRIMERO, `opf-data` DESPUÉS)

La Fase 1 sigue el flujo Git puro para desarrolladores (`opf submit --photo ... --media-out-dir opf-media` → `opf validate-media` → `git push`; véase [CONTRIBUTING.md](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.md)).

1. **Fusione primero la PR de `opf-media`** para que el blob WebP llegue a `main`.
2. **Fusione después la PR de `opf-data`** (`opf check-media-refs` comprueba que existan las referencias).

Automatización opcional (`uvx` + `gh`):
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```
