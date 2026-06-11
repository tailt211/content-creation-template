---
name: post-1a-writer
description: |
  Viết bài Facebook Dạng 1A Product Post B2B cho một sản phẩm của Bình Minh SG.
  Skill đọc detail.md, brand guideline, marketing channels, research blog ẩm thực Nhật,
  tạo bài gốc, bản tối giản, copy ảnh đề xuất, tự động tạo JSON prompt banner,
  tạo poster image và tạo premium frame image.

  Dùng skill này khi user:
  - Yêu cầu viết bài 1A cho một sản phẩm.
  - Yêu cầu viết bài Facebook B2B cho một sản phẩm.
  - Yêu cầu viết post product B2B.
  - Yêu cầu tạo bài Dạng 1A.

  Skill này là setup riêng cho Codex, không phụ thuộc .claude/.
---

# Post 1A Writer

Skill này viết bài Facebook Dạng 1A cho một sản phẩm, dựa trên:

- `context/products/[tên]/detail.md`
- `context/brand-guideline.md`
- `context/marketing-channels.md`
- Research blog ẩm thực Nhật

Output lưu trực tiếp trong:

```text
posts/[tên sản phẩm]/
```

## Ranh giới

- Chỉ dùng chung dữ liệu trong `context/`.
- Không dùng `.claude/skills/post-1a-writer/SKILL.md` làm instruction vận hành.
- Không hỏi confirm trước khi lưu file.
- Bài viết lưu dạng plain text để copy trực tiếp lên Facebook.
- Không dùng markdown heading `#`, `##` hoặc markdown bold `**...**` trong nội dung bài Facebook.

## Input

- Bắt buộc: tên sản phẩm hoặc tên folder trong `context/products/`.
- Tự động: đọc `detail.md`, `brand-guideline.md`, `marketing-channels.md`.
- Tự động: scan ảnh trong folder sản phẩm.
- Tự động: research blog ẩm thực Nhật.

Nếu không tìm thấy folder khớp, báo rõ và liệt kê folder hiện có trong `context/products/`.

## Output

1. Bài gốc:

```text
posts/[tên sản phẩm]/post-facebook-1a.txt
```

2. Bản tối giản:

```text
posts/[tên sản phẩm]/post-facebook-1a-short.txt
```

3. Ảnh đề xuất:

```text
posts/[tên sản phẩm]/data/[ten-sp-khong-dau]-[số thứ tự].[ext]
```

4. JSON prompt banner từ skill `json-prompt-generator`:

```text
posts/[tên sản phẩm]/data/prompt-template-1.json
```

5. Poster image từ skill `product-poster-image-generator`:

```text
posts/[tên sản phẩm]/...
```

6. Premium frame image từ skill `product-premium-frame-generator`:

```text
posts/[tên sản phẩm]/[file hình ảnh]
```

## Workflow

### 1. Tìm folder sản phẩm

- Scan `context/products/*/`.
- So khớp mềm với tên user nhập, không phân biệt hoa thường.
- Ưu tiên folder trùng chính xác.
- Nếu có nhiều folder gần đúng, báo danh sách và dừng để tránh ghi sai.

### 2. Đọc context

Đọc song song:

- `context/products/[tên]/detail.md`
- `context/brand-guideline.md`
- `context/marketing-channels.md`

Từ `detail.md`, lấy:

- Tên sản phẩm.
- Thương hiệu, xuất xứ, quy cách, bảo quản.
- Trạng thái sản phẩm, glazing, chứng nhận nếu có.
- Đặc điểm nổi bật.
- Danh sách ảnh đề xuất nếu có.

Từ `brand-guideline.md`, lấy:

- Tone voice.
- Từ cấm hoặc cách viết cần tránh.
- Hotline/Zalo và thông tin liên hệ.

Từ `marketing-channels.md`, lấy:

- Cấu trúc Dạng 1A.
- Quy tắc viết Facebook B2B.
- Quy tắc hashtag.
- Quy tắc phong cách chống cảm giác AI nếu cần fallback.

### 3. Xác định ảnh đề xuất

- Ưu tiên ảnh được ghi là ảnh đề xuất trong `detail.md`.
- Nếu không có, scan folder sản phẩm và chọn 3-5 ảnh rõ sản phẩm/bao bì nhất.
- Copy ảnh vào `posts/[tên]/data/`.
- Đổi tên theo format `[ten-sp-khong-dau]-[số thứ tự].[ext]`.

Quy tắc chuyển tên không dấu:

- Bỏ dấu tiếng Việt.
- Chuyển `đ` thành `d`.
- Thay khoảng trắng bằng `-`.
- Viết thường toàn bộ.
- Loại ký tự đặc biệt không cần thiết.

### 4. Research blog ẩm thực

Bắt buộc research trước khi viết.

Dùng đúng 1 query chính:

```text
[tên sản phẩm] blog review ẩm thực Nhật
```

Từ kết quả trang đầu, chọn tối đa 1 bài theo thứ tự ưu tiên:

- Page nhà hàng review.
- Food review.
- Page bán hàng.
- Post Facebook.
- Blog cá nhân.

Khi đọc bài, cần lấy:

- Ý chính của phần mở đầu.
- Góc tiếp cận.
- Nhịp câu và phong cách viết.
- Một điểm nổi bật có thể làm hook.

Không sao chép nguyên văn. Tiêu đề và đoạn mở chỉ dựa trên ý chính từ research, không lấy từ `detail.md`.

Nếu research không đủ material, dùng quy tắc phong cách trong `context/marketing-channels.md` làm fallback và ghi rõ trong báo cáo cuối.

### 5. Viết bài gốc Dạng 1A

Bài viết phải là plain text, không dùng markdown.

Cấu trúc:

```text
[Tiêu đề]

[Đoạn mở]

Đặc điểm sản phẩm
[icon] Label: nội dung
[icon] Label: nội dung
[icon] Label: nội dung
[icon] Label: nội dung

Tại sao chọn [Tên sản phẩm] tại Bình Minh SG?
✅ nội dung
✅ nội dung
✅ nội dung
✅ nội dung

📦 Đặt hàng & báo giá sỉ:
📱 Zalo/Hotline: [số điện thoại]
📍 TP.HCM | Giao hàng toàn quốc

[hashtag]
```

Quy tắc:

- Tiêu đề và đoạn mở ưu tiên phong cách từ blog đã research.
- Đoạn mở phải kết thúc bằng một câu nối tự nhiên vào sản phẩm của Bình Minh SG.
- Phần đặc điểm chọn 4-5 điểm phù hợp nhất, không liệt kê mọi thứ.
- Không có phần ứng dụng chế biến trong bài 1A B2B.
- Tính từ phải đi kèm bằng chứng, quy cách hoặc ngữ cảnh cụ thể.
- Không dùng 3 emoji liên tiếp trên cùng một dòng.
- Không dùng em dash để nối ý.
- Không dùng placeholder. Nếu thiếu hotline, ghi `(cần xác nhận)`.

### 6. Tạo bản tối giản

Sau khi lưu bài gốc, tự động tạo:

```text
posts/[tên sản phẩm]/post-facebook-1a-short.txt
```

Cấu trúc bản short:

- Giữ hook hoặc câu mở mạnh nhất từ bài gốc.
- Đoạn mở 2-3 câu.
- Một dòng specs block: `📦 [quy cách] | Bảo quản [nhiệt độ] | [xuất xứ nếu cần]`.
- 2 dòng lợi ích `✅`.
- CTA giữ mẫu chuẩn.
- 4-5 hashtag.

Không có section riêng "Tại sao chọn...", không có heading markdown.

### 7. Tạo JSON prompt banner

Sau khi đã lưu bài gốc, bản short và ảnh:

- Chạy tiếp workflow của `.codex/skills/json-prompt-generator/SKILL.md` cho cùng sản phẩm.
- Tạo `posts/[tên]/data/prompt-template-1.json`.
- Tạo hoặc cập nhật `posts/[tên]/data/input-3-image.[ext]`.
- Không đưa vào JSON các nhãn dịch vụ hoặc persona như `Tư vấn B2B`, `tư vấn bếp Nhật`, `nhà hàng Nhật`, `hỗ trợ menu`; chỉ lưu dữ liệu sản phẩm, cam kết vận hành và thông tin kiểm chứng được.

### 8. Tạo poster image

Sau khi đã tạo `posts/[tên]/data/prompt-template-1.json` và `posts/[tên]/data/input-3-image.[ext]`:

- Chạy tiếp workflow của `.codex/skills/product-poster-image-generator/SKILL.md` cho cùng sản phẩm.
- Tạo và verify poster/banner image theo rule của skill `product-poster-image-generator`.
- Không tự thay thế workflow này bằng prompt hoặc thao tác imagegen thủ công nếu skill đó có hướng dẫn cụ thể hơn.
- Ghi nhận output path và mọi lỗi verification nếu có.

### 9. Tạo premium frame image

Sau khi đã có ảnh sản phẩm trong `posts/[tên]/data/`:

- Chạy tiếp workflow của `.codex/skills/product-premium-frame-generator/SKILL.md` cho cùng sản phẩm.
- Tạo premium frame image theo rule của skill `product-premium-frame-generator`.
- Dùng default output folder/naming của skill đó, trừ khi user yêu cầu khác.
- Ghi nhận output path, file bị skip và mọi lỗi verification nếu có.

### 10. Lưu file và báo cáo

- Tạo folder `posts/[tên sản phẩm]/data/` nếu chưa có.
- Lưu bài gốc, bản short, ảnh đề xuất, JSON prompt, poster image và premium frame image.
- Không hỏi confirm trước khi lưu.
- Sau khi xong, báo danh sách file đã tạo/cập nhật và các điểm cần xác nhận.

## Quy tắc chất lượng

- Bắt buộc research trước khi viết.
- Tiêu đề và đoạn mở không lấy từ `detail.md`.
- Dữ liệu kỹ thuật lấy từ `detail.md`, không bịa.
- Không để placeholder.
- Không dùng markdown trong nội dung bài Facebook.
- Không tạo nội dung hoa mỹ chung chung.
- Nếu không thể research do lỗi mạng, báo rõ và dùng fallback từ `marketing-channels.md`, nhưng vẫn phải ghi lại hạn chế này trong kết quả.
