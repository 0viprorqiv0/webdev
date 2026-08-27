# Exercise 1.2 - Hướng dẫn học và lời giải

## 1. Review nhanh tài liệu

- Tài liệu dài 14 trang, dự kiến 4 giờ, chia thành 4 phần: dự đoán kết quả HTML/CSS, sửa lỗi, dùng DevTools và viết CSS mà không sửa HTML.
- Trọng tâm thật sự là giải thích được `symptom -> cause -> fix`, không phải chép một file CSS chạy được.
- Part 1 và Part 2 nên làm Stage A trước khi xem phần đáp án dưới đây. Sau đó chạy code và sửa đáp án bằng màu khác như tài liệu yêu cầu.
- Điểm chưa nhất quán: Task 5 của Part 3 yêu cầu rõ 3 ảnh (background trước/sau và body monospace), nhưng mục nộp bài ghi 4 ảnh. Cách an toàn là chụp thêm một ảnh sau khi đổi `display: flex` thành `block`, hoặc hỏi giảng viên.

## 2. Lộ trình học đề xuất

1. Ôn block/inline và CSS cascade trong 25 phút.
2. Tự làm Part 1 trên giấy trong 20 phút, rồi mới đối chiếu đáp án.
3. Với Part 2, mỗi lỗi phải mô tả biểu hiện nhìn thấy trước khi nói nguyên nhân.
4. Part 3 phải làm trực tiếp bằng F12 vì số liệu của trang USTH có thể thay đổi.
5. Part 4: viết từng task T1-T13, kiểm tra ngay sau mỗi task, rồi tự nói thành tiếng câu trả lời “Why”.

Mẹo nhớ:

- Cascade: importance -> nguồn/inline -> specificity -> thứ tự xuất hiện.
- Specificity đơn giản: inline > `#id` > `.class`/`:hover` > tên thẻ.
- Box model mặc định: `total = content + padding + border + margin`.
- Flexbox: đặt `display: flex` lên phần tử cha; các con trực tiếp mới là flex items.
- DevTools Styles cho biết cuộc cạnh tranh; Computed cho biết kết quả cuối cùng.

## 3. Part 1 - Đáp án

### Q1 - Block và inline

Trình duyệt vẽ 4 dòng:

1. `Pho bo`
2. `Pho ga`
3. `Bun cha Banh mi`
4. `Com tam`

`div` và `p` mặc định là block nên bắt đầu dòng mới và chiếm chiều ngang khả dụng. `span` là inline nên hai span nằm cùng dòng; ký tự xuống dòng trong mã nguồn được rút gọn thành một khoảng trắng. `p` còn có margin mặc định nên khoảng cách dọc có thể lớn hơn.

### Q2 - Specificity

- Có `#first`: màu đỏ, vì selector id mạnh hơn selector class và selector thẻ.
- Xóa `#first`: màu xanh lá, vì `.note` và `.highlight` ngang specificity, nhưng `.highlight` nằm sau.
- Đổi thứ tự để `.note` nằm sau `.highlight`: màu xanh dương.
- Hai class gắn trên element không tự cộng lại thành một selector. Ngay cả selector kết hợp `.note.highlight` có specificity `(0,2,0)` vẫn thua `#first` `(1,0,0)`.

### Q3 - Box model mặc định (`content-box`)

| Thành phần            | Số px theo chiều ngang |
| --------------------- | ---------------------: |
| Content               |                    300 |
| Padding trái + phải   |                     30 |
| Border trái + phải    |                     10 |
| Hộp trình duyệt vẽ    |                    340 |
| Margin trái + phải    |                     20 |
| Tổng không gian ngang |                    360 |

Công thức: `300 + 15*2 + 5*2 + 10*2 = 360px`.

Background nhìn thấy ở content và padding; border có màu/kiểu riêng; margin luôn trong suốt. Chính xác hơn, background mặc định có thể được vẽ bên dưới border, nhưng border đen đặc che phần đó.

### Q4 - `border-box`

- Hộp được vẽ: `300px`.
- Content: `300 - 30 padding - 10 border = 260px`.
- Với `border-box`, `width` bao gồm content + padding + border; margin vẫn nằm ngoài.

### Q5 - Inline style

- Giá hiển thị màu đen vì `style="color: black"` là inline style.
- Khi không được sửa HTML: `.price { color: red !important; }`.
- Không nên lạm dụng `!important`: nó phá luồng cascade bình thường, làm CSS khó ghi đè, khó tái sử dụng và khó bảo trì. Trong dự án thật nên sửa/bỏ inline style.

## 4. Part 2 - Chín lỗi

|   # |   Dòng | Biểu hiện                                                               | Nguyên nhân                                                                                          | Cách sửa                                       |
| --: | -----: | ----------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
|   1 |      7 | Tiêu đề không đỏ, nhưng vẫn căn giữa                                    | `#b0000` chỉ có 5 chữ số hex nên `color` bị bỏ                                                       | `#b00000`                                      |
|   2 |      8 | “Since 1979” không nghiêng, không xám                                   | `title` chọn thẻ `<title>` chứ không chọn class                                                      | `.title`                                       |
|   3 |   9-10 | Giá không đậm và cũng không đỏ                                          | Thiếu `;` sau `bold`; parser đọc cả `bold color: #b00000` như một giá trị `font-weight` không hợp lệ | Thêm `;`: `font-weight: bold; color: #b00000;` |
|   4 | sau 11 | Bảng có đường viền kép                                                  | Thiếu quy tắc gộp border                                                                             | `table { border-collapse: collapse; }`         |
|   5 |     16 | Cấu trúc heading/DOM bị browser tự sửa, phạm vi của `h1` có thể lan sai | Mở `<h1>` nhưng đóng `</h2>`                                                                         | Đóng bằng `</h1>`                              |
|   6 |     24 | “Pho tai” nằm ngoài danh sách/bullet không cùng nhóm                    | `<li>` đứng ngoài `<ul>`                                                                             | Chuyển dòng 24 lên trước `</ul>`               |
|   7 |     28 | Header rộng 3 cột nhưng dữ liệu chỉ có 2, tạo một cột thừa              | `colspan="3"` sai                                                                                    | Đổi thành `colspan="2"`                        |
|   8 |     34 | “Open 6am...” cũng bấm như liên kết                                     | Thiếu `</a>`, nên anchor tiếp tục bao phần văn bản sau                                               | Thêm `</a>` ngay sau “Visit our website”       |
|   9 |     38 | Khi ảnh lỗi/đọc bằng screen reader không có mô tả thay thế              | Thiếu thuộc tính `alt`                                                                               | Thêm `alt="A bowl of Pho Thin"`                |

Ba câu hỏi cuối Part 2:

1. Hai giá trị CSS bị loại là `#b0000` và toàn bộ giá trị `bold color: #b00000` của `font-weight`. CSS phục hồi lỗi theo từng declaration: declaration không parse được sẽ bị bỏ, phần còn lại vẫn chạy. Ở lỗi thứ hai, `color` không được coi là declaration riêng vì thiếu `;`, nên mất cả đậm lẫn đỏ.
2. Thiếu `</a>` ở dòng 34. HTML parser giữ anchor mở cho đến khi gặp điểm có thể đóng/sửa cấu trúc, nên văn bản ở dòng sau cũng thuộc link.
3. Ảnh thiếu `alt`; người dùng screen reader và người không tải được ảnh bị mất nội dung thay thế.

So sánh trực tiếp `broken.html` và `broken-fixed.html` trong thư mục này để quan sát triệu chứng và bản sửa.

## 5. Part 3 - Cách làm đúng bằng DevTools

Các câu về tag, class, id, font, màu, kích thước và số rule phải đo trên trang USTH ở thời điểm nộp bài; không nên dùng số liệu cố định từ một lời giải cũ.

### Task 1-4

1. Mở `https://usth.edu.vn`, nhấn `Ctrl+Shift+C`, rồi chọn heading chính.
2. Elements: ghi đúng tên tag, toàn bộ `class`, và `id` nếu có; không có thì ghi “none”.
3. Styles: tìm declaration `font-family`; Computed: tìm `font-family` và ghi font đầu tiên thật sự tải/được dùng. Danh sách khai báo có nhiều fallback, còn computed/Rendered Fonts phản ánh font trình duyệt chọn được.
4. Trong Computed, chép màu `rgb(r, g, b)`. Đổi sang hex bằng cách đổi riêng r, g, b sang hai chữ số hex rồi ghép `#RRGGBB`.
5. Box model: `total horizontal = margin-left + border-left + padding-left + content width + padding-right + border-right + margin-right`.
6. Styles: rule bị gạch là rule thua hoặc declaration sai. So specificity trước; nếu specificity bằng nhau thì rule nằm sau thắng.

### Task 5 - Kết quả cần mô tả

- Bỏ chọn `background-color`: nền của element biến mất/để lộ nền phía sau; DOM và file trên server không đổi.
- Đổi `display: flex` thành `block`: các flex item không còn được bố trí theo flex row/gap/alignment; block children thường xếp theo luồng dọc và chiếm chiều ngang khả dụng.
- Đặt `body { font-family: monospace; }`: chữ dùng font đơn cách trừ nơi có rule cụ thể mạnh hơn ghi đè.

Để đủ 4 ảnh theo phần hand-in: chụp background trước, background sau, flex sau khi đổi sang block, và body monospace.

### Task 6 - Đáp án

- Nhấn F5: các thay đổi DevTools mất.
- Máy của bạn học không thấy thay đổi.
- Các chỉnh sửa chỉ tồn tại trong DOM/CSS đang nằm trong bộ nhớ của tab trình duyệt phía client. Không có request cập nhật nào gửi về server, nên server không biết và không lưu chúng.

### Task 7 - Đáp án

- `View Source` là HTML response ban đầu server gửi.
- `Elements` là DOM hiện tại sau khi browser parse HTML, tự sửa markup không hợp lệ và JavaScript có thể thêm/xóa/sửa node.
- Một ví dụ hợp lệ là node menu/banner/cookie được JavaScript chèn vào Elements nhưng không có trong View Source. Hãy ghi đúng node bạn thật sự tìm thấy.

## 6. Part 4 - Vì sao từng nhóm CSS hoạt động

Code hoàn chỉnh nằm trong `ict-department.html` và `style.css`.

- T1: `color` tô foreground/text; `background-color` tô nền. `padding` tạo khoảng trống bên trong nền; `margin` tạo khoảng trống trong suốt bên ngoài.
- T2: `.main-nav` là flex container vì năm link là con trực tiếp cần xếp hàng. Nếu không có `gap`, có thể dùng `margin-right: 20px` cho các link trừ link cuối. `:hover` là pseudo-class biểu diễn trạng thái con trỏ đang ở trên element.
- T3: link hiện tại nhận cả `.nav-link` và `.current`. Hai selector một class ngang specificity, nên `.current` đặt sau thắng. Nếu đặt trước, màu của `.nav-link` nằm sau sẽ ghi đè.
- T4: `.layout` là flex container; `.sidebar` và `.content` là flex items. `flex: 0 0 220px` vừa đặt basis 220px vừa cấm co. Chỉ `width: 220px` vẫn có thể co vì `flex-shrink` mặc định là 1.
- T5: marker/bullet do list item sinh ra theo ngữ cảnh danh sách; `list-style: none` đặt ở `<ul>` được kế thừa/áp cho các item của danh sách.
- T6: `.side-note { color: ... }` thường thua inline style. `!important` buộc declaration external thắng inline declaration không-important. Đây chỉ là lời giải cho ràng buộc đề bài; dự án thật nên bỏ inline style.
- T7: `#about .lead` chỉ chọn phần tử có class `lead` bên trong `#about`. `#about p` sẽ chọn cả ba paragraph.
- T8: `33% * 3 + padding + border + 2 gaps` vượt chiều rộng hàng khi dùng `content-box`. Hai cách tránh: dùng `box-sizing: border-box` cùng chiều rộng đã tính `calc((100% - 40px)/3)`, hoặc dùng flex `flex: 1` không đặt `width`. Lời giải chọn `flex: 1` vì đúng mục tiêu “chia phần còn lại đều nhau”.
- T9: `span` inline không tạo một hộp độc lập theo chiều dọc; padding trên/dưới có thể được vẽ nhưng không điều khiển line box như mong đợi. `display: inline-block` cho badge một hộp có kích thước riêng.
- T10: `border-collapse: collapse` trên table gộp đường viền đôi. `empty-cells: show` diễn đạt rõ việc vẫn vẽ ô rỗng; trong collapsed-border model, border của lưới vẫn được giải quyết và hiển thị.
- T11: `.credits` áp cho mọi bảng credits cùng class; `#ects-table` chỉ áp cho element có id duy nhất. Trong dự án thật thường ưu tiên class để tái sử dụng, nhưng yêu cầu “chỉ bảng này” khiến id hợp lý.
- T12: `width` không điều khiển chiều rộng một inline `span`; `display: inline-block` làm `70px` có hiệu lực mà ngày và link vẫn ở cùng dòng.
- T13: `footer` là block-level element nên `width: auto` mặc định lấp đầy chiều ngang khả dụng.

## 7. Cách tự kiểm tra trước khi nộp

- Thu nhỏ/mở rộng cửa sổ và kiểm tra sidebar không co ở desktop, ba card vẫn đều nhau.
- Hover nav, sidebar links, news links và từng dòng credits.
- Trong Computed, xác nhận `.side-note` là `rgb(176, 0, 0)` và nguồn thắng có `!important`.
- Bỏ tạm `box-sizing: border-box` hoặc `flex: 1` để tự quan sát card tràn/rớt dòng.
- Tự giải thích ngẫu nhiên hai mục “Why” mà không nhìn tài liệu; nếu chưa nói được trong 30 giây, quay lại quan sát Styles/Computed.
