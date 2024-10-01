# Hướng Dẫn Sử Dụng JSP Để Bao Gồm Các Tệp Khác

## Giới Thiệu

Trong video này, chúng ta sẽ học cách sử dụng JSP để bao gồm các tệp khác vào một trang web. Mục đích phổ biến của việc này là để tái sử dụng các phần chung của trang web như header và footer, nhằm tránh phải lặp lại mã trên nhiều trang khác nhau.

## Tại Sao Nên Bao Gồm Các Tệp?

Khi bạn xây dựng một website, bạn thường muốn có cùng một header và footer hiển thị trên tất cả các trang. Thay vì sao chép và dán nội dung đó trên mỗi trang, bạn có thể tạo các tệp riêng biệt cho header và footer, sau đó sử dụng JSP để bao gồm chúng vào từng trang.

## Cách Bao Gồm Tệp Trong JSP

Trong JSP, bạn có thể bao gồm cả tệp HTML hoặc JSP khác bằng cách sử dụng thẻ `<jsp:include>`. Dưới đây là cách thực hiện:

```jsp
<jsp:include page="my-header.html" />
<jsp:include page="my-footer.jsp" />
```

- `my-header.html`: Đây là tệp HTML cho phần header của trang.
- `my-footer.jsp`: Đây là tệp JSP chứa phần footer của trang, có thể bao gồm cả mã Java nếu cần.

## Thực Hiện Ví Dụ Trong Eclipse

Chúng ta sẽ tạo một ví dụ đơn giản gồm:
1. **Tạo tệp header (`my-header.html`)**.
2. **Tạo tệp footer (`my-footer.jsp`)**.
3. **Tạo trang chính (`homepage.jsp`)** để bao gồm cả hai tệp trên.

### Bước 1: Tạo Tệp Header (`my-header.html`)

1. Mở dự án **jspdemo** trong Eclipse.
2. Nhấp chuột phải vào thư mục **WebContent**, chọn **New** -> **File**.
3. Đặt tên tệp là `my-header.html` và nhấn **Finish**.
4. Thêm mã sau vào tệp `my-header.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Header</title>
</head>
<body>
    <h1 style="text-align: center;">JSP Tutorial</h1>
</body>
</html>
```

Tệp này sẽ hiển thị tiêu đề "JSP Tutorial" ở giữa trang.

### Bước 2: Tạo Tệp Footer (`my-footer.jsp`)

1. Tương tự, nhấp chuột phải vào thư mục **WebContent**, chọn **New** -> **File**.
2. Đặt tên tệp là `my-footer.jsp` và nhấn **Finish**.
3. Thêm mã sau vào tệp `my-footer.jsp`:

```jsp
<!DOCTYPE html>
<html>
<body>
    <hr>
    <p style="text-align: center;">
        Last updated on: <%= new java.util.Date() %>
    </p>
</body>
</html>
```

Tệp này sẽ hiển thị dòng chữ "Last updated on" kèm theo ngày giờ hiện tại khi trang được truy cập.

### Bước 3: Tạo Trang Chính (`homepage.jsp`)

1. Nhấp chuột phải vào thư mục **WebContent**, chọn **New** -> **File**.
2. Đặt tên tệp là `homepage.jsp` và nhấn **Finish**.
3. Thêm mã sau vào tệp `homepage.jsp`:

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Home Page</title>
</head>
<body>

    <!-- Bao gồm header -->
    <jsp:include page="my-header.html" />

    <!-- Nội dung chính của trang -->
    <h2>Welcome to our JSP Tutorial Website</h2>
    <p>This is the main content area.</p>
    <p>Here you can add more information about your site.</p>

    <!-- Bao gồm footer -->
    <jsp:include page="my-footer.jsp" />

</body>
</html>
```

Tệp này sẽ bao gồm tệp `my-header.html` ở đầu trang và `my-footer.jsp` ở cuối trang, đồng thời hiển thị nội dung chính ở giữa.

### Bước 4: Chạy Trang `homepage.jsp`

1. Nhấp chuột phải vào tệp `homepage.jsp`.
2. Chọn **Run As** -> **Run on Server**.
3. Chọn máy chủ Tomcat nếu được nhắc nhở và nhấn **Finish**.

### Kết Quả

Khi trang `homepage.jsp` được hiển thị trên trình duyệt, bạn sẽ thấy:

- **Header**: `JSP Tutorial` hiển thị ở giữa trang.
- **Nội dung chính**: Hiển thị lời chào và mô tả trang.
- **Footer**: Hiển thị thời gian cập nhật cuối cùng của trang.

### Tại Sao Sử Dụng `jsp:include`?

- **Dễ bảo trì**: Nếu bạn muốn thay đổi header hoặc footer, bạn chỉ cần cập nhật tệp `my-header.html` hoặc `my-footer.jsp` mà không cần thay đổi tất cả các trang web.
- **Giúp tổ chức mã tốt hơn**: Phân chia nội dung ra nhiều tệp giúp mã dễ đọc và dễ hiểu hơn.

## Tổng Kết

Trong video này, chúng ta đã học cách:

1. Tạo tệp header (`my-header.html`) và footer (`my-footer.jsp`).
2. Bao gồm các tệp này vào trang chính (`homepage.jsp`) bằng cách sử dụng thẻ `<jsp:include>`.
3. Hiển thị kết quả trên trình duyệt.

### Lưu Ý

Bạn có thể sử dụng thẻ `<jsp:include>` để bao gồm bất kỳ tệp HTML hoặc JSP nào, giúp tạo ra các trang web dễ bảo trì và quản lý hơn. Đây là một kỹ thuật quan trọng trong việc xây dựng các ứng dụng web với JSP. Hãy thực hành và khám phá thêm các cách khác nhau để sử dụng `jsp:include`!
