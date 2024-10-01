# Hướng Dẫn Sử Dụng Các Đối Tượng Server Được Tích Hợp Sẵn Trong JSP

## Giới Thiệu

Trong video này, chúng ta sẽ học cách sử dụng các đối tượng server được tích hợp sẵn trong JSP. Đây là các đối tượng được cung cấp sẵn bởi môi trường JSP và có thể được sử dụng trực tiếp mà không cần phải tạo mới.

## Các Đối Tượng Server Thường Dùng Trong JSP

Dưới đây là danh sách các đối tượng server phổ biến trong JSP:

1. **`request`**: Chứa thông tin về các header HTTP và dữ liệu từ form.
2. **`response`**: Dùng để gửi lại thông tin HTTP như header và dữ liệu trả về.
3. **`out`**: Dùng để xuất dữ liệu ra trang JSP. Chúng ta đã sử dụng đối tượng này trong các ví dụ trước với `out.println`.
4. **`session`**: Đối tượng session duy nhất cho mỗi người dùng, giống như một giỏ hàng khi bạn mua sắm trực tuyến.
5. **`application`**: Đối tượng được chia sẻ cho tất cả người dùng của một ứng dụng web.

Đây là các đối tượng được sử dụng rộng rãi trong các ứng dụng JSP để xử lý dữ liệu từ người dùng và trả kết quả về trình duyệt.

## Ý Tưởng Sử Dụng `request` Và `response` 

Trong mô hình giao tiếp giữa trình duyệt và JSP, khi người dùng gửi yêu cầu đến server, một đối tượng `request` sẽ được tạo ra chứa thông tin về yêu cầu đó, như header và body của HTTP request. Sau đó, JSP sẽ xử lý thông tin này và trả về kết quả dưới dạng `response`.

## Thực Hiện Mã Ví Dụ Trong Eclipse

Chúng ta sẽ thực hiện một ví dụ đơn giản để lấy thông tin về trình duyệt và ngôn ngữ mà người dùng đang sử dụng từ đối tượng `request`.

### Bước 1: Tạo Trang JSP Mới

1. Mở dự án **jspdemo** trong Eclipse.
2. Mở thư mục **WebContent**.
3. Nhấp chuột phải, chọn **New** -> **File**.
4. Đặt tên cho tệp là `builtin-test.jsp`.
5. Nhấp **Finish**.

### Bước 2: Viết Mã JSP Sử Dụng Đối Tượng `request`

Trong tệp `builtin-test.jsp`, thêm mã sau:

```jsp
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JSP Built-In Objects Example</title>
</head>
<body>
    <h3>JSP Built-In Server Objects Example</h3>
    
    <!-- Đọc thông tin từ đối tượng request -->
    <p>Request User Agent: 
        <%= request.getHeader("User-Agent") %>
    </p>
    
    <p>Request Locale (Ngôn ngữ người dùng): 
        <%= request.getLocale() %>
    </p>
</body>
</html>
```

### Bước 3: Chạy Trang JSP

1. Nhấp chuột phải vào tệp `builtin-test.jsp`.
2. Chọn **Run As** -> **Run on Server**.
3. Chọn máy chủ Tomcat nếu được nhắc nhở và nhấp **Finish**.

### Kết Quả

- **Request User Agent**: Hiển thị loại trình duyệt và hệ điều hành mà người dùng đang sử dụng, ví dụ: `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36`.
- **Request Locale (Ngôn ngữ người dùng)**: Hiển thị ngôn ngữ mà người dùng đang sử dụng trong trình duyệt, ví dụ: `en_US` (tiếng Anh Hoa Kỳ).

### Kiểm Tra Với Trình Duyệt Khác

1. Sao chép URL của trang `builtin-test.jsp`.
2. Mở một trình duyệt khác (như Chrome hoặc Firefox) và dán URL đó vào thanh địa chỉ.
3. Nhấn Enter và kiểm tra kết quả hiển thị.

Kết quả sẽ hiển thị khác nhau tùy thuộc vào loại trình duyệt và hệ điều hành mà bạn đang sử dụng.

## Tổng Kết

Trong video này, chúng ta đã học cách:

1. **Sử dụng đối tượng `request` trong JSP**: Lấy thông tin về trình duyệt và ngôn ngữ của người dùng.
2. **Hiển thị kết quả**: Sử dụng `request.getHeader("User-Agent")` để hiển thị thông tin về trình duyệt, và `request.getLocale()` để hiển thị ngôn ngữ người dùng.

## Kết Luận

Đây là ví dụ cơ bản về cách sử dụng các đối tượng server được tích hợp sẵn trong JSP. Trong các video tiếp theo, chúng ta sẽ tìm hiểu thêm về cách sử dụng các đối tượng khác như `response`, `session`, và `application` để xây dựng các ứng dụng web phức tạp hơn.

Hãy theo dõi và thực hành để nắm vững cách làm việc với các đối tượng server trong JSP!
