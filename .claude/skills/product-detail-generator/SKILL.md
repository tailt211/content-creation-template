---
name: product-detail-generator
description: |
  Tự động tạo file detail.md chuẩn cho sản phẩm mới trong hệ thống content của Bình Minh SG,
  đồng thời cập nhật product-catalog.md. Kết hợp research từ tên sản phẩm + phân tích ảnh thực tế.

  Dùng skill này khi user:
  - Nói "tạo detail", "thêm sản phẩm mới", "tạo file sản phẩm"
  - Nhắc đến tên folder sản phẩm trong context/products/ (ví dụ: "Râu bạch tuộc nhật")
  - Muốn onboard sản phẩm vào hệ thống catalog
  - Nói "sản phẩm mới cần detail.md" hay "chưa có detail"

  QUAN TRỌNG: Dùng skill này bất cứ khi nào user đề cập đến sản phẩm hải sản + muốn tạo tài liệu
  hay thêm vào catalog — dù họ không dùng từ "detail" hay "skill" một cách tường minh.
---

# Product Detail Generator — Tạo Detail.md Cho Sản Phẩm Mới

Skill này tự động hóa việc tạo file `detail.md` chuẩn cho sản phẩm mới trong hệ thống content của
**Bình Minh SG** (repo tại `context/products/`). Kết hợp 2 nguồn thông tin:
**research độc lập từ tên sản phẩm** và **quan sát trực tiếp từ ảnh thực tế** trong folder.

## Input

- **Bắt buộc:** Tên folder sản phẩm (ví dụ: `"Cá saba Nauy size 400-600"`)
- **Tự động:** Scan tất cả file ảnh/video trong `context/products/[tên sản phẩm]/`
- User **không cần** cung cấp thêm thông tin — tự research và phân tích ảnh

## Output

1. File `detail.md` hoàn chỉnh tại `context/products/[tên sản phẩm]/detail.md`
2. File `context/product-catalog.md` được cập nhật với entry sản phẩm mới
3. Preview cho user review trước khi lưu

---

## Workflow

### Phase 1: Scan & Research

**Bước 1 — Scan folder:**
- Liệt kê toàn bộ file ảnh (`.jpg`/`.png`/`.jpeg`) và video (`.mp4`/`.mov`) trong folder
- Đếm số lượng → dùng để tính số ảnh đề xuất trong catalog

**Bước 2 — Research từ tên sản phẩm + ảnh:**
- Xuất xứ, nhà sản xuất, thương hiệu (ưu tiên đọc từ nhãn trong ảnh)
- Đặc điểm kỹ thuật: dạng sản phẩm, phương pháp đông lạnh, glazing, size
- Tên tiếng Nhật chính xác (kanji + romaji + giải thích)
- Ứng dụng trong bếp Nhật cụ thể cho **sản phẩm này** — không lấy từ sản phẩm khác
- Bảo quản, hạn sử dụng tiêu chuẩn ngành

### Phase 2: Phân Tích Ảnh

Đọc từng ảnh bằng khả năng vision — **chỉ ghi những gì nhìn thấy trực tiếp**, không suy diễn:

- Màu sắc sản phẩm
- Hình dạng, kích thước tương đối
- Kết cấu bề mặt
- Dạng đóng gói (thùng xốp, khay hút chân không, túi PE...)
- Text trên bao bì/nhãn nếu đọc được
- Chọn ảnh đẹp nhất → ghi chú rõ

Để đọc ảnh, dùng path tuyệt đối vào folder sản phẩm trong repo.

### Phase 3: Tạo detail.md

Đọc 1–2 file `detail.md` hiện có trong `context/products/` để nắm format — chỉ tham khảo
**cấu trúc**, tuyệt đối không copy nội dung sang sản phẩm khác.

### Phase 4: Cập Nhật product-catalog.md

Sau khi user confirm `detail.md`, tự động thêm entry vào `context/product-catalog.md`:

1. Đọc catalog → tìm số thứ tự cuối cùng → số mới = cuối + 1
2. Tính số ảnh đề xuất theo quy tắc bên dưới
3. Viết entry theo format chuẩn (xem section **Format Entry Catalog**)
4. Chèn trước dòng `## Hướng dẫn sử dụng catalog này`

---

## Cấu Trúc detail.md

```markdown
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

**Section tùy chọn:** Thêm bảng Markdown phân loại variant khi sản phẩm có nhiều dòng/loại
(ví dụ: 3 màu nhãn = 3 dòng khác nhau).

---

## Format Entry Catalog

Entry catalog phải theo **đúng 4 dòng bullet** này — không thêm, không bớt, không đổi tên field:

```markdown
---

## [Số thứ tự]. [Tên Sản Phẩm] ([Tên Nhật nếu có])

- **Detail:** [products/[tên folder]/detail.md](products/[tên%20folder%20URL-encoded]/detail.md)
- **Danh mục:** ...
- **USP:** [1–2 câu cụ thể — điểm mạnh chính từ research + ảnh]
- **Số ảnh đề xuất mỗi bài:** [kết quả từ bảng bên dưới]
```

Đây là format duy nhất được chấp nhận. Đọc các entry hiện có trong `product-catalog.md` để thấy ví dụ thực tế — tất cả đều theo cùng 4 bullet này.

**Quy tắc số ảnh đề xuất** (đếm file `.jpg`/`.png`/`.jpeg` trong folder):
| Số ảnh trong folder | Ghi vào field |
|---|---|
| ≤ 3 ảnh | `3 ảnh` |
| 4–7 ảnh | `4–5 ảnh` |
| ≥ 8 ảnh | `4–6 ảnh` |

Link `Detail:` phải URL-encode khoảng trắng thành `%20` (ví dụ: `Sò%20trắng`).

---

## Quy Tắc Quan Trọng

- **Kết hợp 2 nguồn:** Mỗi section ghép thông tin từ research + quan sát ảnh trực tiếp
- **Không copy nội dung** từ `detail.md` sản phẩm khác — chỉ copy cấu trúc
- **Ứng dụng bếp Nhật** phải cụ thể cho sản phẩm này: tên món tiếng Nhật + cách dùng đặc thù
- **Không chắc** → ghi `(cần xác nhận)`, không bịa
- **Tone:** Chuyên môn, cụ thể, không hoa mỹ

---

## Trình Tự Thực Thi

1. Scan folder → đếm ảnh
2. Research sản phẩm + phân tích ảnh
3. Đọc 1–2 `detail.md` mẫu → lấy cấu trúc
4. Tạo `detail.md` hoàn chỉnh → **hiển thị preview toàn bộ**
5. **Chờ user confirm** trước khi lưu
6. Lưu `detail.md`
7. Tự động cập nhật `product-catalog.md` (scan số thứ tự, tính số ảnh, ghi entry mới)
8. **Hiển thị entry mới** trong catalog để user review
9. **Chờ user confirm** → lưu catalog
