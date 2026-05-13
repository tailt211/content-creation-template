# Context File Architecture

## Tổng quan

Toàn bộ dữ liệu nguồn nằm trong thư mục `context/`. Không có code để build, lint, hay test — công việc ở đây là chỉnh sửa các file Markdown context và tạo content dựa trên chúng.

## Sơ đồ file context

| File | Nội dung | Tần suất thay đổi |
|---|---|---|
| `context/brand-guideline.md` | Nhận diện thương hiệu, tone of voice, màu sắc, typography, phong cách hình ảnh | Hiếm khi thay đổi |
| `context/marketing-channels.md` | Playbook từng kênh — cấu trúc bài Facebook (Dạng 1A–5), quy tắc tone, hashtag, lịch content | Cập nhật khi thêm kênh hoặc format mới |
| `context/customer-persona.md` | 4 persona khách hàng với pain points và hành vi mua hàng | Ổn định |
| `context/product-catalog.md` | Tổng quan 6 sản phẩm với USP và link đến detail file | Cập nhật khi thêm/bỏ sản phẩm |
| `context/products/[sản phẩm]/detail.md` | Thông số đầy đủ, ứng dụng bếp, điểm bán hàng, danh sách ảnh theo từng sản phẩm | Cập nhật khi có thông tin mới |
| `context/products/[sản phẩm]/*.jpg` | Ảnh sản phẩm — tên file được tham chiếu trong detail.md kèm mô tả | Thêm khi có ảnh mới |
| `context/logo/` | Logo thương hiệu | Hiếm khi thay đổi |

## Nguyên tắc khi chỉnh sửa context files

- `brand-guideline.md` — chỉ chứa định nghĩa thương hiệu (tone, màu, typography, hình ảnh). Không chứa cấu trúc bài viết hay ví dụ content — những thứ đó thuộc `marketing-channels.md`
- `marketing-channels.md` — chứa playbook và cấu trúc bài viết theo từng kênh. Không chứa thông tin sản phẩm cụ thể
- `detail.md` của mỗi sản phẩm — chứa thông tin thực tế có thể đưa vào bài viết (thông số, ứng dụng, điểm bán). Không chứa quy tắc viết lách

## Khi thêm sản phẩm mới

1. Tạo thư mục `context/products/[tên sản phẩm]/`
2. Tạo `detail.md` theo cấu trúc của các sản phẩm hiện có (thông tin sản phẩm → quy cách → thông số kỹ thuật → ứng dụng bếp → điểm bán hàng → hình ảnh)
3. Thêm entry vào `context/product-catalog.md`
4. Copy ảnh sản phẩm vào thư mục và cập nhật danh sách ảnh trong detail.md kèm mô tả ngắn
