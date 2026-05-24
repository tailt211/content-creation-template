---
name: post-1a-writer
description: |
  Viết bài Facebook Dạng 1A: Product Post — B2B cho một sản phẩm cụ thể của Bình Minh SG.
  Research blog ẩm thực Nhật → viết bài chuẩn cấu trúc Dạng 1A → lưu file → tạo bản tối giản
  (post-facebook-1a-short.txt) → copy ảnh đề xuất → tạo JSON prompt banner.

  Dùng skill này khi user:
  - Nói "viết bài 1A cho [sản phẩm]"
  - Nói "viết bài Facebook B2B cho [sản phẩm]"
  - Nói "viết post product B2B cho [sản phẩm]"
  - Nói "tạo bài Dạng 1A cho [sản phẩm]"
---

# Post 1A Writer — Dạng 1A: Product Post B2B

Skill này viết bài Facebook Dạng 1A chuẩn cho một sản phẩm, dựa trên cấu trúc định nghĩa trong
`context/marketing-channels.md`, tone voice từ `context/brand-guideline.md`, và dữ liệu sản phẩm
từ `context/products/[tên]/detail.md`.

## Input

- **Bắt buộc:** Tên sản phẩm (ví dụ: `"Lươn vàng"`)
- **Tự động:** Đọc `context/products/[tên sản phẩm]/detail.md`
- **Tự động:** Đọc `context/brand-guideline.md` và `context/marketing-channels.md`
- Nếu không tìm thấy folder khớp → thông báo và liệt kê folder có trong `context/products/`

## Output

1. File `posts/[tên sản phẩm]/post-facebook-1a.txt` — bài viết hoàn chỉnh
2. File `posts/[tên sản phẩm]/post-facebook-1a-short.txt` — bản tối giản tự động tạo sau bài gốc
3. Ảnh đề xuất được copy vào `posts/[tên sản phẩm]/data/` với tên `[tên-sp-không-dấu]-[số thứ tự].[ext]`
4. JSON prompt banner được tạo bởi skill `json-prompt-generator` (tích hợp ở Phase 6)

**Quy tắc định dạng file — bắt buộc:**
- Toàn bộ nội dung lưu dưới dạng **plain text** — không dùng markdown bold (`**text**`), không dùng heading markdown (`#`, `##`) trong nội dung bài viết
- Lý do: file được dùng để copy-paste trực tiếp lên Facebook. Markdown không render trên Facebook và làm mất ký tự `**` trong caption
- Các label trong bullet đặc điểm viết plain: `🌏 Xuất xứ: ...` thay vì `🌏 **Xuất xứ:** ...`
- Tiêu đề section (Đặc điểm sản phẩm, Tại sao chọn...) viết plain text, không có `##` hay `**`

---

## Workflow

### Phase 1: Đọc dữ liệu

**Bước 1 — Tìm folder sản phẩm:**
- Glob `context/products/*/` để tìm folder khớp với tên sản phẩm user nhập (so khớp mềm, không phân biệt hoa thường)
- Nếu không tìm thấy → thông báo và dừng

**Bước 2 — Đọc context song song:**
- Đọc `context/products/[tên]/detail.md` — lấy: tên sản phẩm, thương hiệu, xuất xứ, trọng lượng/quy cách, glazing, bảo quản, trạng thái, chứng nhận, ứng dụng bếp Nhật, danh sách ảnh đề xuất
- Đọc `context/brand-guideline.md` — nắm tone voice, từ cấm, phong cách viết
- Đọc `context/marketing-channels.md` — nắm đầy đủ quy tắc Dạng 1A

**Bước 3 — Xác định ảnh đề xuất:**
- Từ `detail.md`, đọc danh sách ảnh đề xuất cho bài đăng (nếu có)
- Nếu không có danh sách → scan ảnh trong `context/products/[tên]/` và chọn 3–5 ảnh phù hợp nhất

---

### Phase 2: Research blog ẩm thực

**Bắt buộc thực hiện — không được bỏ qua.**

- Dùng đúng 1 query duy nhất: `[tên sản phẩm] blog review ẩm thực Nhật`
  - Không search thêm query nào khác
  - Không thêm từ mô tả vào truy vấn
- Từ kết quả trang 1, chọn tối đa 1 bài mỗi loại trang theo thứ tự ưu tiên:
  **page nhà hàng review → food review → page bán hàng → post Facebook → blog cá nhân**
- Dùng WebFetch đọc bài ưu tiên cao nhất tìm được
  - **Prompt WebFetch phải yêu cầu trích nguyên văn phần mở đầu bài** — là những đoạn văn giới thiệu xuất hiện trước khi bài đi vào các mục/tiêu đề nội dung chính. Không tóm tắt, không phân loại, không bỏ đoạn nào, lấy đúng thứ tự từ đầu bài.
- Sau khi có nguyên văn: tham khảo **cách đoạn mở đầu được viết** — nhịp câu, cảm xúc, góc tiếp cận — để dùng làm cảm hứng cho tiêu đề và đoạn mở bài. Không chắt lọc insight, không tóm tắt — học cách mở đầu để viết đúng như một đoạn mở đầu bài.
- **Tiêu đề và đoạn mở không được lấy nội dung từ `detail.md`** — chỉ dùng kết quả research. Không tự biến tấu theo ý mình — viết theo đúng phong cách mở đầu của bài research tìm được.
- Không tự nghĩ góc khác ngoài những gì research cho thấy
- Không sao chép nguyên vẹn, không sai lệch thông tin sản phẩm

---

### Phase 3: Viết bài

**Quy tắc phong cách — bắt buộc áp dụng trước khi viết từng câu:**
Quy tắc đầy đủ định nghĩa trong `context/marketing-channels.md` mục **Writing Style — Anti-AI Detection**. Tóm tắt bắt buộc:

- **Câu dài xen câu ngắn.** Không để toàn bài có nhịp điệu đều đều. Câu mô tả kỹ thuật có thể dài; câu chuyển cảnh hoặc kết ý nên ngắn.
- **Không mở đầu câu/đoạn theo cùng một kiểu.** Luân phiên: câu bắt đầu bằng chủ ngữ, câu bắt đầu bằng trạng ngữ, câu bắt đầu bằng điều kiện, câu đảo cấu trúc.
- **Không dùng em dash (—) để nối ý** — dùng dấu phẩy, dấu chấm, hoặc viết lại câu.
- **Tránh từ AI hay dùng:** không viết "không chỉ... mà còn", "đây là lý do tại sao", "điều này cho thấy", "quan trọng hơn", "đặc biệt là", "thực sự", "chính xác là", "rõ ràng là", "không nghi ngờ gì", "đó chính là", "vô cùng".
- **Không lặp cấu trúc ngữ pháp liên tiếp.** Hai câu liền nhau không được có cùng số mệnh đề, cùng loại động từ hoặc cùng vị trí bổ ngữ.
- **Viết có chiều sâu, không chỉ liệt kê.** Một câu tốt giải thích *tại sao* điều đó quan trọng với bếp nhà hàng, không chỉ nêu ra thực tế.
- **Không dùng tính từ đứng một mình** ("chất lượng cao", "tươi ngon", "đặc biệt") — luôn đi kèm số liệu, quy cách, hoặc ngữ cảnh cụ thể.
- **Kết hợp cả câu chủ động và bị động** nơi phù hợp để tạo đa dạng nhịp điệu.
- **Không có đoạn nào toàn câu ngắn** và không có đoạn nào toàn câu dài — trộn đều trong cùng một đoạn.

Viết bài theo đúng cấu trúc Dạng 1A sau:

#### [1] TIÊU ĐỀ
- Từ điểm nổi bật trong đoạn mở đầu bài nghiên cứu → đặt tiêu đề ngắn, gây tò mò hoặc nêu điểm cụ thể
- **Không lấy nội dung từ `detail.md`** để đặt tiêu đề — chỉ dùng kết quả research. Không tự biến tấu theo ý mình.
- Không dùng tiêu đề hoa mỹ chung chung, không có bằng chứng

#### [2] ĐOẠN MỞ
- Tham khảo **cách bài blog mở đầu** — nhịp câu, cảm xúc, góc tiếp cận — để viết đoạn mở theo phong cách mở đầu tương tự. Không phân tích hay liệt kê insight — viết đúng như một đoạn mở đầu bài.
- **Không lấy nội dung từ `detail.md`** để viết đoạn mở — chỉ dùng kết quả research. Không tự biến tấu theo ý mình — viết theo phong cách của bài research, không phải theo hiểu biết về sản phẩm.
- Không nhắm persona, không có góc chiến lược
- **Bắt buộc kết thúc đoạn mở bằng 1 câu nối vào sản phẩm Bình Minh SG** — giọng PR tự nhiên, không giả tạo

#### [3] PHẦN ĐẶC ĐIỂM SẢN PHẨM
- Hiển thị tiêu đề: `Đặc điểm sản phẩm`
- Chọn 4–5 đặc điểm phù hợp nhất với sản phẩm — không liệt kê tất cả mọi thứ
- Bỏ điểm trùng ý hoặc điểm bếp tự nhìn/biết được
- Format mỗi dòng: `icon Label: nội dung` — **plain text, không dùng markdown bold (`**Label:**`)**
- Lý do: file lưu để copy-paste lên Facebook, markdown không render và làm lộ ký tự `**`
- Các dòng icon liệt kê liền nhau — không có dòng trắng giữa các dòng trong cùng danh sách. Chỉ để dòng trắng phân cách giữa phần "Đặc điểm sản phẩm" và phần trên/dưới nó.

Các đặc điểm hay dùng (chọn phù hợp, có thể thêm khác):
- 🌏 Xuất xứ: [quốc gia / nhà sản xuất]
- 📦 Quy cách: [dạng sản phẩm | size/con | đơn vị thùng]
- ⚖️ Glazing: [% hoặc mô tả ngắn]
- 🌡️ Bảo quản: [nhiệt độ đông lạnh]
- 🔪 Trạng thái: [đã sơ chế / nguyên con / fillet / v.v.]

**Quy tắc bắt buộc:**
- Tính từ phải đi kèm số liệu hoặc bằng chứng cụ thể
- KHÔNG có phần ứng dụng chế biến — bếp trưởng tự biết nấu gì

#### [4] PHẦN "TẠI SAO CHỌN [TÊN SẢN PHẨM] TẠI BÌNH MINH SG?"
- 4 dòng dạng `✅ nội dung`
- Các dòng ✅ liệt kê liền nhau — không có dòng trắng giữa các dòng trong cùng danh sách. Chỉ để dòng trắng phân cách giữa phần này và phần trên/dưới nó.
- Mỗi dòng 1 lợi ích cốt lõi, viết gắn — không dài dòng giải thích
- Lợi ích phải xuất phát từ đặc điểm nổi bật nhất của sản phẩm đó — không áp công thức giống nhau cho mọi sản phẩm
- Không lặp thông số đã liệt kê ở phần đặc điểm

#### [5] CTA + LIÊN HỆ
```
📦 Đặt hàng & báo giá sỉ:
📱 Zalo/Hotline: [SỐ ĐIỆN THOẠI]
📍 TP.HCM | Giao hàng toàn quốc
```
- CTA nhẹ nhàng — mời thử, mời hỏi, không ép mua

#### [6] HASHTAG
- **Research hot search trước:** Dùng WebSearch tìm từ khóa hot search liên quan đến sản phẩm (ví dụ: "[tên sản phẩm] giá sỉ", "[tên sản phẩm] nhập khẩu") và từ khóa mua bán hải sản Nhật tại VN. Chọn 2–3 hashtag phù hợp từ kết quả.
- 6–10 hashtag tổng cộng
- Luôn có: `#BinhMinhSG` `#HaiSanBinhMinhSaiGon` `#HaiSanNhapKhau`
- Thêm 2–3 hashtag theo sản phẩm (lấy từ danh sách trong `marketing-channels.md`)
- Thêm 2–3 hashtag ngành F&B phù hợp: `#NhaHangNhat` `#AmThucNhat` `#BepNhat` v.v.
- Bổ sung 1–2 hashtag từ kết quả research hot search ở trên

---

### Phase 4: Lưu bài + copy ảnh

**Sau khi hiển thị bài viết hoàn chỉnh → chờ user confirm trước khi lưu.**

1. Kiểm tra folder `posts/[tên sản phẩm]/data/` — tạo nếu chưa có
2. Lưu file `posts/[tên sản phẩm]/post-facebook-1a.txt`
3. Copy từng ảnh đề xuất vào `posts/[tên sản phẩm]/data/`:
   - Đổi tên theo format: `[tên-sp-không-dấu]-[số thứ tự].[ext]`
   - Ví dụ: `luon-vang-1.jpg`, `luon-vang-2.jpg`
   - Số thứ tự bắt đầu từ `1`, tăng dần
4. Thông báo danh sách file đã lưu (bài viết + ảnh)

**Quy tắc chuyển tên không dấu:**
- Loại bỏ dấu tiếng Việt (à→a, ă→a, â→a, đ→d, ê→e, ô→o, ơ→o, ư→u, v.v.)
- Thay khoảng trắng bằng dấu gạch ngang `-`
- Viết thường toàn bộ

---

### Phase 5: Tạo bản tối giản

**Tự động thực hiện ngay sau khi lưu bài gốc — không hỏi user.**

Tạo file `posts/[tên sản phẩm]/post-facebook-1a-short.txt` theo cấu trúc Dạng 1A Short (xem định nghĩa trong `marketing-channels.md`):

1. **Tiêu đề / câu hook** — giữ nguyên câu mở đầu từ bài gốc (không phải heading markdown)
2. **Đoạn mở** — 2–3 câu: rút gọn từ mở bài gốc, giữ ý chính + câu nối Bình Minh SG
3. **Specs block** — 1 dòng duy nhất gộp thông số quan trọng nhất: `📦 [quy cách] | Bảo quản [nhiệt độ] | [xuất xứ nếu không hiển nhiên]`
4. **Lợi ích** — 2 dòng `✅` thay vì 4, chọn 2 điểm nổi bật nhất
5. **CTA** — giữ nguyên mẫu chuẩn (Zalo/Hotline + địa chỉ)
6. **Hashtag** — 4–5 tag: `#BinhMinhSG` `#HaiSanBinhMinhSaiGon` + 2–3 tag theo sản phẩm

**Không có:** phần "Tại sao chọn..." riêng, không có tiêu đề section markdown, không có "Đặc điểm sản phẩm" header.

Sau khi lưu, thông báo ngắn: *"Đã lưu bản tối giản: posts/[tên]/post-facebook-1a-short.txt"*

---

### Phase 6: Tạo JSON prompt banner

**Sau khi đã lưu bài gốc, bản tối giản và ảnh xong → tự động chạy skill `json-prompt-generator` cho cùng sản phẩm.**

- Thông báo: *"Đang tạo JSON prompt banner cho [tên sản phẩm]..."*
- Gọi skill `json-prompt-generator` với tên sản phẩm đã xác định
- Skill đó sẽ tự xử lý theo workflow của nó (Phase 1–4 trong json-prompt-generator)

---

## Quy Tắc Quan Trọng

- **Bắt buộc research trước khi viết** — không được viết mở bài từ ý tự nghĩ
- **Tiêu đề và đoạn mở KHÔNG lấy nội dung từ `detail.md`** — chỉ dùng kết quả research. Viết theo đúng phong cách mở đầu của bài research, không tự biến tấu.
- **Không để lại placeholder** — số điện thoại phải lấy từ `brand-guideline.md`; nếu không có ghi `(cần xác nhận)`
- **Dữ liệu từ detail.md là nguồn chính cho phần đặc điểm và lý do chọn** — không bịa thông số kỹ thuật; nhưng KHÔNG dùng detail.md cho tiêu đề và đoạn mở
- **Không viết phần ứng dụng chế biến** — đây là Dạng 1A B2B
- **Tính từ phải đi kèm bằng chứng** — không viết "chất lượng cao", "hàng tuyển chọn"
- **Không dùng câu chuyển tiếp giả tạo** giữa thân bài ("Bình Minh SG mang đến đúng điều đó...")
- **Không dùng 3+ emoji liên tiếp** cùng dòng
- Khi không tìm được folder sản phẩm → liệt kê folder hiện có để user chọn lại

---

## Trình Tự Thực Thi

1. Tìm folder sản phẩm trong `context/products/`
2. Đọc `detail.md` + `brand-guideline.md` + `marketing-channels.md` song song
3. Xác định danh sách ảnh đề xuất
4. Research blog ẩm thực: 1 query duy nhất → đọc bài ưu tiên cao nhất → trích nguyên văn mở đầu
5. Tham khảo cách blog mở đầu → viết tiêu đề + mở bài theo phong cách mở đầu tương tự
6. Viết phần đặc điểm, lý do chọn, CTA, hashtag
7. Hiển thị bài viết hoàn chỉnh
8. **Chờ user confirm**
9. Tạo folder `posts/[tên]/data/` nếu cần → lưu `post-facebook-1a.txt`
10. Copy ảnh đề xuất → đổi tên `[tên-sp-không-dấu]-[số thứ tự].[ext]`
11. Thông báo danh sách file đã lưu
12. Tự động tạo và lưu `post-facebook-1a-short.txt` (bản tối giản)
13. Thông báo đã lưu bản tối giản
14. Tự động gọi skill `json-prompt-generator` cho cùng sản phẩm
