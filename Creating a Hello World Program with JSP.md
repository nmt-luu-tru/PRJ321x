# Hướng Dẫn Tạo Chương Trình "Hello World" Với JSP

Trong video này, chúng ta sẽ học cách tạo một chương trình "Hello World" cơ bản với JSP (JavaServer Pages). Đây là bước đầu tiên để bạn làm quen với cách tạo các trang web động bằng cách sử dụng JSP. Chúng ta sẽ tạo một tệp JSP đơn giản, viết mã HTML và chèn một đoạn mã Java để hiển thị thời gian hiện tại trên server.

## 1. JSP Là Gì?

JSP là một trang HTML có chứa mã Java để tạo ra nội dung động. Bằng cách chèn mã Java vào HTML, JSP cho phép bạn tạo ra các trang HTML động dựa trên các yêu cầu từ phía người dùng hoặc các thao tác trên server.

### Cách Thức Hoạt Động Của JSP

- **Quá trình xử lý**: Khi bạn yêu cầu một trang JSP, yêu cầu này sẽ được gửi đến máy chủ (ví dụ như Tomcat hoặc GlassFish).
- **Máy chủ xử lý mã Java**: Máy chủ sẽ xử lý mã Java trong trang JSP và tạo ra một trang HTML động.
- **Kết quả trả về cho trình duyệt**: Trang HTML kết quả sẽ được gửi lại cho trình duyệt, và trình duyệt sẽ hiển thị nội dung như một trang HTML tĩnh.

Ví dụ: Bạn yêu cầu một trang JSP để hiển thị thời gian hiện tại trên máy chủ. Máy chủ sẽ chạy mã Java để lấy thời gian hiện tại, sau đó chèn thời gian này vào một trang HTML và trả kết quả lại cho trình duyệt.

## 2. Tạo Dự Án JSP Trong Eclipse

Bây giờ, chúng ta sẽ tạo một dự án JSP mới trong Eclipse để bắt đầu viết mã.

1. **Tạo Dự Án Mới**:
   - Mở Eclipse.
   - Chọn **File** -> **New** -> **Dynamic Web Project**.
   - Nhập tên dự án, ví dụ: `JSPDemo`.
   - Nhấp vào **Next** và chọn **Finish**.

2. **Thiết Lập Thư Mục Dự Án**:
   - Khi tạo dự án, Eclipse sẽ tạo ra một thư mục có tên **WebContent**. Đây là nơi bạn sẽ đặt các tệp JSP của mình.
   - Bạn có thể tùy chỉnh cấu trúc dự án bằng cách thêm các thư mục con nếu cần thiết.

## 3. Tạo Tệp JSP "Hello World"

Bây giờ, chúng ta sẽ tạo tệp JSP đầu tiên trong dự án.

1. Nhấp chuột phải vào thư mục **WebContent**, chọn **New** -> **File**.
2. Đặt tên tệp là `helloworld.jsp` và nhấp vào **Finish**.

### Nội Dung Tệp `helloworld.jsp`

Dán đoạn mã dưới đây vào tệp `helloworld.jsp`:

```jsp
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Hello World JSP</title>
</head>
<body>
    <h3>Hello World of Java</h3>
    <p>The time on the server is: <%= new java.util.Date() %></p>
</body>
</html>
```

### Giải Thích:
- **Thẻ `<%=%>`**: Đây là cách bạn chèn mã Java vào trang JSP. Mã Java trong thẻ này sẽ được thực thi trên server và kết quả sẽ được chèn vào trang HTML.
- **`new java.util.Date()`**: Tạo một đối tượng `Date` để hiển thị thời gian hiện tại trên server.

## 4. Chạy Chương Trình JSP

Bây giờ, chúng ta sẽ chạy chương trình JSP và xem kết quả.

1. Nhấp chuột phải vào tệp `helloworld.jsp`.
2. Chọn **Run As** -> **Run on Server**.
3. Chọn máy chủ đã được cấu hình trước (ví dụ: Apache Tomcat 8) và nhấp vào **Finish**.

Khi chạy thành công, trình duyệt sẽ mở ra và hiển thị kết quả:

```
Hello World of Java
The time on the server is: [Thời gian hiện tại trên máy chủ]
```

### Kiểm Tra Mã Nguồn Trang HTML
- Bạn có thể nhấp chuột phải vào trang và chọn **View Page Source** để xem mã nguồn HTML đã được tạo.
- Bạn sẽ thấy mã HTML đã hiển thị nội dung động với thời gian hiện tại mà không chứa mã Java, vì mã Java đã được xử lý trên server.

## 5. Giải Thích Quy Trình Hoạt Động

1. **Trình duyệt gửi yêu cầu đến server**: Trình duyệt gửi yêu cầu đến server để lấy trang `helloworld.jsp`.
2. **Server xử lý mã Java**: Server xử lý mã Java trong thẻ `<%= %>`.
3. **Tạo ra trang HTML động**: Server tạo ra trang HTML động với kết quả thời gian hiện tại và trả lại cho trình duyệt.
4. **Trình duyệt hiển thị kết quả**: Trình duyệt nhận và hiển thị trang HTML đã được tạo.

## 6. Tổng Kết

Trong bài học này, bạn đã biết cách tạo một chương trình "Hello World" đơn giản với JSP. Đây là một ví dụ cơ bản để bạn hiểu cách thức hoạt động của JSP trong việc xử lý mã Java và tạo nội dung HTML động.

### Những Điều Bạn Có Thể Làm Tiếp Theo
- Thử thêm các thành phần Java khác như vòng lặp (`for`, `while`) hoặc điều kiện (`if-else`) vào trang JSP.
- Kết nối với cơ sở dữ liệu để hiển thị thông tin động từ cơ sở dữ liệu lên trang web.
- Sử dụng các thẻ tùy chỉnh trong JSP để viết mã dễ đọc hơn và tách biệt logic xử lý.

Hãy tiếp tục khám phá và thử nghiệm với JSP để xây dựng các trang web động theo ý tưởng của bạn!
