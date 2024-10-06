### MVC (Model-View-Controller) Với Servlets và JSP: Ví Dụ Chi Tiết Từng Bước

Trong video này, chúng ta sẽ đi sâu hơn vào cách kết hợp **Servlets** và **JSP** trong mô hình **MVC** (Model-View-Controller). Trước đó, chúng ta đã có cái nhìn tổng quan về cách luồng dữ liệu đi từ yêu cầu của người dùng (request) qua Controller (Servlet), tương tác với Model, và cuối cùng hiển thị kết quả thông qua View (JSP).

Tuy nhiên, trong các video trước, chúng ta chưa thực sự triển khai chi tiết việc sử dụng **Model**. Vì vậy, lần này chúng ta sẽ:
1. Tạo một lớp Helper (`StudentDataUtil.java`) để làm nhiệm vụ Model.
2. Tạo một lớp `Student.java` đại diện cho đối tượng sinh viên.
3. Viết một Controller Servlet để tương tác với Model.
4. Tạo View JSP hiển thị danh sách sinh viên dưới dạng bảng (table) HTML.

### 1. Giới Thiệu Và Luồng Hoạt Động

Trong bài học này, chúng ta sẽ tạo ra một ứng dụng quản lý danh sách sinh viên đơn giản. Ứng dụng sẽ thực hiện các chức năng cơ bản sau:

1. **Controller Servlet** sẽ xử lý các yêu cầu từ người dùng.
2. **Model (StudentDataUtil)** sẽ chứa danh sách sinh viên.
3. **View (JSP)** sẽ hiển thị danh sách sinh viên dưới dạng bảng HTML.

**Luồng dữ liệu hoạt động như sau:**
1. Người dùng gửi yêu cầu đến **Controller Servlet**.
2. **Controller** sẽ tương tác với **Model** (`StudentDataUtil`) để lấy danh sách sinh viên.
3. **Controller** chuyển tiếp (forward) dữ liệu đến **View JSP**.
4. **View** sẽ hiển thị dữ liệu dưới dạng bảng HTML và gửi kết quả về cho người dùng.

### 2. Bước 1: Tạo Lớp Model `Student.java`

Lớp này sẽ đại diện cho đối tượng sinh viên với các thuộc tính cơ bản như `firstName`, `lastName`, và `email`.

```java
package com.luv2code.mvc.model;

// Lớp đại diện cho đối tượng Sinh Viên
public class Student {
    private String firstName;
    private String lastName;
    private String email;

    // Constructor để khởi tạo đối tượng Student
    public Student(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    // Getter và Setter cho từng thuộc tính
    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

### 3. Bước 2: Tạo Lớp Helper `StudentDataUtil.java`

Lớp `StudentDataUtil` sẽ đảm nhận nhiệm vụ tạo danh sách sinh viên và trả về cho Controller khi cần.

```java
package com.luv2code.mvc.util;

import java.util.ArrayList;
import java.util.List;
import com.luv2code.mvc.model.Student;

public class StudentDataUtil {

    // Phương thức trả về danh sách sinh viên mẫu
    public static List<Student> getStudents() {
        // Tạo danh sách sinh viên mẫu
        List<Student> students = new ArrayList<>();

        students.add(new Student("John", "Doe", "john@example.com"));
        students.add(new Student("Mary", "Jane", "mary@example.com"));
        students.add(new Student("Peter", "Parker", "peter@example.com"));

        return students;
    }
}
```

**Giải Thích**:
- `getStudents()` sẽ tạo và trả về một danh sách sinh viên mẫu. Đây sẽ là dữ liệu mà Controller sẽ lấy để hiển thị trên View.

### 4. Bước 3: Tạo Controller Servlet (`MvcDemoServlet.java`)

Servlet này sẽ đóng vai trò là Controller, tương tác với Model để lấy dữ liệu và chuyển tiếp dữ liệu đó đến View JSP.

```java
package com.luv2code.mvc;

import java.io.IOException;
import java.util.List;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.luv2code.mvc.model.Student;
import com.luv2code.mvc.util.StudentDataUtil;

// Định nghĩa URL mapping cho Servlet Controller là "/MvcDemoServlet"
@WebServlet("/MvcDemoServlet")
public class MvcDemoServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        // Bước 1: Lấy danh sách sinh viên từ lớp helper (Model)
        List<Student> students = StudentDataUtil.getStudents();

        // Bước 2: Đặt danh sách vào request với tên "student_list"
        request.setAttribute("student_list", students);

        // Bước 3: Chuyển tiếp yêu cầu đến View JSP ("view_students.jsp")
        RequestDispatcher dispatcher = request.getRequestDispatcher("/view_students.jsp");
        dispatcher.forward(request, response);
    }
}
```

**Giải Thích**:
- Servlet `MvcDemoServlet` tương tác với Model (`StudentDataUtil`) để lấy danh sách sinh viên và chuyển tiếp dữ liệu này đến `view_students.jsp` để hiển thị.

### 5. Bước 4: Tạo Trang View (`view_students.jsp`)

Trang JSP này sẽ hiển thị danh sách sinh viên dưới dạng bảng HTML.

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
    <title>Danh Sách Sinh Viên</title>
</head>
<body>
    <h2>Danh Sách Sinh Viên</h2>
    <table border="1">
        <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Email</th>
        </tr>
        <!-- Sử dụng JSTL để lặp qua danh sách sinh viên và hiển thị -->
        <c:forEach var="student" items="${student_list}">
            <tr>
                <td>${student.firstName}</td>
                <td>${student.lastName}</td>
                <td>${student.email}</td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>
```

**Giải Thích**:
- `c:forEach var="student" items="${student_list}"` dùng để lặp qua danh sách `student_list` (do Servlet gửi tới) và hiển thị từng phần tử dưới dạng bảng HTML (`<table>`).



### 6. Bước 5: Tạo Trang `index.html` Chứa Liên Kết Đến `Servlet`
1. Tạo một tệp HTML mới trong thư mục `WebContent`:
   - Nhấp chuột phải vào thư mục `WebContent` > Chọn `New` > `File`.
   - Đặt tên tệp là `index.html` và nhấp vào `Finish`.
2. Cập nhật nội dung của `index.html` như sau:

```html
<html>
<head>
    <title>Trang Chủ</title>
</head>
<body>
    <h2>Chào mừng đến với Ứng Dụng MVC Demo</h2>
    <!-- Tạo liên kết đến Servlet MvcDemoServletTwo -->
    <a href="MvcDemoServlet">View Students with MVC</a>
</body>
</html>
```

**Giải Thích:**
- Tạo một liên kết đơn giản `a href="MvcDemoServlet"` trỏ đến `Servlet` `MvcDemoServlet.java`.
- Khi người dùng nhấp vào liên kết, yêu cầu sẽ được gửi đến `Servlet` và `Servlet` sẽ xử lý yêu cầu, chuyển tiếp dữ liệu đến `view_students.jsp` để hiển thị.

### 7. Bước 6: Kiểm Tra Hoạt Động Của Ứng Dụng
1. **Chạy máy chủ (Server)**: Đảm bảo rằng máy chủ (Tomcat hoặc bất kỳ máy chủ nào bạn sử dụng) đã được khởi động.
2. **Truy cập `index.html`**:
   - Nhấp chuột phải vào `index.html` > Chọn `Run As` > `Run on Server`.
   - Trình duyệt sẽ mở `index.html` với một liên kết “View Students with MVC”.
3. **Kiểm tra kết quả**:
   - Nhấp vào liên kết “View Students with MVC”.
   - Bạn sẽ thấy trang `view_students.jsp` hiển thị danh sách sinh viên dưới dạng bảng HTML với các cột `First Name`, `Last Name` và `Email`.

### Kết Luận
Với những bước trên, chúng ta đã hoàn thành ứng dụng MVC đầy đủ: 
- `Servlet` (`MvcDemoServlet`) xử lý logic và tương tác với dữ liệu.
- `StudentDataUtil` cung cấp dữ liệu (Model).
- `view_students.jsp` hiển thị dữ liệu cho người dùng (View).
- `index.html` giúp điều hướng và trải nghiệm ứng dụng dễ dàng hơn.



### 8. Lưu Ý Quan Trọng Khi Sử Dụng MVC Với Servlets và JSP
1. **Không viết mã HTML trong Servlet**: Việc sử dụng `out.println` để tạo mã HTML trong Servlet sẽ làm mã nguồn phức tạp và khó bảo trì.
2. **Tránh viết mã Java trong JSP**: Sử dụng JSTL để mã nguồn trở nên gọn gàng và dễ hiểu hơn.
3. **Đảm bảo đường dẫn trong `RequestDispatcher` chính xác**: Đảm bảo rằng đường dẫn JSP trùng khớp với cấu hình trong Servlet.
