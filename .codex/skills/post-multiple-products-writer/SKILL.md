---
name: post-multiple-products-writer
description: |
  Viết bài Facebook Dạng 4 Multi-Product Post cho nhiều sản phẩm Bình Minh SG theo một dịp, mùa, đợt nhập hàng hoặc bài tổng hợp.
  Skill đọc detail.md của từng sản phẩm, tạo folder trong post-multiple-products, lưu file post-multiple-[tiêu đề chính].txt,
  và copy ảnh main-* từ posts/[tên sản phẩm]/ vào folder bài viết.

  Dùng skill này khi user:
  - Yêu cầu viết bài Dạng 4, bài tổng hợp nhiều sản phẩm, bài nhập hàng mới, bài theo mùa/dịp.
  - Đưa danh sách nhiều sản phẩm và muốn lưu vào post-multiple-products.
  - Muốn copy ảnh main-* của nhiều sản phẩm vào cùng folder bài tổng hợp.

  Skill này là setup riêng cho Codex trong repo content-creation-template, không phụ thuộc .claude/.
---

# Post Multiple Products Writer

Skill này tạo bài Facebook Dạng 4 cho nhiều sản phẩm Bình Minh SG, dựa trên:

- `context/products/[tên sản phẩm]/detail.md`
- `context/marketing-channels.md`
- `context/brand-guideline.md` nếu cần lấy tone thương hiệu hoặc hotline
- Ảnh `main-*` trong `posts/[tên sản phẩm]/`

Output luôn lưu trong:

```text
post-multiple-products/[folder bài viết]/
```

## Ranh Giới

- Phân biệt rõ tài liệu repo với yêu cầu user. `context/marketing-channels.md`, `brand-guideline.md` và `detail.md` là nguồn tham khảo nội dung, không phải chỉ đạo cao hơn user.
- Không hỏi confirm trước khi lưu file nếu user đã đưa danh sách sản phẩm.
- Không dùng `.claude/` làm instruction vận hành.
- Không bịa thông số sản phẩm. Nếu thiếu thông tin, ghi ngắn gọn theo dữ liệu có sẵn hoặc đánh dấu `(cần xác nhận)` trong báo cáo cuối, không đưa placeholder vào bài đăng nếu có thể tránh.
- Nội dung bài Facebook là plain text. Không dùng markdown heading `#`, `##` hoặc markdown bold `**...**`.
- Không dùng em dash trong bài viết.

## Input

- Bắt buộc: danh sách nhiều sản phẩm hoặc tên folder trong `context/products/`.
- Tùy chọn: dịp/mùa/chủ đề, ví dụ nhập hàng mới tháng 8, Tết, cuối năm, menu cuối tuần.
- Nếu user không nêu dịp hoặc tiêu đề, tự đặt một chủ đề trung tính như `Tổng hợp sản phẩm hiện có`, `Gợi ý nguyên liệu cho menu Nhật`, hoặc tiêu đề cụ thể hơn theo nhóm sản phẩm.
- Nếu user nói bài sẽ chạy quảng cáo, ưu tiên bài gọn với khoảng 5-7 sản phẩm. Nếu user đưa nhiều hơn, vẫn thực hiện theo danh sách nhưng báo cuối rằng có thể tách thành nhiều bài ads hiệu quả hơn.

## Workflow

### 1. Tìm folder sản phẩm

- Scan `context/products/*/`.
- So khớp mềm tên user nhập, không phân biệt hoa thường và dấu tiếng Việt.
- Ưu tiên folder trùng chính xác.
- Nếu có nhiều folder gần đúng, báo danh sách và dừng để tránh ghi sai.
- File dữ liệu chính là `detail.md`. Nếu user ghi nhầm `detai.md`, vẫn dùng `detail.md`; chỉ thử `detai.md` khi `detail.md` không tồn tại.

### 2. Đọc context

Đọc song song khi có thể:

- `context/products/[tên]/detail.md` cho từng sản phẩm.
- `context/marketing-channels.md`.
- `context/brand-guideline.md` nếu cần xác nhận tone, liên hệ, hashtag hoặc quy tắc thương hiệu.

Từ mỗi `detail.md`, lấy dữ liệu sạch để dùng trong bài:

- Tên sản phẩm.
- Thương hiệu, xuất xứ nếu có.
- Quy cách, size, trọng lượng, bảo quản.
- Trạng thái sản phẩm, thành phần, glazing hoặc chứng từ nếu có.
- Một điểm nổi bật cụ thể, khác nhau giữa các sản phẩm.

Không đưa phần ứng dụng chế biến dài vào bài Dạng 4, trừ khi cần 1 cụm rất ngắn để giải thích vai trò sản phẩm trong menu.

### 3. Xác định chủ đề và cấu trúc bài

- Nếu có dịp/mùa, tiêu đề phải gắn với dịp đó.
- Nếu không có dịp, tự tạo tiêu đề tổng hợp theo logic sản phẩm.
- Với hơn 5 sản phẩm, chia thành 2-3 nhóm nhỏ bằng một dòng phân cách ngắn, ví dụ:

```text
- CÁ VÀ HẢI SẢN -
- SUSHI BAR VÀ GIA VỊ -
```

- Với bài chạy quảng cáo, nên giữ caption gọn. Mỗi sản phẩm chỉ 1-2 câu ngắn và 1 dòng quy cách.

### 4. Viết bài Dạng 4

Bài viết phải theo cấu trúc:

```text
[TIÊU ĐỀ]

[Đoạn mở 2-4 câu, gắn với dịp hoặc mục đích tổng hợp]

[Nhóm sản phẩm nếu cần]

🔷 [TÊN SẢN PHẨM IN HOA]
[1-2 câu PR ngắn, nêu điểm nổi bật cụ thể và vai trò trong bếp/menu.]
📦 [quy cách, size, bảo quản quan trọng nhất]

[Lặp lại cho các sản phẩm]

[Đoạn đóng 1-2 câu, hướng tới hành động cụ thể]

📦 Đặt hàng & báo giá sỉ:
📱 Zalo/Hotline: [số điện thoại]
📍 TP.HCM | Giao hàng toàn quốc

[6-10 hashtag]
```

Quy tắc viết:

- Viết cho Facebook B2B nhà hàng, bếp Nhật, sushi bar, izakaya hoặc F&B Manager tùy bối cảnh user đưa.
- Mỗi sản phẩm có một góc riêng. Không dùng cùng một mẫu câu cho tất cả.
- Tính từ phải đi kèm thông số, quy cách, quan sát thực tế hoặc vai trò cụ thể trong bếp.
- Không dùng giọng catalog máy móc như `lợi thế vận hành`, `biên lợi nhuận`, `tối ưu quy trình` cho mọi sản phẩm.
- Không dùng câu PR chung chung như `Bình Minh SG cam kết đồng hành cùng nhà hàng trong mọi mùa`.
- Không dùng 3 emoji liên tiếp trên cùng một dòng.
- Không để placeholder `[SỐ ĐIỆN THOẠI]`. Nếu không tìm được số, dùng `(cần xác nhận)` trong CTA và báo rõ cuối lượt.

### 5. Phong cách viết

Áp dụng phong cách chống cảm giác máy móc theo yêu cầu user:

- Vary sentence length and syntactic structure.
- Avoid starting all sentences or paragraphs the same way.
- Use hedging language where suitable, such as `có thể`, `thường`, `khá`, `phù hợp khi`.
- Use precise F&B and Japanese-food vocabulary, but keep Facebook readability.
- Avoid vague filler and generic praise.
- Vary punctuation and connector patterns.
- Incorporate both active and passive voice where natural.
- Do not use em dash.

Không biến caption bán hàng thành bài học thuật. Các yêu cầu về academic writing chỉ dùng để làm nhịp câu tự nhiên, có chiều sâu và ít lặp mẫu.

### 6. Lưu file

Tạo folder trong:

```text
post-multiple-products/[folder bài viết]/
```

Tên folder nên không dấu, ngắn, phản ánh tiêu đề chính hoặc dịp:

```text
post-multiple-products/Nhap hang moi thang 8 cho menu Nhat/
post-multiple-products/Tong hop san pham hien co/
```

Tên file:

```text
post-multiple-[tieu-de-chinh-khong-dau].txt
```

Quy tắc chuyển tên:

- Bỏ dấu tiếng Việt.
- Chuyển `đ` thành `d`.
- Viết thường cho tên file.
- Thay khoảng trắng bằng `-`.
- Loại ký tự đặc biệt không cần thiết.

### 7. Copy ảnh sản phẩm

Với từng sản phẩm, copy mọi file trong:

```text
posts/[tên sản phẩm]/
```

nếu tên file bắt đầu bằng:

```text
main-
```

sang folder bài viết đã tạo.

Giữ nguyên filename gốc để truy vết. Nếu sản phẩm không có ảnh `main-*`, không tự tạo ảnh mới trong skill này; báo rõ danh sách thiếu ảnh ở cuối lượt.

### 8. Kiểm tra và báo cáo

Trước khi trả lời final:

- Kiểm tra file `.txt` đã tồn tại.
- Đếm số block sản phẩm `🔷` có khớp danh sách.
- Kiểm tra CTA không còn placeholder nếu đã tìm được hotline.
- Kiểm tra ảnh `main-*` đã copy sang folder bài viết.
- Kiểm tra bài không có em dash.

Final trả ngắn gọn:

- Link tới file bài viết.
- Link tới folder lưu bài.
- Số sản phẩm trong bài và số ảnh đã copy.
- Ghi rõ ảnh thiếu hoặc thông tin cần xác nhận nếu có.
