## 关联商品与数据 PR / Target Product & Companion Data PR

- **商品标识 / Product ID**: `gtin:`
- **关联数据仓 PR（后合入）/ Companion `opf-data` PR (merge second)**:

## 自检清单 / Pre-Merge Checklist

- [ ] **本地媒体校验 / Media Validation**：已运行 `opf validate-media` 且全部通过（内容寻址 `media/<h0:2>/<h2:4>/<sha256>.webp`、`<= 1000px`、无 `EXIF/XMP` 元数据）。 / Passed `opf validate-media` (content-addressed WebP, `<= 1000px`, stripped of `EXIF/XMP` chunks).
- [ ] **功能裁剪与隐私门禁 / Functional-Crop & Privacy Gate**：仅上传局部功能性标签文字裁剪图（营养成分表、配料框、厂家/致敏原、条码净含量），**严禁整盒正面商业美工封面图**，且图片中不包含购买小票、人脸/手部或精确到日的生产/到期批号。 / Cropped functional text regions only (no front-of-pack cover art, receipts, faces/fingers, or day-precision lot/expiry inkjets).
- [ ] **合入顺序 / Dual-PR Merge Order**：本 `opf-media` PR 必须**先于**关联的 `opf-data` PR 合入 `main`。 / Merge this `opf-media` PR **FIRST**, before merging the companion `opf-data` PR.
- [ ] **原创与许可声明 / DCO & Open License**：提交包含 `Signed-off-by`（`git commit -s`），裁剪标签图遵循 `CC BY-SA 4.0`。 / Signed-off DCO (`git commit -s`), licensed under `CC BY-SA 4.0`.
