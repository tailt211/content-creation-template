# CLAUDE.md

File này được Claude đọc tự động khi mở folder — áp dụng cho Claude Code, Cowork và mọi môi trường Claude.

## Repo này là gì

Hệ thống context và template tạo content marketing cho **Hải Sản Bình Minh Sài Gòn (Bình Minh SG)** — nhà cung cấp hải sản đông lạnh và thực phẩm Nhật B2B cho nhà hàng Nhật tại TP.HCM và Hà Nội.

Không có code để build hay test. Toàn bộ công việc là đọc context files và tạo written content.

---

## Đọc file nào cho task nào

### Viết bài Facebook / Zalo / catalog copy
1. `context/brand-guideline.md` — tone, màu sắc, những điều không được làm
2. `context/marketing-channels.md` — cấu trúc bài (Dạng 1A–5), hashtag, CTA chuẩn
3. `context/customer-persona.md` — dùng để chọn **format và thân bài** phù hợp; không ảnh hưởng đến tiêu đề và mở bài
4. `context/product-catalog.md` → link đến `context/products/[tên]/detail.md` của sản phẩm cụ thể
5. **Research blog ẩm thực viral về sản phẩm** — dùng đúng 1 query duy nhất: `[tên sản phẩm] blog review ẩm thực Nhật` (ví dụ: "râu bạch tuộc Nhật tako blog review ẩm thực Nhật"). Không search thêm query nào khác, không thêm từ mô tả vào truy vấn. Từ kết quả trang 1, chọn tối đa 1 bài mỗi loại trang theo thứ tự ưu tiên: **page nhà hàng review → food review → page bán hàng → post Facebook → blog cá nhân**. Khi viết bài, lấy insight từ loại trang ưu tiên cao nhất tìm được — nếu có page nhà hàng review thì dùng đó trước, không có thì xuống food review, v.v. **Khi dùng WebFetch để đọc bài blog: prompt phải yêu cầu trích nguyên văn phần mở đầu bài — là những đoạn văn giới thiệu xuất hiện trước khi bài đi vào các mục/tiêu đề nội dung chính. Không tóm tắt, không phân loại, không bỏ đoạn nào, lấy đúng thứ tự từ đầu bài.** Sau khi có nguyên văn, tự đánh giá đoạn nào phù hợp nhất để làm mở bài (không nhất thiết phải độc đáo hay hay nhất — chỉ cần phù hợp). Tham khảo cách đoạn mở đầu được viết — nhịp câu, cảm xúc, góc tiếp cận — rồi viết đoạn mở bài theo phong cách mở đầu tương tự. Lấy 1 điểm nổi bật từ đó làm hook cho tiêu đề. Không phân tích hay chắt lọc insight — viết đúng như một đoạn mở đầu bài. Không tự nghĩ góc khác ngoài những gì research cho thấy. Không sao chép nguyên vẹn, không sai lệch thông tin sản phẩm. Không nhắm persona, không có góc chiến lược. Kết thúc đoạn mở bắt buộc có 1 câu nối vào sản phẩm Bình Minh SG, giọng PR tự nhiên. **Tiêu đề và đoạn mở không được lấy nội dung từ `detail.md`** — chỉ dùng kết quả research. Không tự biến tấu theo ý mình — viết theo đúng phong cách mở đầu của bài research.

### Thêm sản phẩm mới vào catalog
Dùng skill có sẵn — nói tự nhiên "tạo detail cho [tên sản phẩm]" hoặc gõ:
```
/product-detail-generator [tên folder sản phẩm]
```
Skill sẽ tự scan ảnh, research, tạo `detail.md` và cập nhật `product-catalog.md`.

### Cập nhật thông tin thương hiệu / persona
Chỉnh sửa trực tiếp file liên quan trong `context/`.

---

## Cấu trúc context

| File / Folder | Vai trò |
|---|---|
| `context/brand-guideline.md` | Tone of voice, màu sắc, phong cách ảnh, những điều cấm |
| `context/customer-persona.md` | 4 persona: chủ quán / bếp trưởng / F&B manager / người mua lẻ |
| `context/marketing-channels.md` | Cấu trúc bài Facebook (Dạng 1A–5), tone theo kênh, lịch content |
| `context/product-catalog.md` | Danh mục 9 sản phẩm, USP tổng quan, link đến detail.md |
| `context/products/[tên]/detail.md` | Thông số đầy đủ, ứng dụng bếp Nhật, danh sách ảnh từng sản phẩm |
| `context/logo/Binh Minh logo.jpg` | Logo thương hiệu — dùng khi cần đặt vào tài liệu hoặc mockup |

---

## Quy tắc chung

- Không viết bài mà chưa đọc `brand-guideline.md` — dễ sai tone
- Không tự bịa USP hay thông số kỹ thuật sản phẩm — lấy từ `detail.md`
- Khi không chắc thông tin → ghi "(cần xác nhận)", không đoán mò
- **Bắt buộc research blog ẩm thực viral về sản phẩm** — dùng đúng 1 query duy nhất: `[tên sản phẩm] blog review ẩm thực Nhật`. Không search thêm query nào khác, không thêm từ mô tả vào truy vấn. Từ kết quả trang 1, chọn tối đa 1 bài mỗi loại trang theo thứ tự ưu tiên: **page nhà hàng review → food review → page bán hàng → post Facebook → blog cá nhân**. Khi viết bài, lấy insight từ loại trang ưu tiên cao nhất tìm được. **Khi dùng WebFetch: prompt phải yêu cầu trích nguyên văn phần mở đầu bài theo thứ tự từ đầu — là những đoạn văn xuất hiện trước khi bài đi vào các mục/tiêu đề nội dung chính. Không tóm tắt, không phân loại, không bỏ đoạn nào.** Sau khi có nguyên văn, tự đánh giá đoạn nào phù hợp nhất để làm mở bài. Tham khảo cách đoạn mở đầu được viết — nhịp câu, cảm xúc, góc tiếp cận — rồi viết đoạn mở bài theo phong cách mở đầu tương tự. Lấy 1 điểm nổi bật từ đó làm hook cho tiêu đề. Không phân tích hay chắt lọc insight — viết đúng như một đoạn mở đầu bài. Không tự nghĩ góc khác ngoài những gì research cho thấy. Không sao chép nguyên vẹn, không sai lệch thông tin sản phẩm. Không nhắm persona, không có góc chiến lược. Kết thúc đoạn mở bắt buộc có 1 câu nối vào sản phẩm Bình Minh SG, giọng PR tự nhiên. **Tiêu đề và đoạn mở không được lấy nội dung từ `detail.md`** — chỉ dùng kết quả research. Không tự biến tấu theo ý mình — viết theo đúng phong cách mở đầu của bài research.
