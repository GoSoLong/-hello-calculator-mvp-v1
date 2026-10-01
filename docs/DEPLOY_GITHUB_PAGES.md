# Triển khai Hello Calculator lên GitHub Pages

## 1. Repository đích

Repository đang dùng:

`GoSoLong/-hello-calculator-mvp-v1`

Nếu tạo repository mới trên GitHub Free, dùng repository **Public** để bật Pages theo cách đơn giản nhất.

## 2. Đẩy phiên bản lên GitHub

Trong Terminal tại thư mục project, stage source (không stage tài liệu PDF hoặc file phát sinh không thuộc source), commit và push:

```bash
git add README.md index.html css js docs tests
git commit -m "Release Hello Calculator MVP v1.1"
git push origin main
```

Remote `origin` của project này đã trỏ đến repository ở trên.

## 3. Bật GitHub Pages

Trong repository trên GitHub:

1. Mở **Settings**.
2. Ở menu bên trái, chọn **Pages**.
3. Trong **Build and deployment**:
   - Source: **Deploy from a branch**
   - Branch: **main**
   - Folder: **/(root)**
4. Nhấn **Save**.

Repository đã có `index.html` ở root và file `.nojekyll`, phù hợp để publish trực tiếp.

## 4. Mở website online

Sau khi deploy thành công, vào:

**Settings → Pages → Visit site**

URL thường có dạng:

`https://gosolong.github.io/-hello-calculator-mvp-v1/`

## 5. Kiểm thử online

- Mở URL bằng máy tính.
- Mở URL bằng điện thoại qua 4G/5G hoặc Wi‑Fi khác để xác nhận truy cập Internet thật.
- Chạy lại các test trong `docs/TEST_CASES.md`.
- Mở DevTools → Console và xác nhận không có lỗi JavaScript.
- DevTools → Toggle device toolbar → kiểm tra viewport 320, 375, 768 px.

## 6. Cập nhật phiên bản sau

```bash
git add README.md index.html css js docs tests
git commit -m "Update Hello Calculator"
git push origin main
```

GitHub Pages sẽ tự triển khai lại từ branch `main`.
