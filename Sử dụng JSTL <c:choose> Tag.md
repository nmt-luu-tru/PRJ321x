# Sử dụng Thẻ `<c:choose>` trong JSTL

Trong bài viết này, chúng ta sẽ khám phá cách sử dụng thẻ `<c:choose>` từ thư viện JSTL (JavaServer Pages Standard Tag Library) Core. Thẻ `<c:choose>` cho phép bạn thực hiện xử lý điều kiện tương tự như cấu trúc switch-case trong Java. Nó cung cấp một cách tiếp cận sạch sẽ và dễ đọc hơn để xử lý nhiều nhánh điều kiện trong các trang JSP của bạn.

## Mục Lục

1. Giới Thiệu Về Thẻ `<c:choose>`
2. Cú Pháp và Cấu Trúc
3. Ví Dụ Thực Tế
   - Bước 1: Chuẩn Bị Dữ Liệu
   - Bước 2: Sử Dụng Thẻ `<c:choose>`
   - Bước 3: Kiểm Tra Ứng Dụng
4. Lợi Ích Khi Sử Dụng `<c:choose>`
5. Kết Luận

---

## Giới Thiệu Về Thẻ `<c:choose>`

Thẻ `<c:choose>` là một phần của thư viện JSTL Core và hoạt động tương tự như câu lệnh `switch` trong Java. Nó cho phép bạn đánh giá nhiều điều kiện bằng cách sử dụng các thẻ `<c:when>` và cung cấp một trường hợp mặc định với `<c:otherwise>`. Thẻ này giúp cải thiện tính đọc hiểu và khả năng bảo trì bằng cách cấu trúc logic điều kiện của bạn một cách rõ ràng và có tổ chức.

---

## Cú Pháp và Cấu Trúc

Cấu trúc cơ bản của thẻ `<c:choose>` như sau:

```jsp
<c:choose>
    <c:when test="${condition1}">
        <!-- Nội dung hiển thị nếu condition1 đúng -->
    </c:when>
    <c:when test="${condition2}">
        <!-- Nội dung hiển thị nếu condition2 đúng -->
    </c:when>
    <!-- Thêm nhiều khối <c:when> nếu cần -->
    <c:otherwise>
        <!-- Nội dung hiển thị nếu không có điều kiện nào ở trên đúng -->
    </c:otherwise>
</c:choose>
```

- **`<c:choose>`**: Thẻ bao quanh cho logic điều kiện.
- **`<c:when>`**: Đại diện cho một điều kiện cần kiểm tra. Bạn có thể có nhiều thẻ `<c:when>` bên trong `<c:choose>`.
- **Thuộc tính `test`**: Điều kiện cần đánh giá, thường sử dụng Expression Language (EL).
- **`<c:otherwise>`**: Không bắt buộc. Thực thi nếu không có điều kiện `<c:when>` nào đúng.

---

## Ví Dụ Thực Tế

Chúng ta sẽ xây dựng dựa trên ví dụ trước đó, nơi chúng ta hiển thị danh sách sinh viên. Mỗi sinh viên có `firstName`, `lastName`, và trạng thái `goldCustomer` cho biết họ có phải là khách hàng VIP hay không. Chúng ta sẽ sử dụng thẻ `<c:choose>` để hiển thị các thông báo khác nhau dựa trên trạng thái `goldCustomer`.

### Bước 1: Chuẩn Bị Dữ Liệu

Đầu tiên, đảm bảo bạn có lớp `Student` với các thuộc tính và phương thức cần thiết.

**Student.java**

```java
package com.luv2code.jsp.tagdemo;

public class Student {
    private String firstName;
    private String lastName;
    private boolean goldCustomer;

    // Constructor
    public Student(String firstName, String lastName, boolean goldCustomer) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.goldCustomer = goldCustomer;
    }

    // Getters
    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }
    
    public boolean isGoldCustomer() {
        return goldCustomer;
    }

    // Setters (nếu cần)
    // ...
}
```

Trong tệp JSP của bạn, nhập các lớp cần thiết và thiết lập dữ liệu:

**choose-student-test.jsp**

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

    // Đặt danh sách làm thuộc tính của trang
    pageContext.setAttribute("myStudents", students);
%>
```

### Bước 2: Sử Dụng Thẻ `<c:choose>`

Chúng ta sẽ sử dụng thẻ `<c:choose>` để hiển thị "Special Discount" nếu sinh viên là khách hàng VIP và một thông báo khác nếu không.

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Khuyến Mãi Sinh Viên</title>
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
                    <c:choose>
                        <c:when test="${tempStudent.goldCustomer}">
                            Special Discount
                        </c:when>
                        <c:otherwise>
                            Không có ưu đãi
                        </c:otherwise>
                    </c:choose>
                </td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>
```

**Giải thích:**

- **`<c:choose>`**: Bắt đầu khối logic điều kiện.
- **`<c:when test="${tempStudent.goldCustomer}">`**: Kiểm tra nếu `goldCustomer` là `true`.
    - Nếu đúng, hiển thị "Special Discount".
- **`<c:otherwise>`**: Thực thi nếu không có điều kiện `<c:when>` nào đúng.
    - Hiển thị "Không có ưu đãi".

### Bước 3: Kiểm Tra Ứng Dụng

Chạy trang JSP trên máy chủ của bạn. Kết quả mong đợi là:

| First Name | Last Name | Status           |
|------------|-----------|------------------|
| John       | Doe       | Không có ưu đãi  |
| Maxwell    | Johnson   | Không có ưu đãi  |
| Mary       | Public    | Special Discount |

**Giải thích:**

- Sinh viên không phải khách hàng VIP nhận thông báo "Không có ưu đãi".
- Khách hàng VIP nhận thông báo "Special Discount".

---

## Lợi Ích Khi Sử Dụng `<c:choose>`

- **Đơn Giản Hóa Logic Điều Kiện**: Cung cấp cấu trúc rõ ràng cho nhiều nhánh điều kiện.
- **Tăng Tính Đọc Hiểu**: Mã nguồn dễ đọc hơn so với việc lồng nhiều thẻ `<c:if>`.
- **Tránh Các Giải Pháp Tạm Thời**: Loại bỏ nhu cầu về các giải pháp tạm thời khi xử lý điều kiện "else".
- **Dễ Dàng Bảo Trì**: Thêm hoặc sửa đổi các điều kiện một cách đơn giản.

---

## Kết Luận

Thẻ `<c:choose>` trong JSTL là một công cụ mạnh mẽ để xử lý logic điều kiện trong các trang JSP. Nó cho phép bạn triển khai các điều kiện nhiều nhánh tương tự như cấu trúc switch-case trong Java, làm cho mã JSP của bạn sạch sẽ và dễ bảo trì hơn.

Bằng cách sử dụng `<c:choose>`, `<c:when>`, và `<c:otherwise>`, bạn có thể quản lý hiệu quả việc hiển thị có điều kiện trong ứng dụng web của mình.

---

**Tài Nguyên Tham Khảo:**

- [Tài liệu JSTL Core Tag](https://javaee.github.io/jstl-api/javax/servlet/jsp/jstl/core/package-summary.html)
- [Tài liệu chính thức của JSTL từ Oracle](https://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html)


---

Bài hướng dẫn này cung cấp giải thích từng bước về cách sử dụng thẻ `<c:choose>` trong các ứng dụng JSP của bạn, bao gồm các ví dụ mã và giải thích để giúp bạn hiểu và triển khai tính năng mạnh mẽ này của JSTL.
