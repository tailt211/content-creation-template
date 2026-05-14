# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A content creation template system for **Hải Sản Bình Minh Sài Gòn (Bình Minh SG)** — a B2B frozen seafood and Japanese food supplier targeting Japanese restaurants in TP.HCM and Hà Nội. The repo contains structured context files used to prompt AI-generated marketing content (Facebook posts, Zalo messages, catalog copy).

There is no code to build, lint, or test. All work here is editing Markdown context files and producing written content based on them.

## Context Files — Luôn đọc trước khi tạo content

Trước khi viết bất kỳ bài content nào, đọc các file sau:

| File | Vai trò |
|---|---|
| `context/brand-guideline.md` | Tone of voice, màu sắc, phong cách hình ảnh, những điều không được làm |
| `context/customer-persona.md` | 4 persona (chủ quán / bếp trưởng / F&B manager / người mua lẻ) — xác định đối tượng trước khi chọn format bài |
| `context/marketing-channels.md` | Cấu trúc bài Facebook (Dạng 1A–5), tone theo kênh, hashtag, lịch content, CTA chuẩn |
| `context/product-catalog.md` | Danh mục 9 sản phẩm, USP tổng quan, link đến detail.md từng sản phẩm |

Với từng sản phẩm cụ thể, đọc thêm `context/products/[tên sản phẩm]/detail.md` để lấy thông số đầy đủ, ứng dụng bếp và danh sách ảnh.

---

## Detailed Rules

Detailed rules are in `.claude/rules/`:

| Rule file | Covers |
|---|---|
| `context-file-architecture.md` | File layout in `context/`, when to edit each file, how to add new products |
| `tone-and-language.md` | Voice, forbidden phrases, icon usage, CTA style |
| `facebook-post-formats.md` | Dạng 1A–5 structures, CTA block, hashtag strategy, content calendar |
| `products-and-personas.md` | Quick reference for all 6 products, 4 customer personas, content workflow |
