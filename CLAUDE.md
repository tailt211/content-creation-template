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
5. **Research blog ẩm thực viral về sản phẩm** — dùng đúng 1 query duy nhất: `[tên sản phẩm] blog review ẩm thực Nhật` (ví dụ: "râu bạch tuộc Nhật tako blog review ẩm thực Nhật"). Không search thêm query nào khác, không thêm từ mô tả vào truy vấn. Từ kết quả trang 1, chọn tối đa 1 bài mỗi loại trang theo thứ tự ưu tiên: **page nhà hàng review → food review → page bán hàng → post Facebook → blog cá nhân**. Khi viết bài, lấy insight từ loại trang ưu tiên cao nhất tìm được — nếu có page nhà hàng review thì dùng đó trước, không có thì xuống food review, v.v. **Khi dùng WebFetch để đọc bài blog: prompt phải yêu cầu trích nguyên văn phần mở đầu bài — là những đoạn văn giới thiệu xuất hiện trước khi bài đi vào các mục/tiêu đề nội dung chính. Không tóm tắt, không phân loại, không bỏ đoạn nào, lấy đúng thứ tự từ đầu bài.** Dù WebFetch trả về nguyên văn hay summary, ghi lại ý chính, góc tiếp cận và phong cách viết của bài (nhịp câu, cảm xúc, cấu trúc câu). Lấy 1 điểm nổi bật từ đó làm hook cho tiêu đề. **Ưu tiên tham khảo và dùng phong cách viết của blog** để viết tiêu đề + đoạn mở — không sao chép nguyên văn nhưng viết theo nhịp câu, cảm xúc, góc tiếp cận của bài đó. Chỉ khi blog không đủ material về phong cách, mới áp dụng **Quy tắc phong cách Anti-AI Detection** (trong `marketing-channels.md` mục Writing Style). Không tự nghĩ góc khác ngoài những gì research cho thấy. Không sao chép nguyên vẹn, không sai lệch thông tin sản phẩm. Không nhắm persona, không có góc chiến lược. Kết thúc đoạn mở bắt buộc có 1 câu nối vào sản phẩm Bình Minh SG, giọng PR tự nhiên. **Tiêu đề và đoạn mở không được lấy nội dung từ `detail.md`** — chỉ dùng kết quả research. **TUYỆT ĐỐI không lấy nội dung từ `detail.md`** để bù vào tiêu đề hay đoạn mở khi thiếu material từ research.

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
- **Bắt buộc research blog ẩm thực viral về sản phẩm** — dùng đúng 1 query duy nhất: `[tên sản phẩm] blog review ẩm thực Nhật`. Không search thêm query nào khác, không thêm từ mô tả vào truy vấn. Từ kết quả trang 1, chọn tối đa 1 bài mỗi loại trang theo thứ tự ưu tiên: **page nhà hàng review → food review → page bán hàng → post Facebook → blog cá nhân**. Khi viết bài, lấy insight từ loại trang ưu tiên cao nhất tìm được. **Khi dùng WebFetch: prompt phải yêu cầu trích nguyên văn phần mở đầu bài. WebFetch có thể trả về nguyên văn hoặc summary.** Dù kết quả nào, ghi lại ý chính, góc tiếp cận và phong cách viết của bài (nhịp câu, cảm xúc, cấu trúc câu). Lấy 1 điểm nổi bật từ đó làm hook cho tiêu đề. **Ưu tiên tham khảo và dùng phong cách viết của blog** để viết tiêu đề + đoạn mở — không sao chép nguyên văn nhưng viết theo nhịp câu, cảm xúc, góc tiếp cận của bài đó. Chỉ khi blog không đủ material về phong cách, mới áp dụng **Quy tắc phong cách Anti-AI Detection** (trong `marketing-channels.md` mục Writing Style). Không tự nghĩ góc khác ngoài những gì research cho thấy. Không sao chép nguyên vẹn, không sai lệch thông tin sản phẩm. Không nhắm persona, không có góc chiến lược. Kết thúc đoạn mở bắt buộc có 1 câu nối vào sản phẩm Bình Minh SG, giọng PR tự nhiên. **Tiêu đề và đoạn mở không được lấy nội dung từ `detail.md`** — chỉ dùng kết quả research. **TUYỆT ĐỐI không lấy nội dung từ `detail.md`** để bù vào tiêu đề hay đoạn mở khi thiếu material từ research.
