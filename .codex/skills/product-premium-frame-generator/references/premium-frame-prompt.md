# Premium Import Frame Imagegen Prompt

Use this prompt with `$imagegen` for each target image. Replace `[tên_file_ảnh_cần_chỉnh]` with the current target image path or filename.

```text
Bạn sẽ nhận 2 hình ảnh đầu vào:

1. TEMPLATE_FRAME_IMAGE = "templates\images\frame-temp-1.jpg"
Đây là ảnh mẫu khung Premium Import Frame cần dùng làm template tham chiếu.
2. TARGET_IMAGE = "[tên_file_ảnh_cần_chỉnh]"
Đây là ảnh gốc cần được chỉnh sửa theo khung mẫu.

Nhiệm vụ:
Hãy áp dụng toàn bộ hệ khung thương hiệu từ TEMPLATE_FRAME_IMAGE sang TARGET_IMAGE, tạo ra một ảnh đầu ra mới có phong cách đồng bộ với mẫu Premium Import Frame.

Yêu cầu quan trọng:

- Giữ nguyên nội dung chính của TARGET_IMAGE.
- Không thay đổi sản phẩm, thùng hàng, kho hàng, người, bối cảnh hoặc chi tiết thật trong TARGET_IMAGE.
- Chỉ thêm/căn chỉnh các thành phần khung thương hiệu theo TEMPLATE_FRAME_IMAGE.
- Không tự ý biến ảnh thành poster mới, không thêm sản phẩm mới, không đổi bố cục ảnh gốc.

Các thành phần cần sao chép từ TEMPLATE_FRAME_IMAGE:

1. Khung viền premium:
    - Viền ngoài màu xanh navy.
    - Line phụ mảnh màu xanh/cyan hoặc gold nếu có trong mẫu.
    - Giữ độ dày, bo góc, khoảng cách và cảm giác cao cấp tương tự ảnh mẫu.
2. Logo chính:
    - Đặt logo Bình Minh SG đúng chuẩn nguyên khối tại vị trí giống tinh thần của mẫu, ưu tiên góc trên phải.
    - Logo và tên thương hiệu là 1 thể hoàn chỉnh.
    - Không redraw logo, không đổi font, không đổi màu sai, không tách icon và chữ, không bóp méo.
3. Tag góc trên trái:
    - Áp dụng tag/box giống TEMPLATE_FRAME_IMAGE.
    - Nội dung tag: "Hàng Sẵn kho"
    - Giữ phong cách chữ trắng, nền xanh navy, dáng premium như mẫu.
4. Logo chìm lớn ở giữa:
    - Thêm logo Bình Minh SG nguyên khối dạng watermark lớn ở trung tâm ảnh.
    - Watermark phải mờ, xuyên thấu, tinh tế.
    - Không được che mạnh lên sản phẩm/thùng hàng/tem nhãn quan trọng.
    - Mục tiêu là bảo vệ ảnh chống copy nhưng vẫn giữ ảnh sạch và chuyên nghiệp.
5. Pattern watermark nền:
    - Thêm pattern watermark lặp nhẹ bằng logo Bình Minh SG nguyên khối.
    - Pattern phải rất nhẹ, opacity thấp, bố trí thưa.
    - Các watermark nhỏ trong pattern phải cách đều nhau theo cả chiều ngang và chiều dọc, tạo nhịp lặp nhất quán.
    - Không tính logo chìm lớn ở giữa vào quy tắc cách đều này; watermark lớn trung tâm là một thành phần riêng.
    - Ưu tiên hiện ở vùng nền trống, tường, khoảng thở hoặc vùng ít chi tiết.
    - Không làm rối hình ảnh chính.
6. Footer dưới cùng:
    - Tạo footer giống TEMPLATE_FRAME_IMAGE.
    - Footer màu xanh navy, mảnh, sạch, cao cấp.
    - Nội dung footer: "Hải Sản Bình Minh SG | Hải sản nhập khẩu"
    - Giữ style chữ, khoảng cách và cảm giác thiết kế giống ảnh mẫu.

Phong cách tổng thể cần đạt:

- Premium
- Sạch
- Lạnh
- Chuyên nghiệp
- Đồng bộ thương hiệu Bình Minh SG
- Phù hợp ảnh kho lạnh, hải sản nhập khẩu, hàng sẵn kho
- Tone màu chủ đạo: xanh navy, xanh dương, trắng, xám lạnh, có thể có line gold nhẹ nếu template có

Nguyên tắc bảo vệ ảnh:

- Watermark phải đủ nhận diện để chống copy.
- Các watermark nhỏ phải được phân bố cách đều nhau; ngoại lệ duy nhất là watermark lớn ở giữa ảnh.
- Không đặt watermark quá đậm làm mất thẩm mỹ.
- Không che các chi tiết quan trọng cần xem rõ.
- Không thay đổi nội dung thật của TARGET_IMAGE.

Output:
Tạo một hình ảnh mới từ TARGET_IMAGE đã được áp dụng khung, logo, watermark, pattern watermark và footer theo đúng TEMPLATE_FRAME_IMAGE.
```
