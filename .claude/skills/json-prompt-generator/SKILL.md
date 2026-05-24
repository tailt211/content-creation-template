---
name: json-prompt-generator
description: |
  Tạo file JSON prompt cho banner sản phẩm theo template final-template-1.json,
  điền đầy đủ tất cả {variable} từ detail.md của sản phẩm + research thực tế.
  Output lưu tại posts/[tên sản phẩm]/data/prompt-template-1.json.

  Dùng skill này khi user:
  - Nói "tạo json prompt template 1 cho [sản phẩm]"
  - Nói "tạo prompt banner template 1"
  - Muốn tạo file JSON prompt từ final-template-1.json cho một sản phẩm cụ thể
---

# JSON Prompt Generator — Template 1

Skill này tạo file `prompt-template-1.json` hoàn chỉnh cho một sản phẩm, bằng cách lấy cấu trúc
từ `templates/product-post/final-template-1.json` và điền tất cả `{variable}` từ dữ liệu
thực tế của sản phẩm.

## Input

- **Bắt buộc:** Tên sản phẩm (ví dụ: `"Lươn vàng"`)
- **Tự động:** Đọc `context/products/[tên sản phẩm]/detail.md`
- **Tự động:** Scan ảnh trong `context/products/[tên sản phẩm]/` để lấy màu sản phẩm
- Nếu không tìm thấy folder khớp → thông báo cho user, liệt kê các folder có trong `context/products/`

## Output

File `posts/[tên sản phẩm]/data/prompt-template-1.json` hoàn chỉnh, với mọi `{variable}` đã được điền.

---

## Workflow

### Phase 1: Đọc dữ liệu

**Bước 1 — Tìm folder sản phẩm:**
- Glob `context/products/*/` để tìm folder khớp với tên sản phẩm user nhập (so khớp mềm, không phân biệt hoa thường)
- Nếu không tìm thấy → thông báo và dừng

**Bước 2 — Đọc detail.md:**
- Đọc `context/products/[tên]/detail.md`
- Extract các thông tin: tên sản phẩm, tên gốc trên bao bì, thương hiệu, xuất xứ, trọng lượng đơn vị,
  bảo quản, thành phần (nếu có), chứng nhận (nếu có), ứng dụng bếp Nhật

**Bước 3 — Scan ảnh:**
- Glob file ảnh trong folder → dùng vision để xác định màu thực tế sản phẩm (màu sắc nổi bật nhất)
- Ghi `màu_sản_phẩm_1` (màu chính) và `màu_sản_phẩm_2` (màu phụ bao bì)

**Bước 4 — Đọc template gốc:**
- Đọc `templates/product-post/final-template-1.json` để nắm cấu trúc đầy đủ

---

### Phase 2: Điền biến

Dùng thông tin từ detail.md + quan sát ảnh để điền **tất cả** các `{variable}` sau:

| Variable | Nguồn | Ghi chú |
|---|---|---|
| `{tên_sản_phẩm}` | detail.md — tên sản phẩm | Tên đầy đủ |
| `{tagline_phụ}` | Suy luận từ danh mục sản phẩm | Dạng: "[Loại] cao cấp", 3–5 từ |
| `{tagline_chính}` | Đặc tính nổi bật từ detail.md | Cảm xúc/đặc tính, 3–5 từ, ngăn cách bằng dấu – |
| `{badge_dòng}` | Thương hiệu + tên + xuất xứ | Ví dụ: "Nissi AC — Hạt cam đỏ chuẩn Nhật" |
| `{label_quy_cách}` | Trọng lượng đơn vị từ detail.md | Ví dụ: "500g/khay" |
| `{mô_tả_bao_bì_từ_input_3}` | Quan sát ảnh | Mô tả ngắn gọn bao bì |
| `{mô_tả_sản_phẩm_trong_bối_cảnh}` | Suy luận từ loại sản phẩm | Ví dụ: "Muỗng gỗ múc đầy trứng cá hồi" |
| `{màu_1_sản_phẩm}` | Quan sát ảnh | Hex code hoặc mô tả màu |
| `{màu_sản_phẩm_1}` | Quan sát ảnh | Hex code màu chính |
| `{màu_sản_phẩm_2}` | Quan sát ảnh | Hex code màu phụ |
| `{thương_hiệu_gốc}` | detail.md — thương hiệu | Tên nhà sản xuất gốc |
| `{usp_icon_1..5}` + `{usp_caption_1..5}` | USP từ detail.md | 5 điểm mạnh, icon tên mô tả (ví dụ: "snowflake", "fish", "japan-flag") |
| `{cam_kết_icon_1..4}` + `{cam_kết_1..4}` | Tiêu chuẩn B2B hải sản Bình Minh SG | 4 cam kết: IQF/đông lạnh nhanh, không tạp chất, nhập khẩu chính ngạch, an toàn thực phẩm |
| `{ứng_dụng_1..3}` + `{mô_tả_ảnh_1..3}` | Ứng dụng bếp Nhật từ detail.md | Chọn 3 món tiêu biểu nhất |

**Quy tắc điền USP icons (5 items):** Chọn 5 trong số: xuất xứ, đông lạnh IQF, không chất bảo quản,
nhập khẩu chính ngạch, size chuẩn, thương hiệu, hạn sử dụng, phù hợp nhà hàng — ưu tiên điểm mạnh
nổi bật nhất của sản phẩm từ detail.md.

**Quy tắc điền cam kết (4 items):** Dùng cố định 4 cam kết thương hiệu Bình Minh SG:
1. IQF / đông lạnh nhanh (icon: "snowflake")
2. Không tạp chất, đạt chuẩn xuất khẩu (icon: "shield-check")
3. Nhập khẩu chính ngạch (icon: "certificate")
4. An toàn thực phẩm (icon: "leaf" hoặc "checkmark-circle")

**Quy tắc điền ứng dụng (3 items):** Lấy từ section "Ứng dụng trong bếp Nhật" của detail.md.
Chọn 3 món tiêu biểu nhất (ưu tiên món có tên tiếng Nhật rõ ràng).
Brief ảnh mỗi món: "[Tên món] nhìn từ trên xuống 45°, sản phẩm là nhân vật chính trong bát/đĩa ceramic
Nhật, ánh sáng tự nhiên, nền đá/gỗ tối."

---

### Phase 3: Tạo file JSON output

Tạo JSON output theo đúng cấu trúc của `final-template-1.json`, với:
- Tất cả phần `FIXED` giữ nguyên y hệt template
- Tất cả `{variable}` đã được thay thế bằng giá trị thực tế
- Thêm section `"thông_tin_sản_phẩm_đã_điền"` ở đầu (sau `_meta`) để tổng hợp tất cả giá trị đã dùng
- Không để lại bất kỳ `{variable}` nào chưa điền — nếu không có dữ liệu ghi `"(cần xác nhận)"`

**Thêm field `"nguồn_dữ_liệu"` vào `_meta`:**
```json
"nguồn_dữ_liệu": {
  "detail_md": "context/products/[tên folder]/detail.md",
  "ảnh_scan": ["danh sách file ảnh đã đọc"],
  "generated_at": "[ngày tạo]"
}
```

**Quy tắc `file_ref` trong `input_3`:** Ghi tên file đã đổi tên: `"input-3-image.[ext]"` (ví dụ: `"input-3-image.jpg"`), không ghi tên gốc hay đường dẫn đầy đủ.

---

### Phase 4: Lưu file

1. Kiểm tra folder `posts/[tên sản phẩm]/data/` — tạo nếu chưa có
2. Hiển thị preview phần `thông_tin_sản_phẩm_đã_điền` để user review nhanh
3. **Chờ user confirm** trước khi lưu
4. Lưu file `posts/[tên sản phẩm]/data/prompt-template-1.json`
5. Copy file ảnh dùng làm `input_3` (file_ref) vào `posts/[tên sản phẩm]/data/` và **đổi tên thành `input-3-image.[ext]`** (giữ nguyên đuôi file gốc, ví dụ `.jpg`)
6. Thông báo đường dẫn file đã lưu

---

## Quy Tắc Quan Trọng

- **Chỉ dùng template final-template-1.json** — không tự đổi cấu trúc hay thêm zone
- **Không để lại `{variable}` trống** — phải điền hết, dùng "(cần xác nhận)" nếu thiếu dữ liệu
- **Dữ liệu từ detail.md là nguồn chính** — không bịa thông số kỹ thuật
- **FIXED content giữ nguyên** — Zone F (footer), CTA text, header labels không thay đổi
- Khi không tìm được folder khớp → liệt kê folder hiện có để user chọn lại

---

## Trình Tự Thực Thi

1. Tìm folder sản phẩm trong `context/products/`
2. Đọc `detail.md` + scan ảnh
3. Đọc `final-template-1.json`
4. Điền tất cả variable → tạo JSON hoàn chỉnh
5. Hiển thị preview `thông_tin_sản_phẩm_đã_điền`
6. **Chờ user confirm**
7. Tạo folder `posts/[tên]/data/` nếu cần → lưu `prompt-template-1.json`
8. Copy file ảnh `input_3` vào `posts/[tên]/data/` → đổi tên thành `input-3-image.[ext]`
9. Cập nhật `file_ref` trong JSON thành `"input-3-image.[ext]"` cho đồng bộ
10. Thông báo path đầy đủ
