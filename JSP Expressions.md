# Hướng Dẫn Sử Dụng JSP Expressions

Trong video này, chúng ta sẽ tìm hiểu về cách sử dụng các **JSP Expressions** để nhúng và hiển thị kết quả của mã Java vào trang HTML động. Đây là một thành phần rất quan trọng và cơ bản trong việc xây dựng các ứng dụng web sử dụng JSP. Chúng ta cũng sẽ xem qua các loại thành phần khác trong JSP như **JSP Scriptlets** và **JSP Declarations**, nhưng chủ yếu sẽ tập trung vào **JSP Expressions** trong bài này.

## 1. Các Thành Phần Lập Trình Trong JSP

Trong JSP, có ba loại phần tử lập trình chính:

1. **JSP Expressions**: Là các biểu thức Java đơn giản, cho phép bạn nhúng mã Java vào trang HTML. Kết quả của biểu thức sẽ được hiển thị trực tiếp trên trang HTML.
   
   - Cú pháp: `<%= biểu_thức_java %>`

2. **JSP Scriptlets**: Là đoạn mã Java được chèn vào trang HTML, cho phép bạn viết một hoặc nhiều dòng mã Java để thực hiện các tác vụ phức tạp hơn.

   - Cú pháp: `<% đoạn_mã_java %>`

3. **JSP Declarations**: Cho phép bạn khai báo các biến hoặc phương thức Java mà bạn muốn sử dụng trong trang JSP.

   - Cú pháp: `<%! khai_báo_biến_hoặc_phương_thức %>`

Chúng ta sẽ khám phá từng thành phần trong các video khác nhau, nhưng trong bài này, chúng ta sẽ tập trung vào **JSP Expressions**.

## 2. Tìm Hiểu Về JSP Expressions

### JSP Expression Là Gì?

- JSP Expression cho phép bạn chèn một đoạn mã Java nhỏ vào trong trang HTML và hiển thị kết quả của đoạn mã đó trực tiếp trên trình duyệt.
- Cú pháp của JSP Expression là: `<%= biểu_thức_java %>`.
- Khi máy chủ xử lý trang JSP, biểu thức Java sẽ được tính toán và kết quả sẽ được chèn vào trang HTML trước khi trả về cho trình duyệt.

### Ví Dụ Về JSP Expressions

1. **Sử Dụng Đối Tượng và Chuỗi Ký Tự**: 

   Bạn có thể sử dụng JSP Expression để hiển thị các đối tượng và xử lý chuỗi ký tự. Ví dụ:

   ```jsp
   <%= new String("Hello World").toUpperCase() %>
   ```

   Biểu thức này sẽ chuyển đổi chuỗi `"Hello World"` thành chữ in hoa và hiển thị kết quả là `"HELLO WORLD"`.

2. **Sử Dụng Các Biểu Thức Toán Học**:

   Bạn có thể thực hiện các phép toán cơ bản và hiển thị kết quả:

   ```jsp
   <%= 25 * 4 %>
   ```

   Biểu thức này sẽ tính toán và hiển thị kết quả là `100`.

3. **Sử Dụng Biểu Thức Boolean**:

   Bạn có thể sử dụng các biểu thức logic để hiển thị kết quả đúng (true) hoặc sai (false):

   ```jsp
   <%= 75 < 69 %>
   ```

   Biểu thức này sẽ trả về `false` vì 75 không nhỏ hơn 69.

## 3. Tạo Tệp JSP Để Thử Nghiệm JSP Expressions

Bây giờ chúng ta sẽ thực hiện một ví dụ trong Eclipse để làm quen với cách sử dụng JSP Expressions.

### Bước 1: Tạo Tệp `expression-test.jsp`

1. Mở dự án **JSPDemo** mà bạn đã tạo trong các bài học trước.
2. Nhấp chuột phải vào thư mục **WebContent**, chọn **New** -> **File**.
3. Đặt tên tệp là `expression-test.jsp` và nhấp **Finish**.

### Bước 2: Thêm Mã HTML và JSP Expressions

Dán đoạn mã dưới đây vào tệp `expression-test.jsp`:

```jsp
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JSP Expressions Demo</title>
</head>
<body>
    <h3>Sử dụng JSP Expressions</h3>

    <!-- Ví dụ 1: Chuyển đổi chuỗi thành chữ in hoa -->
    <p>Chuỗi "Hello World" sau khi chuyển đổi thành in hoa: <%= new String("Hello World").toUpperCase() %></p>

    <!-- Ví dụ 2: Thực hiện phép toán -->
    <p>25 nhân với 4 bằng: <%= 25 * 4 %></p>

    <!-- Ví dụ 3: Biểu thức Boolean -->
    <p>75 có nhỏ hơn 69 không? <%= 75 < 69 %></p>
</body>
</html>
```

### Bước 3: Chạy Tệp `expression-test.jsp`

1. Nhấp chuột phải vào tệp `expression-test.jsp`.
2. Chọn **Run As** -> **Run on Server**.
3. Chọn máy chủ Tomcat đã được cấu hình trước và nhấp **Finish**.

### Kết Quả

Khi chạy thành công, trình duyệt sẽ mở ra và hiển thị kết quả:

```
Sử dụng JSP Expressions

Chuỗi "Hello World" sau khi chuyển đổi thành in hoa: HELLO WORLD

25 nhân với 4 bằng: 100

75 có nhỏ hơn 69 không? false
```

Kết quả hiển thị như trên cho thấy cách JSP Expression hoạt động: Tính toán biểu thức Java và chèn kết quả vào trang HTML.

## 4. Giải Thích Chi Tiết

1. **Chuyển Đổi Chuỗi Thành In Hoa**:

   - Biểu thức `<%= new String("Hello World").toUpperCase() %>` tạo một đối tượng chuỗi mới và chuyển đổi nó thành in hoa.
   - Kết quả `"HELLO WORLD"` sẽ được chèn vào trang HTML.

2. **Thực Hiện Phép Toán**:

   - Biểu thức `<%= 25 * 4 %>` tính toán và hiển thị kết quả là `100`.

3. **Biểu Thức Boolean**:

   - Biểu thức `<%= 75 < 69 %>` kiểm tra xem 75 có nhỏ hơn 69 không.
   - Kết quả là `false` sẽ được hiển thị trên trang HTML.

## 5. Tổng Kết

Trong bài học này, chúng ta đã tìm hiểu về **JSP Expressions** và cách sử dụng chúng để hiển thị kết quả của các biểu thức Java lên trang HTML. Đây là một cách đơn giản và hiệu quả để chèn các đoạn mã Java nhỏ vào trang JSP và hiển thị kết quả.

### Những Điều Bạn Có Thể Làm Tiếp Theo

- Thử nghiệm thêm các biểu thức Java khác như các phép toán phức tạp hơn, các phép so sánh hoặc các phương thức của đối tượng Java khác.
- Kết hợp JSP Expressions với JSP Scriptlets và JSP Declarations để tạo ra các trang web động hoàn chỉnh hơn.

Hãy tiếp tục khám phá JSP và thử nghiệm thêm các tính năng khác để xây dựng các trang web động theo ý tưởng của bạn!
