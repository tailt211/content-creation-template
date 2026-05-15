---
description: Tạo file detail.md chuẩn cho sản phẩm mới + cập nhật product-catalog.md. Dùng: /product-detail-generator [tên folder sản phẩm]
---

Tạo file `detail.md` cho sản phẩm có tên folder là: **$ARGUMENTS**

Thực hiện đúng theo trình tự sau:

## Bước 1 — Scan folder

Liệt kê toàn bộ file ảnh (`.jpg`/`.png`/`.jpeg`) và video (`.mp4`/`.mov`) trong `context/products/$ARGUMENTS/`. Đếm số lượng ảnh để tính số ảnh đề xuất sau.

## Bước 2 — Research sản phẩm

Từ tên sản phẩm và ảnh, tìm hiểu:
- Xuất xứ, nhà sản xuất, thương hiệu (ưu tiên đọc từ nhãn trên ảnh)
- Đặc điểm kỹ thuật: dạng sản phẩm, phương pháp đông lạnh, glazing, size
- Tên tiếng Nhật chính xác (kanji + romaji + giải thích)
- Ứng dụng trong bếp Nhật cụ thể cho sản phẩm này — không lấy từ sản phẩm khác
- Bảo quản, hạn sử dụng tiêu chuẩn ngành

## Bước 3 — Phân tích ảnh

Đọc từng ảnh bằng vision — chỉ ghi những gì nhìn thấy trực tiếp, không suy diễn:
- Màu sắc, hình dạng, kết cấu bề mặt
- Dạng đóng gói (thùng xốp, khay hút chân không, túi PE...)
- Text trên bao bì/nhãn nếu đọc được
- Chọn ảnh đẹp nhất, ghi chú rõ

## Bước 4 — Tham khảo cấu trúc

Đọc 1–2 file `detail.md` hiện có trong `context/products/` để nắm format — chỉ tham khảo cấu trúc, không copy nội dung.

## Bước 5 — Tạo detail.md theo template

Tạo file với đúng các section sau theo thứ tự:

# [Tên Sản Phẩm]

## Thông tin sản phẩm
- **Tên sản phẩm:** ...
- **Tên tiếng Nhật:** [kanji] ([romaji]) — [giải thích]
- **Xuất xứ:** ... (ghi "(cần xác nhận)" nếu không chắc)
- **Thương hiệu:** ... (nếu có)
- **Danh mục:** ...
- **Dạng sản phẩm:** ...
- **Bảo quản:** -18°C trở xuống
- **Hạn sử dụng:** ... (ghi "(cần xác nhận)" nếu không chắc)
- **Đóng gói:** ...

## Quy cách đóng gói
- Đơn vị tính: ...
- [Mô tả từ ảnh: thùng/khay/túi, size, trọng lượng, nhãn...]

## Thông số kỹ thuật (quan sát từ ảnh thực tế)
- **Màu sắc:** ...
- **Hình dạng:** ...
- **Kết cấu:** ...
- **Trạng thái:** ...
- **Đặc tính nổi bật:** ...

## Ứng dụng trong bếp Nhật

**[Nhóm món 1]:**
- **[Tên món Nhật (kanji)]** — [mô tả cách dùng cụ thể]

(Tối thiểu 4 ứng dụng, chia nhóm theo loại món)

## Hình ảnh sản phẩm
- [tên_file.jpg] _(mô tả — đánh dấu ảnh đẹp nhất)_

## Bước 6 — Hiển thị preview và chờ confirm

Hiển thị toàn bộ nội dung detail.md vừa tạo. Dừng lại, chờ user xác nhận trước khi lưu.

## Bước 7 — Lưu detail.md

Sau khi user confirm → lưu vào context/products/$ARGUMENTS/detail.md

## Bước 8 — Cập nhật product-catalog.md

Đọc context/product-catalog.md, xác định số thứ tự tiếp theo, tính số ảnh đề xuất theo quy tắc:
- 3 ảnh hoặc ít hơn → "3 ảnh"
- 4 đến 7 ảnh → "4–5 ảnh"
- 8 ảnh trở lên → "4–6 ảnh"

Thêm entry theo đúng format này (4 bullet, không thêm bớt):

---

## [Số thứ tự]. [Tên Sản Phẩm] ([Tên Nhật nếu có])

- **Detail:** [products/$ARGUMENTS/detail.md](products/[URL-encoded]/detail.md)
- **Danh mục:** ...
- **USP:** [1–2 câu cụ thể]
- **Số ảnh đề xuất mỗi bài:** [kết quả từ quy tắc trên]

Chèn entry trước dòng "## Hướng dẫn sử dụng catalog này". URL-encode khoảng trắng thành %20.

## Bước 9 — Hiển thị entry và chờ confirm

Hiển thị entry mới. Dừng lại, chờ user xác nhận trước khi lưu.

## Bước 10 — Lưu catalog

Sau khi user confirm → lưu context/product-catalog.md

---

Quy tắc bắt buộc:
- Kết hợp research + quan sát ảnh thực tế cho mọi section
- Không copy nội dung từ sản phẩm khác — chỉ copy cấu trúc
- Không chắc → ghi "(cần xác nhận)", không bịa
- Tone: chuyên môn, cụ thể, không hoa mỹ
