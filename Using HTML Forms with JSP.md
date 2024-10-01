# Hướng Dẫn Sử Dụng HTML Forms với JSP

Trong video này, chúng ta sẽ học cách sử dụng HTML forms kết hợp với JSP (JavaServer Pages) để tạo ra một form nhập liệu cơ bản và xử lý dữ liệu trên server. Đây là một phần quan trọng trong việc xây dựng các ứng dụng web động, cho phép người dùng tương tác và gửi thông tin qua các biểu mẫu.

## Nội Dung Bài Học

Chúng ta sẽ lần lượt đi qua các bước sau:

1. **Ôn Tập Về HTML Forms**: Tìm hiểu cơ bản về cách tạo HTML forms và cấu trúc của chúng.
2. **Xây Dựng HTML Form**: Tạo một biểu mẫu nhập thông tin học viên với các trường như tên và họ.
3. **Đọc Dữ Liệu Từ Form Với JSP**: Xử lý và đọc dữ liệu từ biểu mẫu gửi lên bằng trang JSP.
4. **Gửi Phản Hồi**: Hiển thị thông tin xác nhận về những dữ liệu mà người dùng đã nhập vào.

## HTML Forms là Gì?

HTML forms là một thành phần cơ bản trong hầu hết các website mà chúng ta tương tác hàng ngày. Nếu bạn đã từng đăng nhập vào một trang web bằng tên người dùng và mật khẩu, hoặc đặt vé máy bay hay đặt phòng khách sạn, thì bạn đã tương tác với một **HTML form**. Chúng ta sẽ bắt đầu bằng việc tạo một biểu mẫu HTML cho phép học viên nhập thông tin của mình.

## Tạo HTML Form Đơn Giản

Dưới đây là mã nguồn để tạo một form HTML cơ bản cho thông tin học viên, bao gồm trường nhập liệu cho tên và họ, cùng một nút gửi (submit):

```html
<!DOCTYPE html>
<html>
<head>
    <title>Student Information Form</title>
</head>
<body>
    <h2>Student Information</h2>
    <form action="student-response.jsp" method="post">
        First Name: <input type="text" name="firstName"><br><br>
        Last Name: <input type="text" name="lastName"><br><br>
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

### Giải Thích:

- **Thẻ `<form>`**: Xác định một biểu mẫu trong HTML. Thuộc tính `action` xác định nơi gửi dữ liệu biểu mẫu, ở đây là trang `student-response.jsp`. Thuộc tính `method` là `post` để gửi dữ liệu theo phương thức POST.
- **Các trường nhập liệu (`input type="text"`)**: Sử dụng cho việc nhập tên (`name="firstName"`) và họ (`name="lastName"`). Tên (`name`) của các trường này sẽ được dùng để đọc dữ liệu trên server.
- **Nút Gửi (`input type="submit"`)**: Gửi dữ liệu biểu mẫu đến trang JSP đã chỉ định trong thuộc tính `action`.

## Đọc Dữ Liệu Form Bằng JSP

Dưới đây là mã JSP để xử lý dữ liệu mà người dùng nhập vào từ biểu mẫu:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Student Confirmation</title>
</head>
<body>
    <h2>Student Information Confirmation</h2>
    <p>The student is confirmed.</p>
    <p>First Name: <%= request.getParameter("firstName") %></p>
    <p>Last Name: <%= request.getParameter("lastName") %></p>
</body>
</html>
```

### Giải Thích:

- **`<%= request.getParameter("firstName") %>`**: Đây là cách để đọc dữ liệu từ biểu mẫu mà người dùng đã gửi. `request.getParameter("firstName")` sẽ trả về giá trị của trường `firstName` trong biểu mẫu HTML. Tương tự với `lastName`.
- **Hiển thị thông tin**: Dữ liệu mà người dùng nhập sẽ được hiển thị trở lại trên trang xác nhận, giúp người dùng kiểm tra thông tin mình đã điền.

## Phương Pháp Khác Để Đọc Dữ Liệu Form Trong JSP

Ngoài cách sử dụng `request.getParameter()`, bạn có thể sử dụng một cú pháp ngắn gọn hơn với `param` như sau:

```jsp
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Student Confirmation</title>
</head>
<body>
    <h2>Student Information Confirmation</h2>
    <p>The student is confirmed.</p>
    <p>First Name: ${param.firstName}</p>
    <p>Last Name: ${param.lastName}</p>
</body>
</html>
```

### Giải Thích:

- **`${param.firstName}`**: Đây là cách viết ngắn gọn để lấy giá trị của trường `firstName` trong form. `param` là một đối tượng trong JSP được sử dụng để lấy dữ liệu từ biểu mẫu. Cách này chỉ dùng để hiển thị dữ liệu và không thể sử dụng trong scriptlet hoặc logic phức tạp khác.

## Tóm Tắt

Qua bài học này, bạn đã biết cách tạo một biểu mẫu HTML đơn giản để nhập thông tin học viên và sử dụng JSP để đọc và hiển thị thông tin này. Đây là một ví dụ cơ bản nhưng rất hữu ích để hiểu về cách tương tác giữa frontend (HTML) và backend (JSP) trong một ứng dụng web. 

Trong video tiếp theo, chúng ta sẽ thực hành viết mã trong Eclipse và thực hiện chạy ứng dụng để xem kết quả.
