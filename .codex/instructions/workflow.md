# Workflow Làm Việc Của Codex

## Nguyên tắc chung

- Đọc bối cảnh trước khi sửa file.
- Giữ nguyên setup của `.claude/`.
- Chỉ dùng chung `context/` với các agent khác.
- Khi tạo content sản phẩm, ưu tiên dữ liệu trong ảnh thực tế và các file hiện có trong folder sản phẩm.
- Không bịa thông tin về xuất xứ, thương hiệu, hạn sử dụng, quy cách đóng gói.

## Khi tạo detail sản phẩm

1. Xác định folder sản phẩm trong `context/products/`.
2. Liệt kê ảnh và video trong folder.
3. Đọc 1-2 file `detail.md` hiện có để nắm format.
4. Phân tích ảnh thực tế: màu sắc, dạng đóng gói, text trên nhãn, quy cách, trạng thái sản phẩm.
5. Research thêm từ tên sản phẩm nếu cần, ưu tiên nguồn đáng tin cậy.
6. Tạo hoặc cập nhật `context/products/[tên sản phẩm]/detail.md`.
7. Cập nhật trực tiếp `context/product-catalog.md`.
8. Báo lại file đã tạo/cập nhật và các thông tin cần xác nhận.

## Khi cập nhật catalog

- Tìm số thứ tự lớn nhất trong `context/product-catalog.md`.
- Entry mới dùng số tiếp theo.
- Chèn entry trước phần hướng dẫn sử dụng catalog nếu có.
- Nếu không có phần hướng dẫn, chèn entry ở cuối file.
- Link detail phải URL-encode khoảng trắng thành `%20`.
- Không tạo entry trùng nếu sản phẩm đã có trong catalog.

## Khi gặp dữ liệu không rõ

- Nếu có thể tiếp tục bằng cách ghi `(cần xác nhận)`, hãy tiếp tục.
- Chỉ hỏi user khi thiếu thông tin làm thay đổi bản chất sản phẩm hoặc có nguy cơ ghi sai nghiêm trọng.
