# JSTL Function Tags - length, toUpperCase and startsWith

Trong bài viết này, chúng ta sẽ tìm hiểu cách sử dụng các thẻ JSTL Function, cung cấp một số hàm tiện ích để thao tác với chuỗi và các đối tượng khác. Những thẻ này giúp bạn thực hiện các chức năng như đo độ dài chuỗi, chuyển đổi chuỗi sang chữ hoa hoặc chữ thường, kiểm tra tiền tố, và nhiều thao tác khác.

## Mục Lục

1. [Giới Thiệu Về Thẻ JSTL Function](#gioi-thieu)
2. [Cú Pháp Và Cách Sử Dụng](#cu-phap)
3. [Ví Dụ Sử Dụng Thẻ JSTL Function](#vi-du)
   - [Ví Dụ 1: Đo Độ Dài Chuỗi](#vi-du-1)
   - [Ví Dụ 2: Chuyển Đổi Chữ Hoa Chữ Thường](#vi-du-2)
   - [Ví Dụ 3: Kiểm Tra Tiền Tố Chuỗi](#vi-du-3)
4. [Lợi Ích Của Việc Sử Dụng Thẻ JSTL Function](#loi-ich)
5. [Kết Luận](#ket-luan)

---

## 1. Giới Thiệu Về Thẻ JSTL Function

Thẻ JSTL Function là một tập hợp các hàm tiện ích được thiết kế để xử lý chuỗi và các đối tượng khác trong JSP (JavaServer Pages). Những hàm này rất nhẹ và dễ sử dụng, cho phép bạn thực hiện các thao tác như:

- Tính độ dài của chuỗi
- Chuyển đổi chuỗi sang chữ hoa hoặc chữ thường
- Kiểm tra tiền tố hoặc hậu tố của chuỗi
- Thao tác với danh sách và mảng

Thẻ Function thường được khai báo với tiền tố là **`fn`** và phải được khai báo trong trang JSP của bạn.

---

## 2. Cú Pháp Và Cách Sử Dụng

Để sử dụng các thẻ JSTL Function, bạn cần khai báo thư viện thẻ trong phần đầu của trang JSP:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
```

- **`uri`**: Đường dẫn URI đến thư viện JSTL Function.
- **`prefix`**: Tên tiền tố mà bạn sẽ sử dụng để gọi các hàm trong thư viện này (thường dùng là `fn`).

Một số thẻ function phổ biến bao gồm:

- **`fn:length()`**: Trả về độ dài của chuỗi hoặc mảng.
- **`fn:toUpperCase()`**: Chuyển chuỗi sang chữ hoa.
- **`fn:toLowerCase()`**: Chuyển chuỗi sang chữ thường.
- **`fn:startsWith()`**: Kiểm tra xem một chuỗi có bắt đầu bằng chuỗi con nào đó hay không.
- **`fn:contains()`**: Kiểm tra xem chuỗi có chứa chuỗi con hay không.

---

## 3. Ví Dụ Sử Dụng Thẻ JSTL Function

### Ví Dụ 1: Đo Độ Dài Chuỗi

Trong ví dụ này, chúng ta sẽ sử dụng thẻ `fn:length()` để đo độ dài của một chuỗi.

**Mã nguồn JSP:**

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<!DOCTYPE html>
<html>
<head>
    <title>Đo Độ Dài Chuỗi</title>
</head>
<body>
    <%
        // Khai báo một biến chuỗi
        String data = "Luv2code";
        pageContext.setAttribute("data", data);
    %>

    <h2>Ví Dụ JSTL Function: Đo Độ Dài Chuỗi</h2>
    <p>Chuỗi: ${data}</p>
    <p>Độ dài của chuỗi là: ${fn:length(data)}</p>
</body>
</html>
```

**Giải thích:**

- **`<%@ taglib ... %>`**: Khai báo thư viện thẻ JSTL Function.
- **`pageContext.setAttribute("data", data);`**: Đặt biến `data` làm thuộc tính của trang.
- **`${fn:length(data)}`**: Sử dụng hàm `fn:length()` để tính độ dài của chuỗi `data`.

**Kết quả hiển thị:**

```
Chuỗi: Luv2code
Độ dài của chuỗi là: 8
```

### Ví Dụ 2: Chuyển Đổi Chữ Hoa Chữ Thường

Chúng ta sẽ sử dụng thẻ `fn:toUpperCase()` để chuyển một chuỗi sang chữ hoa.

**Mã nguồn JSP:**

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<!DOCTYPE html>
<html>
<head>
    <title>Chuyển Đổi Chữ Hoa</title>
</head>
<body>
    <%
        // Khai báo một biến chuỗi
        String data = "Luv2code";
        pageContext.setAttribute("data", data);
    %>

    <h2>Ví Dụ JSTL Function: Chuyển Đổi Chữ Hoa</h2>
    <p>Chuỗi gốc: ${data}</p>
    <p>Chuỗi chữ hoa: ${fn:toUpperCase(data)}</p>
</body>
</html>
```

**Giải thích:**

- **`${fn:toUpperCase(data)}`**: Chuyển chuỗi `data` thành chữ hoa.

**Kết quả hiển thị:**

```
Chuỗi gốc: Luv2code
Chuỗi chữ hoa: LUV2CODE
```

### Ví Dụ 3: Kiểm Tra Tiền Tố Chuỗi

Chúng ta sẽ sử dụng thẻ `fn:startsWith()` để kiểm tra xem chuỗi có bắt đầu bằng một chuỗi con cụ thể hay không.

**Mã nguồn JSP:**

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<!DOCTYPE html>
<html>
<head>
    <title>Kiểm Tra Tiền Tố</title>
</head>
<body>
    <%
        // Khai báo một biến chuỗi
        String data = "Luv2code";
        pageContext.setAttribute("data", data);
    %>

    <h2>Ví Dụ JSTL Function: Kiểm Tra Tiền Tố</h2>
    <p>Chuỗi gốc: ${data}</p>
    <p>Chuỗi có bắt đầu bằng 'Luv' không? ${fn:startsWith(data, 'Luv')}</p>
</body>
</html>
```

**Giải thích:**

- **`${fn:startsWith(data, 'Luv')}`**: Kiểm tra xem chuỗi `data` có bắt đầu bằng `Luv` hay không.

**Kết quả hiển thị:**

```
Chuỗi gốc: Luv2code
Chuỗi có bắt đầu bằng 'Luv' không? true
```

---

## 4. Lợi Ích Của Việc Sử Dụng Thẻ JSTL Function

- **Đơn Giản Và Dễ Dùng**: Các thẻ JSTL Function rất dễ sử dụng và giúp bạn thực hiện các thao tác với chuỗi mà không cần viết mã Java phức tạp.
- **Tăng Tính Đọc Hiểu**: Việc sử dụng thẻ JSTL giúp mã nguồn JSP của bạn trở nên dễ đọc và dễ hiểu hơn.
- **Giảm Thiểu Mã Java Trong JSP**: Tách biệt rõ ràng giữa phần logic và phần hiển thị, giúp trang JSP trở nên gọn gàng và tối ưu hơn.
- **Tích Hợp Tốt Với EL (Expression Language)**: Bạn có thể kết hợp các thẻ JSTL Function với EL để thực hiện các thao tác phức tạp hơn một cách dễ dàng.

---

## 5. Kết Luận

Thẻ JSTL Function cung cấp một bộ hàm tiện ích giúp bạn dễ dàng xử lý chuỗi và đối tượng trong JSP mà không cần phải viết mã Java phức tạp. Các hàm như `fn:length()`, `fn:toUpperCase()`, `fn:startsWith()`, và nhiều hàm khác giúp bạn dễ dàng thao tác với chuỗi, kiểm tra điều kiện, và chuyển đổi dữ liệu.

**Tài Nguyên Tham Khảo:**

- [Tài liệu JSTL Function Tag của Oracle](https://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html)
- [Thư viện JSTL Function Tag](https://javaee.github.io/jstl-api/javax/servlet/jsp/jstl/functions/package-summary.html)
