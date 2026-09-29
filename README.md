# support

Trang Support & Privacy dùng chung cho các app của Khang Nguyen — static HTML, hosted trên GitHub Pages. Không build step, không dependency, không JS.

Mỗi app một thư mục. `assets/site.css` giữ toàn bộ layout; mỗi trang app override màu bằng khối `<style>:root{...}</style>` trong `<head>` của chính nó.

## URL để dán vào App Store Connect

| App | Ô trong ASC | URL |
|---|---|---|
| Fruitette | Support URL | `https://khang-nguyen265.github.io/support/fruitette/` |
| Fruitette | Privacy Policy URL (App Information) | `https://khang-nguyen265.github.io/support/fruitette/privacy.html` |
| Paper Worlds | Support URL | `https://khang-nguyen265.github.io/support/paper/` |
| Paper Worlds | Privacy Policy URL (App Information) | `https://khang-nguyen265.github.io/support/paper/privacy.html` |

Paper Worlds dùng Apple Standard EULA đã được link trong Shop; hiện không cần trang Terms of Use riêng. Build app cần `--dart-define=PAPER_PRIVACY_URL=https://khang-nguyen265.github.io/support/paper/privacy.html` để nút Privacy Policy trong Shop hoạt động. Kiểm tra URL đã publish và nội dung App Privacy trong App Store Connect trước khi submit; Paper Worlds có Firebase Analytics và RevenueCat nên không khai “Data Not Collected”.

Trước khi submit Paper Worlds:

- Mở cả Support URL và Privacy Policy URL công khai trên điện thoại sau khi GitHub Pages deploy.
- Khai App Privacy theo Firebase Analytics và RevenueCat của **bản release thực tế**; kiểm tra bản iOS được build theo cấu hình Firebase không có IDFA như ghi trong repo `paper`, file `lib/main.dart`.
- Build với `PAPER_PRIVACY_URL` ở trên và xác minh nút Privacy Policy trong Shop mở trang thật. Privacy link cũng cần dễ tìm trong app theo App Review Guidelines.
- Không nhầm Restore purchases với khôi phục postcard: postcard và progress lưu cục bộ, còn quyền mua do store và RevenueCat quản lý.

## Deploy lần đầu

```bash
git init
git add .
git commit -m "support site"
git branch -M main
git remote add origin git@github.com:khang-nguyen265/support.git
git push -u origin main
```

Trên GitHub: **Settings → Pages → Build and deployment**
- Source: **Deploy from a branch**
- Branch: **main** · Folder: **/ (root)** → **Save**

Đợi ~1 phút. Sau đó sửa file và push là Pages tự deploy lại.

> **Tên repo `support` nằm trong URL đã nộp cho Apple — đổi tên repo là gãy link.**

## Thêm app mới

1. Copy thư mục `fruitette/` thành `<app>/`.
2. Sửa khối `<style>:root{...}</style>` trong `<head>` của cả hai file, đổ token màu của app đó vào.
3. Viết lại nội dung: privacy theo **hành vi dữ liệu thật của app đó**, FAQ theo tính năng thật.
4. Thêm một dòng vào `.app-list` trong `index.html` gốc.
5. Thêm dòng vào bảng URL ở trên.

**Không sửa `assets/site.css` khi thêm app.** Nếu thấy cần, nghĩa là đang thiếu một biến — thêm biến vào `:root` mặc định của `site.css`, đừng thêm màu của app.

## Rule: mỗi app một privacy policy riêng biệt

Site chung là chung cái **vỏ** (HTML skeleton, CSS, footer). **Không chung nội dung.**

Không bao giờ viết một policy dùng chung mô tả "các app của chúng tôi". Hành vi dữ liệu của chúng khác nhau thật — Inhale/Still có IAP + RevenueCat, Fruitette không có gì, Even So có worker. Một policy gộp mô tả sai hành vi của một app cụ thể vừa là rủi ro bị Apple reject (họ đối chiếu policy với hành vi thật + usage string trong `Info.plist`), vừa là tuyên bố sai sự thật với người dùng.

## Không nằm ở đây

`inhale-site`, `still-site`, `pinly-site` là các repo riêng có từ trước và **vẫn đang chạy** — không đụng tới. Muốn gộp chúng vào đây thì phải sửa URL trong ASC của từng app; đó là việc riêng, không phải việc của repo này.

Ba site cũ dùng `khang.d.d.nguyen@gmail.com`. Fruitette dùng `khang.plant@gmail.com`.
