# 🚀 Bật GitHub Pages chính chủ (tuỳ chọn — nâng cấp)

Trang player hiện đang chạy qua `htmlpreview.github.io` — đã xem được ngay, không cần làm gì thêm.
Nếu bạn muốn player chạy trên **URL chính chủ** của GitHub Pages:

```
https://<username>.github.io/kwan-lmao-2/
```

thì làm theo 2 bước sau (**chỉ làm 1 lần, ~1 phút**):

## Bước 1 — Thêm workflow deploy

Vào repo trên GitHub → tạo file `.github/workflows/deploy-pages.yml` với nội dung:

```yaml
name: Deploy player to GitHub Pages

on:
  push:
    branches: [main]
    paths: ['docs/**']
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

*(Cách làm nhanh trên web: bấm **Add file → Create new file**, paste tên `.github/workflows/deploy-pages.yml` + nội dung trên, Commit.)*

## Bước 2 — Bật Pages

Vào **Settings → Pages** → mục **Build and deployment** → **Source** chọn **GitHub Actions**.

Xong! Chờ ~1-2 phút (xem tab **Actions**), player sẽ chạy tại URL chính chủ ở trên.
Khi đó có thể cập nhật các link trong `README.md` sang URL mới.

## Ghi chú

- Video vẫn stream trực tiếp từ repo (`raw.githubusercontent.com`) nên trang Pages cực nhẹ, deploy trong vài giây.
- Player đã có sẵn nguồn dự phòng: nếu raw bị lỗi sẽ tự chuyển sang release asset (`releases/download/v1.0.0/...`).
