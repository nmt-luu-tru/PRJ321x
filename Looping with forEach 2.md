# Sử dụng JSTL `<c:forEach>` với Bảng HTML để Hiển Thị Danh Sách Sinh Viên

Trong bài viết này, chúng ta sẽ tìm hiểu cách sử dụng thẻ JSTL `<c:forEach>` cùng với bảng HTML để hiển thị thông tin từ một danh sách các đối tượng sinh viên. Chúng ta sẽ tạo một lớp `Student`, thiết lập dữ liệu mẫu, và sau đó sử dụng JSP để hiển thị thông tin trong một bảng HTML.

## Mục Lục

1. [Giới Thiệu](#giới-thiệu)
2. [Tạo Lớp Student](#tạo-lớp-student)
3. [Thiết Lập Dữ Liệu Mẫu](#thiết-lập-dữ-liệu-mẫu)
4. [Tạo Trang JSP Hiển Thị Bảng](#tạo-trang-jsp-hiển-thị-bảng)
5. [Chạy và Kiểm Tra Ứng Dụng](#chạy-và-kiểm-tra-ứng-dụng)
6. [Kết Luận](#kết-luận)

## Giới Thiệu

Mục tiêu của chúng ta là:

- Tạo một lớp `Student` với các thuộc tính `firstName`, `lastName`, và `goldCustomer`.
- Tạo một danh sách các sinh viên và thêm vào dữ liệu mẫu.
- Sử dụng thẻ JSTL `<c:forEach>` để lặp qua danh sách sinh viên và hiển thị thông tin trong một bảng HTML.

## Tạo Lớp Student

### Bước 1: Tạo Gói Mới

- Mở Eclipse và tạo một gói Java mới.
- Nhấp chuột phải vào thư mục **`src`** trong **`Java Resources`**, chọn **New** > **Package**.
- Đặt tên gói là `com.luv2code.jsp.tagdemo`.

### Bước 2: Tạo Lớp Student

- Nhấp chuột phải vào gói vừa tạo, chọn **New** > **Class**.
- Đặt tên lớp là `Student`.

**Mã nguồn của lớp `Student`:**

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

    // Getters and Setters
    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }
    
    public boolean isGoldCustomer() {
        return goldCustomer;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public void setGoldCustomer(boolean goldCustomer) {
        this.goldCustomer = goldCustomer;
    }
}
```

**Giải thích:**

- **Thuộc tính:**
  - `firstName`: Tên của sinh viên.
  - `lastName`: Họ của sinh viên.
  - `goldCustomer`: Biến boolean cho biết sinh viên có phải là khách hàng VIP hay không.

- **Constructor:**
  - Khởi tạo đối tượng `Student` với các giá trị cho `firstName`, `lastName`, và `goldCustomer`.

- **Getters and Setters:**
  - Cung cấp phương thức truy cập và cập nhật các thuộc tính của đối tượng.

## Thiết Lập Dữ Liệu Mẫu

Chúng ta sẽ tạo một danh sách các sinh viên và thêm dữ liệu mẫu vào danh sách này.

**Bước 1: Tạo Danh Sách Sinh Viên**

- Sử dụng `ArrayList` để lưu trữ danh sách sinh viên.

**Mã nguồn:**

```jsp
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.luv2code.jsp.tagdemo.Student" %>
<%
    // Tạo danh sách sinh viên
    ArrayList<Student> students = new ArrayList<>();

    // Thêm sinh viên vào danh sách
    students.add(new Student("John", "Doe", false));
    students.add(new Student("Maxwell", "Johnson", false));
    students.add(new Student("Mary", "Public", true));

    // Đặt danh sách vào phạm vi trang với tên "myStudents"
    pageContext.setAttribute("myStudents", students);
%>
```

**Giải thích:**

- **Import các gói cần thiết:**
  - `java.util.ArrayList` để sử dụng danh sách động.
  - `com.luv2code.jsp.tagdemo.Student` để sử dụng lớp `Student`.

- **Tạo và thêm dữ liệu:**
  - Tạo một `ArrayList` các đối tượng `Student`.
  - Thêm ba sinh viên với thông tin mẫu.

- **Đặt thuộc tính:**
  - Sử dụng `pageContext.setAttribute("myStudents", students);` để đặt danh sách sinh viên vào phạm vi trang với tên `myStudents`.

## Tạo Trang JSP Hiển Thị Bảng

Chúng ta sẽ tạo một trang JSP để hiển thị danh sách sinh viên trong một bảng HTML sử dụng thẻ `<c:forEach>`.

**Bước 1: Tạo Trang JSP Mới**

- Trong thư mục **`WebContent`**, nhấp chuột phải và chọn **New** > **JSP File**.
- Đặt tên tệp là `foreach-student-test.jsp`.

**Bước 2: Viết Mã JSP**

**Mã nguồn đầy đủ:**

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

    // Đặt danh sách vào phạm vi trang với tên "myStudents"
    pageContext.setAttribute("myStudents", students);
%>
<!DOCTYPE html>
<html>
<head>
    <title>Hiển Thị Danh Sách Sinh Viên</title>
</head>
<body>
    <h2>Danh Sách Sinh Viên:</h2>
    <table border="1">
        <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Gold Customer</th>
        </tr>
        <c:forEach var="tempStudent" items="${myStudents}">
            <tr>
                <td>${tempStudent.firstName}</td>
                <td>${tempStudent.lastName}</td>
                <td>${tempStudent.goldCustomer}</td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>
```

**Giải thích:**

- **Khai báo thư viện thẻ JSTL:**
  - Sử dụng `<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>` để khai báo thư viện thẻ JSTL Core với tiền tố `c`.

- **Thiết lập dữ liệu mẫu:**
  - Sử dụng scriptlet để tạo danh sách sinh viên và đặt vào phạm vi trang với tên `myStudents`.

- **Hiển thị bảng HTML:**
  - Tạo bảng HTML với các tiêu đề cột: `First Name`, `Last Name`, `Gold Customer`.
  - Sử dụng thẻ `<c:forEach>` để lặp qua danh sách `myStudents`.
    - **`var="tempStudent"`**: Biến tạm thời để lưu từng đối tượng `Student` trong mỗi lần lặp.
    - **`items="${myStudents}"`**: Danh sách sinh viên cần lặp qua.
  - Trong mỗi lần lặp, tạo một hàng bảng `<tr>` và hiển thị thông tin sinh viên:
    - **`${tempStudent.firstName}`**: Hiển thị `firstName` của sinh viên.
    - **`${tempStudent.lastName}`**: Hiển thị `lastName` của sinh viên.
    - **`${tempStudent.goldCustomer}`**: Hiển thị trạng thái `goldCustomer` của sinh viên (`true` hoặc `false`).

**Lưu ý về Expression Language (EL):**

- Khi sử dụng EL như `${tempStudent.firstName}`, JSP sẽ tự động gọi phương thức getter tương ứng, cụ thể là `getFirstName()`.
- Đối với thuộc tính boolean `goldCustomer`, EL sẽ gọi phương thức `isGoldCustomer()`.

## Chạy và Kiểm Tra Ứng Dụng

### Bước 1: Chạy Trang JSP

- Nhấp chuột phải vào tệp `foreach-student-test.jsp` và chọn **Run As** > **Run on Server**.
- Chọn máy chủ ứng dụng (ví dụ: Apache Tomcat) và nhấn **Finish**.

### Bước 2: Kiểm Tra Kết Quả

**Kết quả trên trình duyệt:**

| First Name | Last Name | Gold Customer |
|------------|-----------|---------------|
| John       | Doe       | false         |
| Maxwell    | Johnson   | false         |
| Mary       | Public    | true          |

**Giải thích:**

- Bảng HTML hiển thị danh sách sinh viên với thông tin tương ứng.
- Trạng thái **Gold Customer** hiển thị `true` hoặc `false` dựa trên giá trị của thuộc tính `goldCustomer`.

## Kết Luận

Trong bài viết này, chúng ta đã học cách sử dụng thẻ JSTL `<c:forEach>` để lặp qua một danh sách các đối tượng và hiển thị chúng trong một bảng HTML. Việc sử dụng JSTL giúp mã nguồn JSP trở nên sạch sẽ hơn, tách biệt giữa **business logic** và **presentation layer**, và tăng tính bảo trì cho ứng dụng.

**Lợi ích:**

- **Giảm thiểu mã Java trong JSP:** Chỉ cần sử dụng mã Java để thiết lập dữ liệu mẫu.
- **Tăng tính đọc hiểu:** Mã nguồn JSP rõ ràng, dễ đọc và dễ bảo trì.
- **Tái sử dụng:** Có thể áp dụng cùng một phương pháp cho các đối tượng và dữ liệu khác.

**Khuyến nghị:**

- Trong ứng dụng thực tế, dữ liệu thường được cung cấp bởi tầng Controller trong mô hình MVC, thay vì sử dụng scriptlet trong JSP.
- Nên tiếp tục tìm hiểu và sử dụng các thẻ khác trong JSTL để tối ưu hóa mã nguồn JSP.

**Chúc mừng bạn đã hoàn thành việc tìm hiểu về cách sử dụng thẻ `<c:forEach>` với bảng HTML trong JSP!**

Nếu bạn có bất kỳ câu hỏi hoặc cần hỗ trợ thêm, đừng ngần ngại liên hệ. Chúc bạn thành công trong việc phát triển ứng dụng web với JSP và JSTL.
