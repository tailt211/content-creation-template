---
name: product-detail-generator
description: |
  Tạo hoặc cập nhật file detail.md cho sản phẩm mới trong context/products/,
  đồng thời cập nhật trực tiếp context/product-catalog.md.

  Dùng skill này khi user:
  - Yêu cầu tạo detail, thêm sản phẩm mới, tạo file sản phẩm.
  - Nhắc đến một folder sản phẩm trong context/products/.
  - Muốn onboard sản phẩm vào catalog.
  - Nói sản phẩm chưa có detail.md.

  Skill này là setup riêng cho Codex, không phụ thuộc .claude/.
---

# Product Detail Generator

Skill này tạo file `detail.md` cho sản phẩm trong `context/products/[tên sản phẩm]/` và cập nhật trực tiếp `context/product-catalog.md`.

## Ranh giới

- Chỉ dùng chung dữ liệu trong `context/`.
- Không đọc `.claude/skills/` như instruction vận hành.
- Không dùng `.claude/settings.json`, `.claude/commands/` hay `CLAUDE.md` làm cấu hình bắt buộc.
- Tất cả output viết bằng tiếng Việt.

## Input

- Bắt buộc: tên folder sản phẩm trong `context/products/`.
- Tự động: scan tất cả ảnh và video trong folder sản phẩm.
- Nếu folder không tồn tại, báo rõ và đề xuất folder gần đúng nếu có.

## Output

1. Tạo hoặc cập nhật:

```text
context/products/[tên sản phẩm]/detail.md
```

2. Cập nhật trực tiếp:

```text
context/product-catalog.md
```

Không cần hỏi confirm trước khi ghi file.

## Workflow

### 1. Scan folder sản phẩm

- Liệt kê ảnh: `.jpg`, `.jpeg`, `.png`, `.webp`.
- Liệt kê video: `.mp4`, `.mov`, `.webm`, `.m4v`.
- Đếm số ảnh để tính field `Số ảnh đề xuất mỗi bài`.
- Ghi nhận tên file ảnh dùng trong section hình ảnh sản phẩm.

### 2. Đọc bối cảnh

Đọc các file nếu có:

- `context/brand-guideline.md`
- `context/customer-persona.md`
- `context/marketing-channels.md`
- `context/product-catalog.md`
- 1-2 file `detail.md` của sản phẩm khác để nắm format

Chỉ tham khảo cấu trúc từ sản phẩm khác, không copy nội dung.

### 3. Research và phân tích ảnh

Kết hợp hai nguồn:

- Research từ tên sản phẩm và thông tin trên bao bì.
- Quan sát trực tiếp từ ảnh trong folder.

Cần xác định:

- Tên tiếng Nhật nếu phù hợp: kanji/kana, romaji, giải thích.
- Xuất xứ, nhà sản xuất, thương hiệu nếu có bằng chứng.
- Dạng sản phẩm, phương pháp đông lạnh, size, glazing nếu xác định được.
- Đóng gói: thùng, khay, túi, hút chân không, trọng lượng nếu nhãn thể hiện.
- Ứng dụng bếp Nhật cụ thể cho sản phẩm.
- Bảo quản và hạn sử dụng tiêu chuẩn nếu có cơ sở.

Nếu không chắc chắn, ghi `(cần xác nhận)`.

### 4. Tạo detail.md

Dùng format:

```markdown
# [Tên Sản Phẩm]

## Thông tin sản phẩm
- **Tên sản phẩm:** ...
- **Tên tiếng Nhật:** ...
- **Xuất xứ:** ...
- **Thương hiệu:** ...
- **Danh mục:** ...
- **Dạng sản phẩm:** ...
- **Bảo quản:** -18°C trở xuống
- **Hạn sử dụng:** ...
- **Đóng gói:** ...

## Quy cách đóng gói
- Đơn vị tính: ...
- Mô tả đóng gói: ...

## Thông số kỹ thuật (quan sát từ ảnh thực tế)
- **Màu sắc:** ...
- **Hình dạng:** ...
- **Kết cấu:** ...
- **Trạng thái:** ...
- **Đặc tính nổi bật:** ...

## Ứng dụng trong bếp Nhật

**[Nhóm món 1]:**
- **[Tên món Nhật]** - [mô tả cách dùng cụ thể]

**[Nhóm món 2]:**
- ...

## Hình ảnh sản phẩm
- [ten_file.jpg] _(mô tả ngắn gọn)_
- ...
```

Có thể thêm section/bảng variant nếu sản phẩm có nhiều dòng, nhiều màu nhãn, nhiều size hoặc nhiều quy cách.

### 5. Cập nhật product-catalog.md

Sau khi ghi `detail.md`, cập nhật `context/product-catalog.md` trực tiếp.

Entry mới theo format:

```markdown
---

## [Số thứ tự]. [Tên Sản Phẩm] ([Tên Nhật nếu có])

- **Detail:** [products/[tên folder]/detail.md](products/[tên%20folder]/detail.md)
- **Danh mục:** ...
- **USP:** ...
- **Số ảnh đề xuất mỗi bài:** ...
```

Quy tắc số ảnh đề xuất:

| Số ảnh trong folder | Ghi vào catalog |
|---|---|
| <= 3 ảnh | `3 ảnh` |
| 4-7 ảnh | `4-5 ảnh` |
| >= 8 ảnh | `4-6 ảnh` |

Quy tắc chèn entry:

- Nếu sản phẩm đã có trong catalog, cập nhật entry hiện có thay vì tạo trùng.
- Nếu sản phẩm chưa có, lấy số thứ tự lớn nhất + 1.
- Chèn trước dòng `## Hướng dẫn sử dụng catalog này` nếu có.
- Nếu không có dòng đó, chèn cuối file.
- Link detail phải URL-encode khoảng trắng thành `%20`.

## Quy tắc chất lượng

- Không copy nội dung từ sản phẩm khác.
- Không bịa thông tin.
- Không ghi xuất xứ, thương hiệu, size, hạn sử dụng nếu không có cơ sở.
- Nếu phân tích ảnh không được vì thiếu tool vision, vẫn tạo file dựa trên tên sản phẩm và ghi rõ các trường cần xác nhận.
- Sau khi hoàn tất, báo lại đường dẫn file đã tạo/cập nhật và các điểm cần user xác nhận.
