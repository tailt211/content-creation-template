---
name: json-prompt-generator
description: |
  Tạo file JSON prompt banner cho một sản phẩm theo templates/product-post/final-template-1.json.
  Skill đọc detail.md, scan ảnh sản phẩm, điền toàn bộ biến trong template, lưu trực tiếp vào posts/[tên sản phẩm]/data/prompt-template-1.json.

  Dùng skill này khi user:
  - Yêu cầu tạo JSON prompt template 1 cho một sản phẩm.
  - Yêu cầu tạo prompt banner template 1.
  - Muốn tạo prompt từ final-template-1.json dựa trên detail.md.

  Skill này là setup riêng cho Codex, không phụ thuộc .claude/.
---

# JSON Prompt Generator

Skill này tạo file `prompt-template-1.json` hoàn chỉnh cho một sản phẩm, dựa trên template:

```text
templates/product-post/final-template-1.json
```

Output lưu trực tiếp tại:

```text
posts/[tên sản phẩm]/data/prompt-template-1.json
```

## Ranh giới

- Chỉ dùng chung dữ liệu trong `context/`.
- Không dùng `.claude/skills/json-prompt-generator/SKILL.md` làm instruction vận hành.
- Không hỏi confirm trước khi lưu file.
- Tất cả nội dung mô tả, metadata và giá trị điền vào template viết bằng tiếng Việt.

## Input

- Bắt buộc: tên sản phẩm hoặc tên folder trong `context/products/`.
- Tự động: đọc `context/products/[tên sản phẩm]/detail.md`.
- Tự động: scan ảnh trong folder sản phẩm để chọn ảnh input chính và xác định màu sản phẩm.
- Tự động: đọc `templates/product-post/final-template-1.json`.

Nếu không tìm thấy folder khớp, báo rõ và liệt kê các folder hiện có trong `context/products/`.

## Output

1. File JSON:

```text
posts/[tên sản phẩm]/data/prompt-template-1.json
```

2. Ảnh input chính:

```text
posts/[tên sản phẩm]/data/input-3-image.[ext]
```

## Workflow

### 1. Tìm folder sản phẩm

- Scan `context/products/*/`.
- So khớp mềm theo tên user nhập, không phân biệt hoa thường.
- Ưu tiên folder trùng chính xác trước, sau đó mới dùng folder gần đúng.
- Nếu có nhiều folder gần đúng, báo lại danh sách và dừng để tránh ghi sai sản phẩm.

### 2. Đọc dữ liệu sản phẩm

Đọc:

- `context/products/[tên]/detail.md`
- Danh sách ảnh trong `context/products/[tên]/`
- `context/brand-guideline.md` nếu cần lấy cam kết thương hiệu
- `templates/product-post/final-template-1.json`

Từ `detail.md`, extract:

- Tên sản phẩm.
- Tên gốc trên bao bì nếu có.
- Thương hiệu, xuất xứ, danh mục.
- Quy cách, trọng lượng đơn vị, bảo quản.
- Trạng thái sản phẩm, đặc điểm kỹ thuật.
- Ứng dụng trong bếp Nhật.
- Danh sách ảnh đề xuất nếu có.
- Chỉ lấy thêm từ `context/brand-guideline.md` các cam kết thương hiệu có thể thể hiện bằng thông tin vận hành hoặc kiểm chứng được, ví dụ: nguồn gốc rõ ràng, đúng quy cách, bảo quản lạnh đúng chuẩn, đóng gói chuyên nghiệp.

### 3. Chọn ảnh input chính

- Ưu tiên ảnh được đánh dấu là ảnh đẹp nhất hoặc ảnh đề xuất trong `detail.md`.
- Nếu không có, chọn ảnh rõ bao bì và sản phẩm nhất trong folder.
- Copy ảnh đã chọn vào `posts/[tên]/data/`.
- Đổi tên thành `input-3-image.[ext]`, giữ nguyên đuôi file gốc.
- Trong JSON, `file_ref` của input chính phải là `"input-3-image.[ext]"`, không dùng path đầy đủ.

### 4. Điền toàn bộ biến

Dùng dữ liệu từ `detail.md`, ảnh và brand context để điền toàn bộ `{variable}` trong template.

Các nhóm biến chính:

- `{tên_sản_phẩm}`: tên đầy đủ của sản phẩm.
- `{tagline_phụ}`: mô tả ngắn 3-5 từ theo danh mục.
- `{tagline_chính}`: đặc tính nổi bật 3-5 từ.
- `{badge_dòng}`: thương hiệu, xuất xứ hoặc điểm nhận diện chính.
- `{label_quy_cách}`: quy cách/trọng lượng đơn vị nếu có.
- `{mô_tả_bao_bì_từ_input_3}`: mô tả ngắn bao bì từ ảnh input chính.
- `{mô_tả_sản_phẩm_trong_bối_cảnh}`: mô tả cảnh chụp phù hợp với sản phẩm.
- `{màu_1_sản_phẩm}`, `{màu_sản_phẩm_1}`, `{màu_sản_phẩm_2}`: màu chính/phụ từ ảnh.
- `{thương_hiệu_gốc}`: thương hiệu hoặc nhà sản xuất.
- `{usp_icon_1..5}` và `{usp_caption_1..5}`: 5 điểm mạnh.
- `{cam_kết_icon_1..4}` và `{cam_kết_1..4}`: 4 cam kết thương hiệu nhưng phải là cam kết dạng vận hành/chất lượng gắn với sản phẩm hoặc giao nhận.
- `{ứng_dụng_1..3}` và `{mô_tả_ảnh_1..3}`: 3 ứng dụng tiêu biểu trong bếp Nhật.

Nếu thiếu dữ liệu, điền `(cần xác nhận)`. Không để lại placeholder dạng `{...}`.

Ràng buộc bắt buộc khi điền:

- Không lưu vào JSON các nhãn dịch vụ, persona hoặc ngành khách hàng như `Tư vấn B2B`, `tư vấn bếp Nhật`, `phù hợp nhà hàng Nhật`, `hỗ trợ menu`, `đồng hành cùng bếp`.
- Tuyệt đối không đưa bất kỳ nhãn nào chứa ý `tư vấn`, `hỗ trợ`, `đồng hành`, `B2B`, `menu`, `phù hợp nhà hàng` vào box `cam_kết`, kể cả khi chỉ là cách diễn đạt mềm hơn của dịch vụ.
- Không dùng các cụm mơ hồ thiên về bán hàng hoặc tư vấn nếu chúng không phải thuộc tính của sản phẩm hay quy trình giao hàng.
- `cam_kết_*` chỉ được phép là các ý có thể kiểm chứng hoặc suy ra trực tiếp từ `detail.md`, bao bì, ảnh, hoặc context thương hiệu mức vận hành.
- Ưu tiên các nhãn ngắn như: `Nguồn gốc rõ ràng`, `Đúng quy cách`, `Bảo quản lạnh chuẩn`, `Đóng gói chuyên nghiệp`, `Theo nhãn từng lô`.
- Nếu không đủ 4 cam kết thực chất, dùng `(cần xác nhận)` cho mục còn thiếu thay vì tự thêm nhãn tư vấn/dịch vụ.

### 5. Giữ nguyên cấu trúc template

- Giữ nguyên toàn bộ field cố định trong `final-template-1.json`.
- Không thêm zone mới.
- Không xóa field có sẵn.
- Chỉ thay các placeholder bằng giá trị thực tế.

Thêm hoặc cập nhật trong `_meta`:

```json
"nguồn_dữ_liệu": {
  "detail_md": "context/products/[tên folder]/detail.md",
  "ảnh_scan": ["danh sách file ảnh đã đọc"],
  "generated_at": "[ngày tạo]"
}
```

Thêm section sau `_meta` nếu template cho phép:

```json
"thông_tin_sản_phẩm_đã_điền": {
  "tên_sản_phẩm": "...",
  "thương_hiệu": "...",
  "xuất_xứ": "...",
  "quy_cách": "...",
  "ảnh_input_chính": "input-3-image.[ext]",
  "ghi_chú_cần_xác_nhận": ["..."]
}
```

### 6. Lưu file

- Tạo folder `posts/[tên sản phẩm]/data/` nếu chưa có.
- Lưu JSON vào `posts/[tên sản phẩm]/data/prompt-template-1.json`.
- Copy ảnh input chính vào cùng folder.
- Không hỏi confirm trước khi lưu.
- Sau khi lưu, báo lại đường dẫn file JSON và ảnh input chính.

## Quy tắc chất lượng

- Không tự đổi cấu trúc template.
- Không để sót placeholder `{variable}`.
- Không bịa thông số kỹ thuật.
- Dữ liệu từ `detail.md` là nguồn chính.
- Không đưa thông tin tư vấn, chân dung khách hàng, tên nhóm ngành, hoặc thông điệp dịch vụ vào các field dữ liệu sản phẩm trong JSON.
- Box `cam_kết` chỉ dành cho cam kết chất lượng/vận hành kiểm chứng được. Mọi nhãn mang tính tư vấn hay hỗ trợ phải bị loại bỏ khỏi `cam_kết`.
- Nếu phân tích ảnh không thực hiện được, ghi rõ các trường cần xác nhận và vẫn tạo JSON hoàn chỉnh.
