# Hướng Dẫn Sử Dụng Codex

Thư mục `.codex/` chứa toàn bộ setup riêng cho Codex trong project này. Codex không dùng chung setup, command, settings hay skill từ `.claude/`.

## Nguyên tắc tách biệt

- `.codex/` là không gian cấu hình riêng của Codex.
- `.claude/` là không gian cấu hình riêng của Claude.
- Hai bên chỉ dùng chung dữ liệu nghiệp vụ trong `context/`.
- Không copy setting từ `.claude/` sang `.codex/` nếu chưa được viết lại cho Codex.
- Không dùng `.claude/skills/` làm instruction vận hành cho Codex.

## Thư mục dùng chung

- `context/brand-guideline.md`: thông tin thương hiệu.
- `context/customer-persona.md`: chân dung khách hàng.
- `context/marketing-channels.md`: kênh truyền thông.
- `context/product-catalog.md`: catalog sản phẩm tổng quan.
- `context/products/`: dữ liệu chi tiết từng sản phẩm.

## Thư mục riêng của Codex

- `.codex/instructions/`: quy tắc project, workflow và content.
- `.codex/skills/`: skill riêng của Codex.
- `.codex/prompts/`: prompt tái sử dụng cho các tác vụ Codex.
- `.codex/config.json`: mô tả ranh giới sử dụng của Codex trong project.

## Skill hiện có

- `.codex/skills/product-detail-generator/SKILL.md`: tạo `detail.md` và cập nhật `context/product-catalog.md`.
- `.codex/skills/json-prompt-generator/SKILL.md`: tạo JSON prompt banner từ `templates/product-post/final-template-1.json`.
- `.codex/skills/post-1a-writer/SKILL.md`: viết bài Facebook Dạng 1A, tạo bản short, copy ảnh và tạo JSON prompt banner.

## Task prompt

- `.codex/prompts/task/tao-detail-san-pham.md`
- `.codex/prompts/task/tao-json-prompt-template-1.md`
- `.codex/prompts/task/viet-bai-facebook-1a.md`

## Workflow chính

Khi user yêu cầu tạo detail sản phẩm mới, dùng skill:

```text
.codex/skills/product-detail-generator/SKILL.md
```

Khi user yêu cầu tạo JSON prompt banner, dùng skill:

```text
.codex/skills/json-prompt-generator/SKILL.md
```

Khi user yêu cầu viết bài Facebook Dạng 1A, dùng skill:

```text
.codex/skills/post-1a-writer/SKILL.md
```
