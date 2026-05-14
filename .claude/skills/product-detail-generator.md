# Product Detail Generator — Tạo Detail.md Cho Sản Phẩm Mới

Workflow này tự động hóa việc tạo file `detail.md` chuẩn cho sản phẩm mới trong hệ thống content của Bình Minh SG. Kết hợp 2 nguồn thông tin: **research độc lập từ tên sản phẩm** và **quan sát trực tiếp từ ảnh thực tế** trong folder.

**Khi nào dùng:** User nhắc đến "tạo detail", "thêm sản phẩm mới", hoặc nêu tên folder sản phẩm chưa có detail.md.

## Input

- **Bắt buộc:** Tên folder sản phẩm (ví dụ: "Râu bạch tuộc nhật")
- **Tự động:** Scan tất cả file ảnh/video trong folder đó
- User **không cần** cung cấp thêm thông tin — tự research và phân tích ảnh

## Output

1. File `detail.md` hoàn chỉnh tại `context/products/[tên sản phẩm]/detail.md`
2. File `context/product-catalog.md` được cập nhật tự động với entry sản phẩm mới
3. Hiển thị preview để user review — **chờ confirm trước khi lưu**

---

## Workflow

### Phase 1: Scan & Research

**Bước 1 — Scan folder:**
- Liệt kê toàn bộ file ảnh (.jpg/.png/.jpeg) và video (.mp4/.mov) trong folder
- Đếm số lượng → dùng để tính số ảnh đề xuất trong catalog

**Bước 2 — Research từ tên sản phẩm + ảnh:**
- Xuất xứ, nhà sản xuất, thương hiệu (ưu tiên đọc từ nhãn trong ảnh)
- Đặc điểm kỹ thuật: dạng sản phẩm, phương pháp đông lạnh, glazing, size
- Tên tiếng Nhật chính xác (kanji + romaji + giải thích)
- Ứng dụng trong bếp Nhật cụ thể cho **sản phẩm này** — không lấy từ sản phẩm khác
- Bảo quản, hạn sử dụng tiêu chuẩn ngành

### Phase 2: Phân Tích Ảnh

Quan sát từng ảnh — **chỉ ghi những gì nhìn thấy trực tiếp**, không suy diễn:

- Màu sắc sản phẩm
- Hình dạng, kích thước tương đối
- Kết cấu bề mặt
- Dạng đóng gói (thùng xốp, khay hút chân không, túi PE...)
- Text trên bao bì/nhãn nếu đọc được
- Chọn ảnh đẹp nhất → ghi chú rõ

### Phase 3: Tạo detail.md

Đọc 1-2 file detail.md hiện có trong `context/products/` để nắm format — chỉ tham khảo **cấu trúc**, không copy nội dung.

### Phase 4: Cập Nhật product-catalog.md

Sau khi tạo xong `detail.md`, tự động thêm entry mới vào `context/product-catalog.md`:

1. Tính toán số thứ tự = số cuối cùng trong catalog + 1
2. Xác định số ảnh đề xuất theo quy tắc (≤3 ảnh → "3 ảnh", 4–7 ảnh → "4–5 ảnh", ≥8 ảnh → "4–6 ảnh")
3. Viết entry theo format chuẩn (xem phần **Cập Nhật product-catalog.md** bên dưới)
4. Thêm vào file trước phần "## Hướng dẫn sử dụng catalog này"

---

## Cấu Trúc detail.md

### Section bắt buộc (theo thứ tự)

```
# [Tên Sản Phẩm]

## Thông tin sản phẩm
- **Tên sản phẩm:** ...
- **Tên tiếng Nhật:** [kanji] ([romaji]) — [giải thích]
- **Xuất xứ:** ... (cần xác nhận nếu không chắc)
- **Thương hiệu:** ... (nếu có)
- **Danh mục:** ...
- **Dạng sản phẩm:** ...
- **Bảo quản:** -18°C trở xuống
- **Hạn sử dụng:** ... (cần xác nhận nếu không chắc)
- **Đóng gói:** ...

## Quy cách đóng gói
- Đơn vị tính: ...
- [Mô tả từ ảnh: thùng xốp/carton/khay, size, trọng lượng, nhãn bao bì...]

## Thông số kỹ thuật (quan sát từ ảnh thực tế)
- **Màu sắc:** ...
- **Hình dạng:** ...
- **Kết cấu:** ...
- **Trạng thái:** ...
- **Đặc tính nổi bật:** ...

## Ứng dụng trong bếp Nhật

**[Nhóm món 1]:**
- **[Tên món Nhật (kanji)]** — [mô tả cách dùng cụ thể]

**[Nhóm món 2]:**
- ...

(Tối thiểu 4 ứng dụng, chia nhóm theo loại món)

## Hình ảnh sản phẩm
- [tên_file.jpg] _(mô tả — đánh dấu ảnh đẹp nhất)_
- ...
```

### Section tùy chọn (thêm khi cần)

- **Phân loại theo dòng/variant** — dùng bảng Markdown khi sản phẩm có nhiều loại (ví dụ: 3 dòng màu nhãn)

---

## Cập Nhật product-catalog.md

Thêm entry theo format:

```
---

## [Số thứ tự]. [Tên Sản Phẩm] ([Tên Nhật nếu có])

- **Detail:** [products/[tên folder]/detail.md](products/[tên%20folder%20URL-encoded]/detail.md)
- **Danh mục:** ...
- **USP:** [1-2 câu cụ thể — điểm mạnh chính từ research + ảnh]
- **Số ảnh đề xuất mỗi bài:** [theo quy tắc bên dưới]
```

**Quy tắc số ảnh đề xuất:**
- ≤3 ảnh → "3 ảnh"
- 4–7 ảnh → "4–5 ảnh"
- ≥8 ảnh → "4–6 ảnh"

Số thứ tự = số cuối cùng trong catalog + 1. Link phải URL-encode khoảng trắng thành `%20`.

---

## Quy Tắc

- **Kết hợp 2 nguồn:** Mỗi section ghép thông tin từ research + quan sát ảnh
- **Không copy nội dung** từ detail.md sản phẩm khác — chỉ copy cấu trúc
- **Ứng dụng bếp Nhật** phải cụ thể cho sản phẩm này, tên món + cách dùng đặc thù
- **Không chắc** → ghi "(cần xác nhận)", không bịa
- **Tone:** Chuyên môn, cụ thể, không hoa mỹ
- **Bước cuối bắt buộc:** Hiển thị preview → chờ user confirm → lưu cả `detail.md` và cập nhật `product-catalog.md`

## Thực Thi

**Bước 1:** Tạo file `detail.md` hoàn chỉnh → hiển thị preview chi tiết  
**Bước 2:** Chờ user confirm entry detail.md  
**Bước 3:** Lưu `detail.md`  
**Bước 4:** Tự động cập nhật `product-catalog.md` (scan số thứ tự, tính số ảnh, ghi entry mới)  
**Bước 5:** Hiển thị toàn bộ entry mới trong catalog để user review  
**Bước 6:** Chờ user confirm → lưu catalog
