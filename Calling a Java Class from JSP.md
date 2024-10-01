# Hướng Dẫn Gọi Lớp Java Từ Trang JSP

Trong video này, chúng ta sẽ học cách gọi một lớp Java từ trang JSP. Đây là một phương pháp tốt để tách rời các đoạn mã Java phức tạp khỏi JSP, giúp cho mã nguồn trở nên dễ bảo trì và dễ đọc hơn.

## 1. Tại Sao Phải Sử Dụng Lớp Java Riêng Biệt?

Như đã đề cập trong các video trước, bạn muốn hạn chế số lượng scriptlet và các đoạn mã Java phức tạp trong JSP. Việc đưa các đoạn mã dài vào trong JSP không chỉ làm cho mã nguồn trở nên khó quản lý mà còn khiến hiệu suất xử lý trở nên kém hiệu quả.

Để giải quyết vấn đề này, bạn có thể:
- Tách riêng các đoạn mã phức tạp thành các lớp Java riêng biệt.
- Sử dụng mô hình MVC (Model-View-Controller).

Trong video này, chúng ta sẽ thực hiện việc tách riêng mã thành một lớp Java.

## 2. Kế Hoạch Thực Hiện

### To-do List

1. **Tạo lớp Java:** Chứa phương thức xử lý dữ liệu.
2. **Gọi lớp Java từ trang JSP:** Sử dụng lớp này để xử lý dữ liệu và hiển thị kết quả trên trang JSP.

## 3. Thực Hiện Trong Eclipse

Chúng ta sẽ thực hiện theo từng bước như sau:

### Bước 1: Tạo Lớp Java Mới

1. Mở dự án **jspdemo** trong Eclipse.
2. Trong **Java Resources** -> **src**, nhấp chuột phải và chọn **New** -> **Package**.
3. Đặt tên cho package là `com.luv2code.jsp` và nhấp **Finish**.

4. Nhấp chuột phải vào package vừa tạo, chọn **New** -> **Class**.
5. Đặt tên lớp là `FunUtils`.
6. Nhấp **Finish** để tạo lớp.

### Bước 2: Thêm Phương Thức Vào Lớp Java

Trong lớp `FunUtils.java`, thêm mã sau:

```java
package com.luv2code.jsp;

public class FunUtils {

    // Phương thức chuyển đổi chuỗi thành chữ thường
    public static String makeItLower(String data) {
        return data.toLowerCase();
    }
}
```

- Phương thức `makeItLower` sẽ nhận một chuỗi và trả về phiên bản chữ thường của chuỗi đó.

### Bước 3: Tạo Trang JSP Để Gọi Lớp Java

1. Nhấp chuột phải vào thư mục **WebContent**.
2. Chọn **New** -> **File**.
3. Đặt tên tệp là `fun-test.jsp` và nhấp **Finish**.

### Bước 4: Viết Mã JSP Để Gọi Lớp Java

Trong tệp `fun-test.jsp`, thêm mã sau:

```jsp
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JSP Java Class Integration</title>
</head>
<body>
    <h3>Let's have some fun!</h3>
    
    <%@ page import="com.luv2code.jsp.FunUtils" %>
    
    <!-- Sử dụng lớp FunUtils để chuyển chuỗi thành chữ thường -->
    <p>Kết quả: 
        <%= FunUtils.makeItLower("FUN FUN FUN") %>
    </p>
</body>
</html>
```

- Dòng `<%@ page import="com.luv2code.jsp.FunUtils" %>` dùng để import lớp `FunUtils` vào trang JSP.
- Gọi phương thức `FunUtils.makeItLower("FUN FUN FUN")` để chuyển chuỗi `"FUN FUN FUN"` thành chữ thường và hiển thị kết quả.

### Bước 5: Chạy Trang JSP

1. Nhấp chuột phải vào tệp `fun-test.jsp`.
2. Chọn **Run As** -> **Run on Server**.
3. Chọn máy chủ Tomcat nếu được nhắc nhở và nhấp **Finish**.

### Kết Quả

Trình duyệt sẽ mở và hiển thị kết quả:

```
Let's have some fun!

Kết quả: fun fun fun
```

Kết quả hiển thị đúng cho thấy rằng trang JSP đã gọi thành công phương thức `makeItLower` từ lớp Java `FunUtils` để chuyển chuỗi `"FUN FUN FUN"` thành `"fun fun fun"`.

## 4. Tổng Kết

Trong video này, chúng ta đã học cách:

1. Tạo một lớp Java riêng biệt để chứa các phương thức xử lý dữ liệu.
2. Gọi lớp Java từ trang JSP bằng cách sử dụng `import`.
3. Thực hiện xử lý dữ liệu và hiển thị kết quả trên trang JSP.

### Lưu Ý:

- **Hạn chế mã Java trong JSP:** Hãy luôn tách biệt mã Java phức tạp ra khỏi JSP để mã dễ quản lý và bảo trì hơn.
- **Sử dụng mô hình MVC:** Ở những bài học sau, chúng ta sẽ tìm hiểu cách sử dụng mô hình MVC để xây dựng các ứng dụng web mạnh mẽ hơn.

### Tiếp Theo

Trong các bài học tiếp theo, chúng ta sẽ tìm hiểu cách kết hợp JSP với Servlets để xây dựng các ứng dụng web hoàn chỉnh và mạnh mẽ hơn.
