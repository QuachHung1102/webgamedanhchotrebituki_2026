# Cú Bông kể chuyện

Web game truyện tương tác cho trẻ tự kỷ (khoảng 4–10 tuổi). Bé xem từng trang truyện
như một visual novel, rồi chú cú Bông hỏi vài câu về truyện (kiểu Akinator). Bé trả lời
bằng cách chạm vào hình.

## Chạy thử

```bash
npm install
npm run dev      # mở http://localhost:3000
npm test         # kiểm tra dữ liệu truyện
npm run build    # bản chạy thật
```

## Giao diện: tranh Đông Hồ in ban đêm

Mỗi trang truyện là một bức tranh in như tranh Đông Hồ: mảng màu phẳng, nét viền đen, chữ đề
in bên cạnh. Nền là giấy dó nhuộm chàm (tối) để bé đỡ chói. Mỗi màu có một việc riêng:

- **Vàng hoè:** nút làm việc chính ("Tiếp", "Đọc truyện"). Chỉ một nút vàng trên mỗi màn.
- **Đỏ son:** con dấu "Đã xong" / "Đúng". Không dùng đỏ cho lỗi.
- **Xanh lá:** đáp án đúng.
- **Kem (điệp):** chữ và khung tranh.

Font tiêu đề là Grenze (nét như khắc gỗ). Chữ để đọc là Andika, thiết kế cho trẻ mới tập đọc.

## Game làm gì để bé dễ tập trung

- **Mỗi màn chỉ một việc.** Nút "Tiếp" luôn ở góc dưới bên phải, cùng cỡ, cùng màu.
- **Không gấp gáp.** Không đếm giờ, không tự lật trang, không có hình chuyển động liên tục.
  Tranh mới hiện ra nhẹ trong 0,3 giây; máy đặt "giảm chuyển động" thì không động gì cả.
- **Sai không bị phạt.** Không màu đỏ, không còi. Lựa chọn sai chỉ còn nét viền mờ, cú Bông cho
  xem lại đúng trang truyện chứa đáp án. Câu hỏi nào cũng kết thúc bằng một câu trả lời đúng.
- **Biết trước việc sắp tới.** Màn giới thiệu và thanh trên cùng dùng cùng một bộ dấu:
  ô trang truyện → ô câu hỏi → con dấu. Trạng thái là hình (ô tô đầy / ô rỗng), không chỉ là màu.
- **Âm thanh nhẹ, tắt được.** Máy tự đọc to từng trang bằng giọng tiếng Việt có sẵn trên
  thiết bị, kèm tiếng chuông nhỏ khi bé trả lời đúng. Tắt từng thứ trong "Cài đặt".
  Thiết bị không có giọng tiếng Việt thì nút "Nghe" tự ẩn.
- **Mở lại truyện luôn bắt đầu từ đầu.** Truyện đã xong được đóng dấu son ở trang chủ
  (lưu trên trình duyệt của máy đó).

## Thêm, sửa, xoá truyện

Mỗi truyện là **một thư mục** trong `public/stories/`:

```
public/stories/
  03-muop-o-dau/
    truyen.json        chữ của truyện và câu hỏi
    trang-01.svg       tranh trang 1
    trang-02.svg       tranh trang 2
    ...
    mat-na-buon.svg    hình cho một đáp án
```

- **Thêm truyện:** chép một thư mục có sẵn, đổi tên thành `11-ten-truyen`, rồi sửa `truyen.json`
  và thay tranh. Số đầu tên thư mục quyết định thứ tự ở trang chủ. Phần sau số là địa chỉ trang:
  `/truyen/ten-truyen` (chữ thường, không dấu, nối bằng dấu gạch).
- **Sửa truyện:** sửa `truyen.json` hoặc thay file tranh (giữ nguyên tên file, hoặc đổi tên
  trong `truyen.json`).
- **Xoá truyện:** xoá cả thư mục.
- **Đổi thứ tự:** đổi số đầu tên thư mục. Tiến độ "Đã xong" của bé vẫn giữ, vì nó theo phần tên sau số.

`truyen.json` trông như sau:

```json
{
  "title": "Đi siêu thị",
  "topic": "Chờ đến lượt khi xếp hàng",
  "group": "Bạn bè",
  "pages": [
    { "image": "trang-01.png", "text": "Bin và mẹ đi siêu thị." },
    { "image": "trang-02.png", "by": "Bin", "text": "Ồ, nhiều người quá!" }
  ],
  "questions": [
    {
      "ask": "Bin đi đâu với mẹ?",
      "choices": [
        { "label": "Siêu thị", "emoji": "🛒" },
        { "label": "Trường học", "image": "truong-hoc.png" }
      ],
      "answer": 1,
      "hint": 1,
      "yes": "Đúng rồi! Bin đi siêu thị với mẹ."
    }
  ]
}
```

- **`topic`:** người lớn đọc để chọn truyện.
- **`group`:** nút lọc ở trang chủ. Thói quen, Cảm xúc, Bạn bè và Thay đổi mỗi nhóm có màu riêng;
  nhóm mới hiện màu kem.
- **`pages`:** mỗi trang một tranh (`image`) và một câu (`text`, tối đa 70 ký tự).
  Có `by` là người đó nói, tên hiện đúng như bạn viết. Không có `by` là cú Bông kể.
  Hai trang có thể dùng chung một file tranh.
- **`questions`:**
  - Mỗi câu có 2 hoặc 3 lựa chọn. Mỗi lựa chọn có đúng một hình: `emoji` hoặc `image`
    (file trong thư mục truyện).
  - `answer` là số thứ tự đáp án đúng, đếm từ 1.
  - `hint` là số trang chứa đáp án, cũng đếm từ 1.
  - `yes` bắt đầu bằng "Đúng rồi!".

**Tranh:**
- **Định dạng:** `.svg`, `.png`, `.jpg`, `.jpeg` hoặc `.webp`.
- **Tranh trang:** khung 16:9, ví dụ 1600×900. Khác tỉ lệ thì bị cắt mép cho vừa khung.
- **Hình đáp án:** vuông, nền trong suốt.
- **Phong cách:** để hợp giao diện, nên vẽ mảng màu phẳng có nét viền đen, dùng các màu
  vàng hoè, đỏ son, xanh lá, xanh chàm, nâu đất, kem.
- **Hình Cú Bông:** nằm ở `public/cu-bong/`.

Khi chạy `npm run dev`, sửa file rồi tải lại trang là thấy ngay. File sai sẽ báo lỗi bằng tiếng Việt,
kèm tên thư mục và từng chỗ sai (thiếu file tranh, đáp án ngoài khoảng, câu quá dài...).
Chạy `npm test` để kiểm tra tất cả truyện mà không cần mở trình duyệt.

## Dùng trên máy tính

Phím `→` sang trang, `←` lùi lại, `1` `2` `3` chọn đáp án. Nút "Cài đặt" (góc trên phải) bật/tắt
tự đọc to, tiếng chuông và chữ to.

## Nguồn hình

- Tranh các trang truyện, khuôn mặt nhân vật, Cú Bông, giấy dó, tre: vẽ riêng cho dự án.
- Hình đáp án đồ vật (bóng, cốc, mèo...): dựa trên [Twemoji](https://github.com/jdecked/twemoji)
  © Twitter và các cộng tác viên, giấy phép [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/),
  đã đổi màu sang bảng màu Đông Hồ và thêm nét viền.
# webgamedanhchotrebituki_2026
