# OpenPackagedFood Media (`opf-media`)

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Español](README.es.md) | [Italiano](README.it.md)** · **[📦 商品数据主仓 (`opf-data`)](https://github.com/mickey996icu/opf-data)** · **[🧰 工具链与规范 (`open-packaged-food`)](https://github.com/mickey996icu/open-packaged-food)**

本仓库是 **OpenPackagedFood (`opf` v0.2)** 的独立证据图片媒体仓，采用 **SHA-256 内容寻址 WebP 存储**，与 [`opf-data`](https://github.com/mickey996icu/opf-data) 的 JSON 元数据仓库完全物理隔离，避免大体积二进制图膨胀 Git 历史或挤占 GitHub Pages 配额。

---

## 存储结构与严格准入规范

- **目录分片规则**：`media/<sha256[0:2]>/<sha256[2:4]>/<sha256>.webp`（引用 URI 为 `opf-media://sha256/<sha256>`）。
- **仅限局部功能性文字标签裁剪图**：仅接收**营养成分表、配料表、致敏原声明、厂家与 SC 生产许可区、条形码/净含量区**等局部裁剪图；**严禁收录整盒/整瓶包装正面宣传封面图（front-of-pack cover art）**，降低版权风险并保持高 OCR 清晰度。
- **格式与隐私强制要求**：
  - 仅允许 **WebP** 格式，长边像素 **`≤ 1000px`**，单文件 **`≤ 512 KB`**；
  - 必须经 `EXIF` 像素旋转烘焙并**彻底剥离所有 `EXIF` / `XMP` / `IPTC` 隐私元数据**（文件名必须严格等于文件字节的 SHA-256）；
  - 严禁出现人脸、手部、购物小票、快递面单或精确到日的生产喷码批号。

---

## ⚠️ 双仓库 PR 合入顺序红线（先媒体、后数据）

第一阶段仅开放开发者通过三仓纯 Git 流程提交（普通用户投稿将在第二阶段启用 `OPF_SUBMIT_URL` Relay 后经由 Agent 开放；详见 [`CONTRIBUTING.zh-CN.md`](https://github.com/mickey996icu/open-packaged-food/blob/main/CONTRIBUTING.zh-CN.md)）。

当一条投稿同时包含标签裁剪照片与商品数据时：
1. 本地执行 `./open-packaged-food/bin/opf submit --input draft.json --out-dir opf-data/products --photo nutrition_table=crop_nutrition.jpg --media-out-dir opf-media` 生成脱敏 WebP 并完成 `opf validate-media` 校验，再在双仓分别 `git commit -s && git push`；
2. **先合入 `opf-media` 的 Companion Media PR**；
3. 再合入（或重新触发 CI 并合入）`opf-data` 的 Data PR——因为 `opf-data` CI 会运行 `opf check-media-refs` 校验 `evidence[].media_ref` 指向的 `.webp` 已真实存在于 `opf-media` 的 `main` 分支上，避免出现断链。

*可选一键自动化通道（`uvx` + `gh`）*：已登录 `gh` 且安装 `uvx` 的开发者亦可选用：
```bash
uvx --from git+https://github.com/mickey996icu/open-packaged-food@v0.2.0 \
  opf pr --input draft.json --photo nutrition_table=crop_nutrition.jpg
```

---

## 未来对象存储无缝迁移

由于 `opf-data` 中仅存储与域名无关的逻辑 URI（`opf-media://sha256/<hex>`），当图片规模超出 Git 适合承载的体积时，只需将 `media/` 目录同步至 **Cloudflare R2 / AWS S3 / CDN** 并修改 `OPF_MEDIA_BASE_URL` 环境变量重新编译网页，**所有历史商品 JSON 一行都无需修改**。

## 开源许可

裁剪标签证据图片采用 [**Creative Commons Attribution-ShareAlike 4.0 International (`CC-BY-SA-4.0`)**](LICENSE) 协议开放共享。
