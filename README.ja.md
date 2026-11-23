# OpenPackagedFood メディアリポジトリ (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 商品データベース (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 仕様・CLI (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

本リポジトリは **OpenPackagedFood (`opf` v0.2)** 専用の **切り抜きラベル WebP 証拠画像ストア**（SHA-256 コンテンツアドレス格納）です。画像バイナリを [`opf-data`](https://github.com/mickey996icu/opf-data) の JSON データから分離することで、Git 履歴の肥大化と GitHub Pages の配信容量消費を防ぎます。

---

## 格納構造と受け入れ基準

- **コンテンツアドレス配置**: `media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp`（`opf-data` からは `opf-media://sha256/<sha256>` で参照）。
- **機能的テキスト領域の切り抜き画像のみ**: 栄養成分表示・原材料名・アレルゲン・製造者表示・バーコード/内容量（7 種の標準 `region`）のみを受け入れます。**パッケージ表面の商業デザイン全体画像の投稿は厳禁です。**
- **技術・プライバシーゲート（`opf validate-media` CI で自動検証）**:
  - **WebP 形式のみ**、長辺 **`≤ 1000px`**、ファイルサイズ **`≤ 512 KB`**。
  - EXIF の向きを画素に焼き付けたうえで **すべての EXIF / XMP / IPTC メタデータを完全削除**（ファイル名は SHA-256 と一致必須）。

---

## ⚠️ 二重 PR のマージ順（`opf-media` 先、`opf-data` 後）

第 1 段階は開発者向けの純 Git 主経路（`opf submit --photo ... --media-out-dir opf-media` → `opf validate-media` → `git push`）のみを受け付けています（[CONTRIBUTING.md](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.md) 参照）。

1. **同伴の `opf-media` PR を最初にマージ**し、WebP 画像を `main` に反映します。
2. 続いて **`opf-data` PR をマージ**します（`opf check-media-refs` が `opf-media/main` 上の画像実在を検証します）。

任意のワンステップ自動化（`uvx` + `gh`）：
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```
