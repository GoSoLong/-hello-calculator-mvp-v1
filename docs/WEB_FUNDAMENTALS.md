# Kiến thức HTML, CSS và JavaScript cho Calculator MVP

Tài liệu này giải thích các kiến thức Web được áp dụng trong Hello Calculator MVP v1.1, từ cấu trúc trang đến tương tác và triển khai.

## 1. Kiến trúc tổng quan

Ứng dụng chia trách nhiệm thành ba lớp:

| Lớp | File | Trách nhiệm |
|---|---|---|
| Cấu trúc và nội dung | `index.html` | Khai báo màn hình, phím bấm và các vùng hiển thị |
| Trình bày | `css/style.css` | Bố cục, màu sắc, trạng thái giao diện và responsive |
| Hành vi | `js/app.js`, `js/calculator.js` | Nhận thao tác, quản lý phép tính và cập nhật giao diện |

Luồng tương tác chính:

**Thao tác người dùng → Event → CalculatorModel cập nhật State → render() cập nhật DOM**

`calculator.js` không phụ thuộc DOM nên có thể kiểm thử riêng. `app.js` nối model với giao diện và các sự kiện trình duyệt.

## 2. HTML: cấu trúc và ngữ nghĩa

### Khung tài liệu

- `<!doctype html>` bật chế độ tiêu chuẩn của trình duyệt.
- `<html lang="vi">` khai báo ngôn ngữ chính, hữu ích cho công nghệ hỗ trợ và công cụ tìm kiếm.
- `<meta charset="utf-8">` giúp hiển thị tiếng Việt và ký tự toán học đúng.
- Thẻ viewport giúp trang dùng chiều rộng thiết bị thật trên điện thoại.
- `<meta name="description">` mô tả nội dung trang; `<title>` đặt tiêu đề tab trình duyệt.
- CSS và JavaScript được liên kết bằng đường dẫn tương đối. Query `?v=1.1.0` tạo URL asset riêng theo release để tránh trình duyệt giữ file cũ trong cache. `type="module"` cho phép `app.js` dùng `import`/`export` và module được thực thi sau khi HTML được phân tích.

### Ngữ nghĩa và khả năng truy cập

- Dùng `<main>`, `<header>`, `<section>` và `<footer>` để thể hiện cấu trúc nội dung thay vì chỉ dùng các thẻ chung chung.
- Phím được tạo bằng `<button type="button">`: trình duyệt cung cấp sẵn khả năng focus, kích hoạt bằng bàn phím và hành vi phù hợp.
- `data-action` và `data-value` gắn dữ liệu hành động vào phím, ví dụ `data-action="operator"` và `data-value="/"`. Đây là custom data attributes, không phải thuộc tính JavaScript đặc biệt.
- `aria-label` cung cấp tên dễ hiểu cho biểu tượng như `⌫` hoặc `÷`.
- `role="status"`, `aria-live="polite"` và `aria-atomic="true"` giúp trình đọc màn hình nhận biết kết quả mới mà không ngắt lời đột ngột.
- `<details>` và `<summary>` cung cấp panel có thể mở, đóng bằng chức năng HTML tích hợp.

## 3. CSS: bố cục, giao diện và responsive

### Quy tắc nền tảng

- `:root` chứa CSS custom properties như màu, khoảng cách, bán kính góc và bóng đổ. Thay token một lần có thể cập nhật giao diện nhất quán.
- `box-sizing: border-box` giúp kích thước phần tử bao gồm padding và border, dễ dự đoán khi tính bố cục.
- CSS tách hình thức khỏi HTML và hành vi JavaScript; các class như `.is-result`, `.is-error`, `.is-selected` biểu diễn trạng thái nhìn thấy được.

### Grid và kích thước co giãn

- `.keypad` dùng CSS Grid với bốn cột bằng nhau để xếp phím; `grid-column` và `grid-row` cho phép phím `0` và `=` chiếm nhiều ô.
- `minmax(0, 1fr)`, `min-width: 0`, `overflow: hidden` và `text-overflow: ellipsis` giúp nội dung dài không đẩy vỡ bố cục.
- `width: min(...)` giới hạn bề rộng tối đa nhưng vẫn co theo màn hình.
- Media queries tại `560px` và `360px` điều chỉnh padding, kích thước phím và lưới panel cho màn hình nhỏ. Thiết kế hướng đến viewport tối thiểu 320px.
- `font-variant-numeric: tabular-nums` giúp chữ số có độ rộng ổn định, hạn chế màn hình kết quả rung khi giá trị thay đổi.

### Trạng thái tương tác

- `:hover`, `:active` và `:focus-visible` phản hồi thao tác chuột, chạm và điều hướng bằng bàn phím.
- `.utility-row` tạo hàng thao tác `±` và `%` bằng CSS Grid độc lập với keypad bốn cột, giúp mở rộng chức năng mà không phải đổi bố cục phím số.
- Không nên xóa outline focus nếu không thay bằng chỉ báo focus rõ ràng.
- Các class trạng thái được JavaScript bật/tắt, còn CSS quyết định màu sắc và hình thức tương ứng.

## 4. JavaScript: model, state và tương tác

### ES modules và phân chia trách nhiệm

- `calculator.js` export `CalculatorModel` và `OPERATOR_LABELS`.
- `app.js` import model, lấy các node DOM, nhận sự kiện và gọi `render()`.
- Đây là cách phân tách model và view ở mức phù hợp cho MVP: logic phép tính kiểm thử được mà không cần dựng trang HTML.

### State của máy tính

`CalculatorModel` giữ những dữ liệu cần để tiếp tục phép tính:

- `currentInput`: chuỗi đang nhập hoặc hiển thị.
- `firstOperand`: toán hạng thứ nhất; `null` khi chưa có.
- `operator`: phép toán đang chờ.
- `waitingForOperand`: cho biết đang chờ nhập toán hạng tiếp theo.
- `justCalculated`, `error`, `completedExpression`: hỗ trợ kết quả, lỗi và biểu thức đã tính.
- `lastEvent`, `lastValue`: dữ liệu cho Learning Panel.

Giữ input dưới dạng chuỗi giúp kiểm soát việc nhập dấu thập phân. Chỉ chuyển sang `Number` lúc cần tính toán. Các hàm `inputNumber`, `inputDecimal`, `chooseOperator`, `performEquals`, `backspace`, `toggleSign`, `percent` và `reset` thay đổi state; `handleAction` định tuyến hành động đến hàm phù hợp. `toggleSign()` đổi dấu toán hạng hiện tại; `percent()` chuyển giá trị hiện tại thành giá trị chia 100, ví dụ `25 → 0.25`.

JavaScript `Number` dùng số dấu phẩy động nhị phân, nên một số phép tính thập phân có thể có sai số biểu diễn. `formatResult()` làm tròn kết quả tối đa 10 chữ số thập phân và từ chối giá trị không hữu hạn. Chia cho 0 được chuyển thành trạng thái `Error` thay vì hiển thị `Infinity`.

### DOM và event

- DOM là biểu diễn tài liệu HTML mà JavaScript có thể truy vấn và cập nhật.
- `querySelector()` lấy các node cần dùng; `textContent` cập nhật nội dung văn bản mà không diễn giải dữ liệu như HTML.
- `classList.toggle()` đồng bộ class giao diện với state model.
- Một click listener đặt trên `#keypad` xử lý các nút con. `event.target.closest('button[data-action]')` xác định phím được bấm. Đây là event delegation: không cần gắn listener riêng cho từng phím.
- `dataset.action` và `dataset.value` đọc dữ liệu `data-*` từ HTML.
- Listener `keydown` ánh xạ phím vật lý sang cùng các action của model, gồm `F9` cho đổi dấu và `Shift+5` cho phần trăm. Nhánh nhận `Shift+5` được kiểm tra trước nhánh số để ký tự `5` không bị nhập nhầm.

`render()` đọc state hiện tại rồi cập nhật màn hình, biểu thức, trạng thái phím phép toán và Learning Panel. Sau mỗi action, việc gọi render tạo chu trình rõ ràng: **Event → State → Render → DOM**.

## 5. Kiểm thử và triển khai

- Unit test `tests/model_test.mjs` kiểm tra logic model trực tiếp bằng Node.js, không cần trình duyệt hoặc package bên ngoài; phiên bản 1.1 có 18 ca model test.
- `docs/TEST_CASES.md` bao gồm phép tính, dấu thập phân, đổi dấu, phần trăm, chia cho 0, xóa, bàn phím, kích thước 320px và focus bàn phím.
- Khi kiểm tra giao diện, xác nhận không có lỗi trong Console và thử các viewport nhỏ, vừa, lớn; đồng thời kiểm tra cả phím bấm lẫn bàn phím vật lý.
- Đây là ứng dụng static: HTML, CSS và JavaScript chạy trực tiếp ở trình duyệt, không có backend hoặc bước build. Có thể chạy local bằng Live Server hoặc `python3 -m http.server 8080`.
- GitHub Pages có thể publish trực tiếp thư mục gốc vì `index.html` nằm tại root. Đường dẫn tài nguyên tương đối như `css/style.css` và `js/app.js` hoạt động khi repository được phục vụ dưới đường dẫn con.
- Sau khi push commit lên branch đã cấu hình, GitHub Pages tự triển khai lại. Kiểm tra URL HTTPS sau deploy và chạy lại các test quan trọng trên site thật.

## 6. Ý chính cần nhớ

**HTML định nghĩa nội dung và ngữ nghĩa. CSS tạo bố cục, khả năng co giãn và trạng thái trực quan. JavaScript nhận event, cập nhật state và đồng bộ DOM.** Khi ba phần có trách nhiệm rõ ràng, calculator dễ hiểu, kiểm thử và triển khai như một ứng dụng static.

## 7. Cách trình bày các cải tiến v1.1

| Kiến thức | Ánh xạ trong phiên bản 1.1 |
|---|---|
| HTML | Hai `<button>` mang `data-action="sign"` và `data-action="percent"`; `aria-label` mô tả tác vụ cho trình đọc màn hình. URL CSS/JS có query version để tránh cache asset cũ. |
| CSS | `.utility-row` dùng Grid hai cột; bỏ `min-width` cứng trên `body` để viewport 320px không bị cuộn ngang. |
| JavaScript | Event delegation nhận nút trong `.calculator-card`; `keydown` ánh xạ `F9` và `Shift+5`; `handleAction()` chuyển action tới `CalculatorModel`; ES module import model với cùng version cache. |
| State/model | `toggleSign()` hỗ trợ cả số đang nhập lẫn toán hạng kế tiếp; `percent()` chia giá trị hiện tại cho 100; nhập chữ số sau `-0` tạo số âm đúng. |
| Kiểm thử | Node kiểm tra model; browser kiểm tra click, phím tắt, lỗi Console và nhiều viewport. |

Kịch bản trình bày ngắn: nhấn `5`, `+`, `±`, `6`, `=` để thấy model tạo toán hạng `-6` và kết quả `-1`. Sau đó nhấn `C`, nhập `25`, nhấn `%` để thấy quy tắc phần trăm `25 / 100 = 0.25`. Với mỗi thao tác, chỉ ra `data-action` ở HTML, sự kiện trong `app.js`, thay đổi state trong `calculator.js`, và kết quả render lên DOM bằng `textContent`.