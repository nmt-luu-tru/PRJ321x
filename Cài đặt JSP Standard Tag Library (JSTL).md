# Cài đặt JSP Standard Tag Library (JSTL)

Trong bài viết này, chúng ta sẽ tìm hiểu cách cài đặt JSP Standard Tag Library (JSTL) trong Eclipse và tạo một ví dụ đơn giản để kiểm tra việc cài đặt. JSTL là một thư viện thẻ tiêu chuẩn cho JSP, giúp đơn giản hóa việc viết mã và tăng hiệu quả phát triển ứng dụng web.

## Mục lục

1. [Giới thiệu về JSTL](#giới-thiệu-về-jstl)
2. [Tạo dự án mới trong Eclipse](#tạo-dự-án-mới-trong-eclipse)
3. [Cấu hình dự án](#cấu-hình-dự-án)
4. [Tải xuống và thêm thư viện JSTL](#tải-xuống-và-thêm-thư-viện-jstl)
5. [Tạo trang JSP để kiểm tra](#tạo-trang-jsp-để-kiểm-tra)
6. [Chạy và kiểm tra ứng dụng](#chạy-và-kiểm-tra-ứng-dụng)
7. [Kết luận](#kết-luận)

## Giới thiệu về JSTL

JSP Standard Tag Library (JSTL) là một tập hợp các thẻ tùy chỉnh được chuẩn hóa, cung cấp các chức năng thường dùng trong phát triển web như lặp, điều kiện, xử lý chuỗi, và quốc tế hóa. Việc sử dụng JSTL giúp tách biệt **business logic** khỏi mã hiển thị, làm cho mã nguồn sạch sẽ và dễ bảo trì hơn.

## Tạo dự án mới trong Eclipse

### Bước 1: Tạo dự án Web động

1. Mở Eclipse và vào menu **File** > **New** > **Dynamic Web Project**.

2. Đặt tên cho dự án, ví dụ: `tag-demo`.

3. Nhấn **Next** để tiếp tục.

## Cấu hình dự án

### Bước 2: Thiết lập thư mục nguồn

1. Trong phần **Source folders on build path**, chọn thư mục `src/main/java` và nhấn **Remove** để xóa.

2. Nhấn **Add Folder** và thêm thư mục mới với tên `src`.

3. Mục đích của việc này là để đồng bộ với cấu trúc dự án trong các video hoặc tài liệu trước đây.

### Bước 3: Thiết lập thư mục nội dung Web

1. Nhấn **Next** để chuyển đến phần **Web Module**.

2. Đổi **Content Directory** thành `WebContent` để phù hợp với cấu trúc thông thường của các dự án web.

3. Nhấn **Finish** để hoàn tất việc tạo dự án.

## Tải xuống và thêm thư viện JSTL

### Bước 4: Tải xuống JSTL

1. Mở trình duyệt web và truy cập vào URL:

    ```
    https://lovetocode.com/download-jstl
    ```

2. Tải xuống tệp zip chứa các thư viện JSTL cần thiết.

### Bước 5: Thêm thư viện vào dự án

1. Giải nén tệp zip đã tải xuống, bạn sẽ thấy hai tệp JAR:

    - `javax.servlet.jsp.jstl-api-1.2.1.jar`
    - `javax.servlet.jsp.jstl-1.2.1.jar`

2. Sao chép cả hai tệp JAR này.

3. Trong Eclipse, điều hướng đến thư mục **WebContent/WEB-INF/lib** trong dự án của bạn.

4. Dán hai tệp JAR vào thư mục **lib**.

**Lưu ý:** Việc đặt các tệp JAR vào thư mục **WEB-INF/lib** là rất quan trọng, vì đây là thư mục mà máy chủ ứng dụng sẽ tìm kiếm các thư viện cần thiết cho ứng dụng web của bạn.

## Tạo trang JSP để kiểm tra

### Bước 6: Tạo tệp JSP mới

1. Trong thư mục **WebContent**, nhấp chuột phải và chọn **New** > **JSP File**.

2. Đặt tên cho tệp, ví dụ: `test.jsp`.

3. Nhấn **Finish** để tạo tệp.

### Bước 7: Sử dụng JSTL  

**Nếu không JSTL để thiết lập biến:**

Mở tệp `test.jsp` và thêm nội dung sau:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
    <title>Kiểm tra JSTL</title>
</head>
<body>
    <h2>Sử dụng JSTL Core Tag</h2>
    <%
        // Thiết lập thời gian hiện tại trong biến "currentTime"
        java.util.Date date = new java.util.Date();
        request.setAttribute("currentTime", date);
    %>
    <p>Thời gian trên máy chủ là: ${currentTime}</p>
</body>
</html>
```

**Giải thích:**

- Dòng đầu tiên khai báo thư viện thẻ JSTL Core với tiền tố `c`.
- Sử dụng biểu thức `${currentTime}` để hiển thị thời gian hiện tại.
- Chúng ta đã đặt thuộc tính `currentTime` trong đối tượng `request` để sử dụng trong biểu thức.

**Sử dụng JSTL để thiết lập biến:**

Thay vì sử dụng mã Java, bạn có thể sử dụng thẻ JSTL `<c:set>` để thiết lập biến:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
    <title>Kiểm tra JSTL</title>
</head>
<body>
    <h2>Sử dụng JSTL Core Tag</h2>
    <c:set var="currentTime" value="<%= new java.util.Date() %>" />
    <p>Thời gian trên máy chủ là: ${currentTime}</p>
</body>
</html>
```

**Giải thích:**

- Thẻ `<c:set>` được sử dụng để thiết lập biến `currentTime` với giá trị thời gian hiện tại.
- Sử dụng `${currentTime}` để hiển thị giá trị của biến.

## Chạy và kiểm tra ứng dụng

### Bước 8: Chạy trang JSP

1. Nhấp chuột phải vào tệp `test.jsp` và chọn **Run As** > **Run on Server**.

2. Chọn máy chủ ứng dụng (ví dụ: Apache Tomcat) và nhấn **Finish**.

### Bước 9: Kiểm tra kết quả

Trang web sẽ mở ra trong trình duyệt tích hợp của Eclipse hoặc trình duyệt mặc định của bạn.

**Kết quả:**

```
Sử dụng JSTL Core Tag

Thời gian trên máy chủ là: Sat Sep 30 12:34:56 ICT 2023
```

**Giải thích:**

- Thời gian hiện tại trên máy chủ được hiển thị thông qua biến `currentTime` sử dụng JSTL.

## Kết luận

Trong bài viết này, chúng ta đã học cách cài đặt JSTL vào dự án JSP trong Eclipse và tạo một ví dụ đơn giản để kiểm tra việc cài đặt. Việc sử dụng JSTL giúp mã nguồn JSP trở nên sạch sẽ hơn và tách biệt **business logic** khỏi mã hiển thị.

**Lợi ích của việc sử dụng JSTL:**

- **Đơn giản hóa mã JSP**: Giảm thiểu việc sử dụng mã Java trong trang JSP.
- **Tăng tính đọc hiểu**: Mã nguồn rõ ràng, dễ bảo trì.
- **Tái sử dụng**: Các thẻ JSTL có thể được sử dụng trong nhiều trang và dự án khác nhau.

**Tiếp theo:**

- Tìm hiểu sâu hơn về các nhóm thẻ trong JSTL như Core Tags, Function Tags, và sử dụng chúng trong các ứng dụng thực tế.
- Khám phá cách tạo và sử dụng JSP Custom Tags để tùy chỉnh chức năng theo nhu cầu cụ thể.

**Chúc mừng bạn đã hoàn thành việc cài đặt JSTL và thực hiện ví dụ đầu tiên!**
