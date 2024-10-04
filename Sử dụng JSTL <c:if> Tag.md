
# Sử dụng JSTL `<c:if>` Tag

Trong bài viết này, chúng ta sẽ tìm hiểu về thẻ `<c:if>` trong thư viện JSTL Core của JSP. Thẻ `<c:if>` được sử dụng để thực hiện các kiểm tra điều kiện trong trang JSP một cách đơn giản và hiệu quả, giúp giảm thiểu việc sử dụng mã Java trong trang và làm cho mã nguồn sạch sẽ hơn.

## Mục Lục

1. Giới Thiệu Về Thẻ `<c:if>`
2. Sử Dụng Thẻ `<c:if>`
   - Cú Pháp Cơ Bản
   - Ví Dụ Cơ Bản
3. Ứng Dụng Thực Tế: Hiển Thị "Special Discount" Cho Khách Hàng VIP
   - Bước 1: Chuẩn Bị Dữ Liệu
   - Bước 2: Sử Dụng Thẻ `<c:if>`
   - Bước 3: Kiểm Tra Kết Quả
4. Xử Lý Điều Kiện "else" Với `<c:if>`
5. Kết Luận

---

## 1. Giới Thiệu Về Thẻ `<c:if>`

Thẻ `<c:if>` trong JSTL được sử dụng để thực hiện kiểm tra điều kiện. Nếu điều kiện được đánh giá là **`true`**, nội dung bên trong thẻ sẽ được hiển thị; nếu **`false`**, nội dung sẽ bị bỏ qua.

**Cú pháp tổng quát:**

```jsp
<c:if test="${condition}">
    <!-- Nội dung sẽ hiển thị nếu điều kiện đúng -->
</c:if>
```

- **`test`**: Biểu thức điều kiện cần kiểm tra, sử dụng Expression Language (EL) của JSP.
- **Nội dung bên trong thẻ `<c:if>`** sẽ được hiển thị nếu biểu thức **`test`** đánh giá là **`true`**.

---

## 2. Sử Dụng Thẻ `<c:if>`

### Cú Pháp Cơ Bản

```jsp
<c:if test="${expression}">
    <!-- Nội dung hiển thị nếu biểu thức là true -->
</c:if>
```

- **`${expression}`**: Biểu thức logic trả về giá trị boolean (`true` hoặc `false`).

### Ví Dụ Cơ Bản

Giả sử chúng ta có một biến `isLoggedIn` để kiểm tra người dùng đã đăng nhập hay chưa.

```jsp
<c:if test="${isLoggedIn}">
    <p>Chào mừng bạn trở lại!</p>
</c:if>
```

- Nếu `${isLoggedIn}` là `true`, thông báo chào mừng sẽ được hiển thị.

---

## 3. Ứng Dụng Thực Tế: Hiển Thị "Special Discount" Cho Khách Hàng VIP

Chúng ta sẽ thực hiện một ví dụ hiển thị "Special Discount" (Giảm giá đặc biệt) nếu sinh viên là khách hàng VIP (`goldCustomer`).

### Bước 1: Chuẩn Bị Dữ Liệu

Sử dụng lớp `Student` từ ví dụ trước, với thuộc tính `goldCustomer` xác định sinh viên có phải là khách hàng VIP hay không.

**Mã nguồn:**

```java
// Student.java
package com.luv2code.jsp.tagdemo;

public class Student {
    private String firstName;
    private String lastName;
    private boolean goldCustomer;

    // Constructor, getters, and setters
    // ...
}
```

**Thiết lập dữ liệu trong trang JSP:**

```jsp
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.luv2code.jsp.tagdemo.Student" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%
    // Tạo danh sách sinh viên
    ArrayList<Student> students = new ArrayList<>();

    // Thêm sinh viên vào danh sách
    students.add(new Student("John", "Doe", false));
    students.add(new Student("Maxwell", "Johnson", false));
    students.add(new Student("Mary", "Public", true));

    // Đặt danh sách vào phạm vi trang
    pageContext.setAttribute("myStudents", students);
%>
```

### Bước 2: Sử Dụng Thẻ `<c:if>`

Chúng ta sẽ sử dụng thẻ `<c:if>` để kiểm tra giá trị của `goldCustomer` và hiển thị "Special Discount" nếu điều kiện đúng.

**Mã nguồn JSP:**

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Hiển Thị Special Discount</title>
</head>
<body>
    <h2>Danh Sách Sinh Viên:</h2>
    <table border="1">
        <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Status</th>
        </tr>
        <c:forEach var="tempStudent" items="${myStudents}">
            <tr>
                <td>${tempStudent.firstName}</td>
                <td>${tempStudent.lastName}</td>
                <td>
                    <c:if test="${tempStudent.goldCustomer}">
                        Special Discount
                    </c:if>
                </td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>
```

**Giải thích:**

- **Vòng lặp `<c:forEach>`**: Lặp qua danh sách `myStudents`.
- **Thẻ `<c:if>` bên trong `<td>`**: Kiểm tra `tempStudent.goldCustomer`.
  - Nếu `goldCustomer` là `true`, hiển thị "Special Discount".
  - Nếu `goldCustomer` là `false`, không hiển thị gì (ô trống).

### Bước 3: Kiểm Tra Kết Quả

**Kết quả khi chạy trang JSP:**

| First Name | Last Name | Status           |
|------------|-----------|------------------|
| John       | Doe       |                  |
| Maxwell    | Johnson   |                  |
| Mary       | Public    | Special Discount |

**Giải thích:**

- Chỉ sinh viên Mary Public là khách hàng VIP (`goldCustomer` là `true`), nên cột **Status** hiển thị "Special Discount".
- Các sinh viên khác không phải khách hàng VIP, nên cột **Status** để trống.

---

## 4. Xử Lý Điều Kiện "else" Với `<c:if>`

Thẻ `<c:if>` không hỗ trợ trực tiếp cấu trúc `else`. Tuy nhiên, chúng ta có thể sử dụng một thẻ `<c:if>` khác với điều kiện ngược lại để mô phỏng hành vi của `else`.

**Cách thực hiện:**

```jsp
<td>
    <c:if test="${tempStudent.goldCustomer}">
        Special Discount
    </c:if>
    <c:if test="${not tempStudent.goldCustomer}">
        -
    </c:if>
</td>
```

**Hoặc sử dụng cú pháp rút gọn:**

```jsp
<td>
    <c:if test="${tempStudent.goldCustomer}">
        Special Discount
    </c:if>
    <c:if test="${!tempStudent.goldCustomer}">
        -
    </c:if>
</td>
```

**Giải thích:**

- **`${not tempStudent.goldCustomer}`** hoặc `${!tempStudent.goldCustomer}`: Kiểm tra điều kiện ngược lại (khi `goldCustomer` là `false`).
- Hiển thị dấu "-" nếu sinh viên không phải là khách hàng VIP.

**Cập nhật mã nguồn JSP:**

```jsp
<td>
    <c:if test="${tempStudent.goldCustomer}">
        Special Discount
    </c:if>
    <c:if test="${!tempStudent.goldCustomer}">
        -
    </c:if>
</td>
```

**Kết quả sau khi cập nhật:**

| First Name | Last Name | Status           |
|------------|-----------|------------------|
| John       | Doe       | -                |
| Maxwell    | Johnson   | -                |
| Mary       | Public    | Special Discount |

**Giải thích:**

- Bây giờ, nếu sinh viên không phải khách hàng VIP, cột **Status** sẽ hiển thị dấu "-".
- Sinh viên là khách hàng VIP vẫn hiển thị "Special Discount".

---

## 5. Kết Luận

Thẻ `<c:if>` trong JSTL là một công cụ mạnh mẽ giúp thực hiện các kiểm tra điều kiện trong trang JSP một cách đơn giản và hiệu quả. Mặc dù không hỗ trợ trực tiếp cấu trúc `else`, chúng ta có thể sử dụng các thẻ `<c:if>` bổ sung với điều kiện ngược lại để đạt được mục tiêu tương tự.

**Lợi ích của việc sử dụng `<c:if>`:**

- **Giảm thiểu mã Java trong JSP**: Tăng tính đọc hiểu và bảo trì.
- **Tách biệt logic và hiển thị**: Giúp mã nguồn sạch sẽ hơn.
- **Tích hợp tốt với EL và JSTL**: Tận dụng sức mạnh của Expression Language.

**Khuyến nghị:**

- **Tránh sử dụng scriptlet trong JSP**: Thay vào đó, sử dụng EL và JSTL.
- **Tìm hiểu thêm về các thẻ JSTL khác**: Như `<c:choose>`, `<c:when>`, và `<c:otherwise>` để xử lý các điều kiện phức tạp hơn.

**Tài nguyên tham khảo:**

- [Hướng dẫn JSTL của Oracle](https://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html)
- [JSTL Core Tag Reference](https://javaee.github.io/jstl-api/javax/servlet/jsp/jstl/core/package-summary.html)

---

**Chúc mừng bạn đã hoàn thành việc tìm hiểu về thẻ `<c:if>` trong JSTL!** Việc áp dụng đúng cách thẻ này sẽ giúp bạn xây dựng các trang JSP linh hoạt và hiệu quả hơn.
