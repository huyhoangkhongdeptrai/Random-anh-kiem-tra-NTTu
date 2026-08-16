# Hướng dẫn "Kho đề mẫu" — chỉ cần kéo cả thư mục ảnh lên, không cần soạn danh sách

## Cách hoạt động (mặc định — nhanh nhất)
Web sẽ **tự động quét** thư mục `samples/` trên GitHub bằng GitHub API:
mỗi thư mục con trong `samples/` = 1 "bộ đề mẫu", tên thư mục = tên bộ đề
hiển thị cho người dùng, và mọi ảnh (.jpg/.jpeg/.png/.gif/.webp/.avif) trong
đó sẽ tự được liệt kê — **không cần tạo hay sửa file danh sách nào cả**.

> Vì sao không dùng Google Drive? Link chia sẻ Drive không cho trình duyệt
> tải ảnh trực tiếp bằng `fetch`/`<img>` do CORS và cơ chế xác thực của
> Google. GitHub Pages + GitHub API là cách miễn phí, ổn định và không cần
> đăng nhập.

## Cách dùng — chỉ 2 bước

1. Trong repo GitHub, tạo một thư mục con mới trong `samples/`, ví dụ:
   ```
   samples/de-gia-dinh/
   ```
2. Kéo thả **cả một thư mục ảnh** từ máy vào đó (qua giao diện web GitHub:
   kéo thả nhiều file cùng lúc vào thư mục, hoặc qua Git/GitHub Desktop nếu
   quen dùng) rồi Commit.

Xong — không cần sửa gì trong file HTML hay tạo file `manifest.json`. Mở
lại web, bấm **"📦 Dùng kho đề mẫu có sẵn"** sẽ tự thấy bộ đề mới.

Cấu trúc ví dụ:
```
ten-repo/
├── index.html                 <- đổi tên file HTML của bạn thành index.html
└── samples/
    ├── de-gia-dinh/
    │   ├── 1.jpg
    │   ├── 2.jpg
    │   └── 3.jpg
    └── de-dong-vat/
        ├── a.png
        └── b.png
```
→ Web sẽ tự hiện 2 bộ: "de-gia-dinh" (3 ảnh) và "de-dong-vat" (2 ảnh).

## Muốn tên bộ đề đẹp hơn / có mô tả? (tùy chọn, không bắt buộc)
Nếu muốn hiển thị tên đẹp (có dấu, khoảng trắng) hoặc thêm mô tả ngắn thay
vì dùng thẳng tên thư mục, bạn có thể tạo thêm file `samples/manifest.json`
— khi có file này, web sẽ ưu tiên dùng nó thay vì tự quét:

```json
[
  {
    "id": "de-gia-dinh",
    "name": "Đề mẫu: Gia đình",
    "description": "Ảnh minh hoạ chủ đề gia đình",
    "folder": "samples/de-gia-dinh",
    "images": ["1.jpg", "2.jpg", "3.jpg"]
  }
]
```
Trong trường hợp này bạn vẫn phải tự liệt kê tên file ảnh — nên chỉ nên dùng
manifest.json khi thực sự cần đặt tên/mô tả tuỳ chỉnh; còn lại cứ để web tự
quét cho nhanh.

## Cách đưa lên GitHub Pages
1. Tạo một repository mới trên GitHub (public).
2. Đổi tên file `tải_ảnh_lên_và_random_V3.html` (đã được bổ sung tính năng)
   thành `index.html`, đưa vào thư mục gốc repo, cùng thư mục `samples/`.
3. Vào **Settings → Pages**, chọn nhánh (thường `main`) và thư mục gốc
   (`/root`), bấm Save.
4. Sau vài phút sẽ có link dạng
   `https://<ten-tai-khoan>.github.io/<ten-repo>/` — chia sẻ link này.

Web sẽ **tự nhận diện owner/repo từ chính URL** của trang (dạng
`<user>.github.io/<repo>/`), nên không cần cấu hình gì thêm. Nếu bạn host
theo cách khác (domain riêng, hoặc trang gốc `<user>.github.io` không có
tên repo trong URL), mở file HTML tìm đến đoạn:
```js
const GITHUB_OWNER = '';
const GITHUB_REPO = '';
const GITHUB_BRANCH = 'main';
```
và điền tay username + tên repo GitHub của bạn vào đó (đổi `main` thành
`master` nếu repo dùng nhánh đó).

## Lưu ý
- Repo phải để **public** thì GitHub API mới đọc được thư mục `samples/`
  miễn phí, không cần đăng nhập.
- GitHub API cho phép khoảng 60 lượt gọi/giờ với mỗi người dùng chưa đăng
  nhập — với quy mô web cá nhân/nhóm nhỏ thì thoải mái dùng.
- Nếu mở file HTML trực tiếp từ máy (đường dẫn `file://...`) thay vì qua
  GitHub Pages, trình duyệt sẽ chặn các lệnh gọi mạng này — web sẽ báo lỗi,
  đây là điều bình thường.

## Muốn test thử trên máy trước khi đưa lên GitHub?
```bash
python3 -m http.server 8000
```
rồi mở `http://localhost:8000`. Lưu ý: chế độ tự quét qua GitHub API chỉ
hoạt động khi trang thực sự được host trên `*.github.io` (hoặc bạn điền tay
`GITHUB_OWNER`/`GITHUB_REPO`) — chạy `localhost` để test giao diện thì cần
đã đẩy ảnh lên GitHub trước đó, hoặc dùng file `manifest.json` để test cục
bộ.
