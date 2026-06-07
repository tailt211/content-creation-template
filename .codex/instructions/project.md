# Hướng Dẫn Project Cho Codex

Project này dùng để tạo và quản lý content sản phẩm cho Bình Minh SG, tập trung vào hải sản, thực phẩm đông lạnh và nguyên liệu bếp Nhật.

## Vai trò của Codex

Codex hỗ trợ các việc:

- Tạo và cập nhật file sản phẩm trong `context/products/`.
- Cập nhật catalog trong `context/product-catalog.md`.
- Tạo prompt, template và tài liệu vận hành phục vụ content.
- Kiểm tra format, tính nhất quán và lỗi thiếu thông tin.

## Ranh giới với Claude

Codex không sử dụng:

- `.claude/settings.json`
- `.claude/settings.local.json`
- `.claude/commands/`
- `.claude/skills/`
- `.claude/worktrees/`
- `CLAUDE.md` như instruction bắt buộc

Nếu cần tham khảo một workflow đang có trong `.claude/`, phải viết lại thành instruction riêng trong `.codex/` trước khi dùng như quy tắc Codex.

## Dữ liệu dùng chung

Thư mục dùng chung duy nhất giữa Codex và Claude là `context/`.

Codex được đọc và ghi vào:

- `context/products/[tên sản phẩm]/detail.md`
- `context/product-catalog.md`

Codex được đọc để lấy bối cảnh:

- `context/brand-guideline.md`
- `context/customer-persona.md`
- `context/marketing-channels.md`
- Các file `detail.md` mẫu trong `context/products/`

## Nguyên tắc ngôn ngữ

- Tất cả instruction, tài liệu nội bộ và output content trong project viết bằng tiếng Việt.
- Nếu cần dùng thuật ngữ Nhật, ghi kèm romaji và giải thích ngắn gọn bằng tiếng Việt.
- Nếu thông tin chưa chắc chắn, ghi rõ `(cần xác nhận)`.
