# Hướng Dẫn Sử Dụng Servlet Để Đọc Dữ Liệu Từ Biểu Mẫu HTML

Trong bài viết này, chúng ta sẽ tìm hiểu cách sử dụng servlet để đọc dữ liệu từ một biểu mẫu HTML đơn giản. Chúng ta sẽ tạo một biểu mẫu HTML để người dùng nhập thông tin học sinh (họ và tên), sau đó gửi thông tin này đến servlet để xử lý và hiển thị kết quả trên trình duyệt.

## Mục Lục

1. Giới Thiệu
2. Tạo Biểu Mẫu HTML (HTML Form)
3. Tạo Servlet Để Xử Lý Biểu Mẫu
   - Bước 1: Định Nghĩa Servlet
   - Bước 2: Ghi Đè Phương Thức `doGet`
4. Chạy Và Kiểm Tra Servlet
5. Sự Khác Biệt Giữa `GET` và `POST` trong Servlet
6. Kết Luận

---

## 1. Giới Thiệu

**Servlet** là một lớp Java chạy trên máy chủ (server) và có nhiệm vụ xử lý các yêu cầu (request) từ trình duyệt (client). Trong ví dụ này, chúng ta sẽ xây dựng một ứng dụng web đơn giản với hai thành phần chính:

1. **`student-form.html`**: Biểu mẫu HTML để thu thập thông tin học sinh (họ và tên).
2. **`StudentServlet.java`**: Servlet xử lý yêu cầu từ biểu mẫu HTML và hiển thị kết quả.

Khi người dùng nhập thông tin vào biểu mẫu và nhấn nút "Submit", dữ liệu sẽ được gửi đến servlet. Servlet sẽ đọc dữ liệu từ biểu mẫu và trả về một trang HTML xác nhận thông tin đã nhận được.

---

## 2. Tạo Biểu Mẫu HTML (HTML Form)

**Mã nguồn của biểu mẫu HTML (`student-form.html`):**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Biểu Mẫu Nhập Thông Tin Học Sinh</title>
</head>
<body>
    <h2>Nhập Thông Tin Học Sinh</h2>
    <form action="StudentServlet" method="GET">
        <label for="firstName">Họ:</label>
        <input type="text" id="firstName" name="firstName"><br><br>
        
        <label for="lastName">Tên:</label>
        <input type="text" id="lastName" name="lastName"><br><br>
        
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

**Giải thích:**

- **`<form action="StudentServlet" method="GET">`**: Định nghĩa biểu mẫu sẽ gửi dữ liệu đến servlet `StudentServlet` bằng phương thức `GET`.
- **`<input type="text" id="firstName" name="firstName">`**: Trường nhập liệu cho "Họ" của học sinh.
- **`<input type="text" id="lastName" name="lastName">`**: Trường nhập liệu cho "Tên" của học sinh.
- **`<input type="submit" value="Submit">`**: Nút gửi dữ liệu đến servlet.

Biểu mẫu này sẽ gửi dữ liệu về servlet `StudentServlet` và gọi phương thức `doGet` của servlet đó để xử lý yêu cầu.

---

## 3. Tạo Servlet Để Xử Lý Biểu Mẫu

### Bước 1: Định Nghĩa Servlet

**Mã nguồn của `StudentServlet.java`:**

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// Định nghĩa servlet với đường dẫn /StudentServlet
@WebServlet("/StudentServlet")
public class StudentServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // Ghi đè phương thức doGet để xử lý yêu cầu GET từ biểu mẫu
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        // Thiết lập loại nội dung trả về là HTML
        response.setContentType("text/html");

        // Lấy đối tượng PrintWriter để ghi HTML trả về cho trình duyệt
        PrintWriter out = response.getWriter();

        // Đọc dữ liệu từ biểu mẫu
        String firstName = request.getParameter("firstName");
        String lastName = request.getParameter("lastName");

        // Tạo nội dung HTML trả về
        out.println("<html><body>");
        out.println("<h2>Thông Tin Học Sinh Đã Nhận</h2>");
        out.println("Họ: " + firstName + "<br>");
        out.println("Tên: " + lastName + "<br>");
        out.println("</body></html>");
    }
}
```

**Giải thích:**

- **`@WebServlet("/StudentServlet")`**: Định nghĩa đường dẫn để truy cập servlet.
- **`doGet` method**: Phương thức xử lý các yêu cầu `GET` từ biểu mẫu HTML.
- **`request.getParameter("firstName")`**: Đọc dữ liệu từ trường "Họ" của biểu mẫu.
- **`response.getWriter()`**: Lấy đối tượng `PrintWriter` để ghi nội dung HTML trả về.

### Bước 2: Ghi Đè Phương Thức `doGet`

Trong phương thức `doGet` của servlet:

1. **Thiết lập loại nội dung trả về**:
   ```java
   response.setContentType("text/html");
   ```
   Điều này thông báo cho trình duyệt biết dữ liệu trả về là HTML.

2. **Lấy đối tượng `PrintWriter` để gửi dữ liệu HTML về trình duyệt**:
   ```java
   PrintWriter out = response.getWriter();
   ```

3. **Đọc dữ liệu từ biểu mẫu**:
   ```java
   String firstName = request.getParameter("firstName");
   String lastName = request.getParameter("lastName");
   ```

4. **Tạo nội dung HTML**:
   ```java
   out.println("<html><body>");
   out.println("<h2>Thông Tin Học Sinh Đã Nhận</h2>");
   out.println("Họ: " + firstName + "<br>");
   out.println("Tên: " + lastName + "<br>");
   out.println("</body></html>");
   ```

---

## 4. Chạy Và Kiểm Tra Servlet

1. Triển khai servlet và biểu mẫu trên máy chủ (ví dụ: Apache Tomcat).
2. Truy cập biểu mẫu HTML tại `http://localhost:8080/student-form.html`.
3. Nhập thông tin và nhấn nút "Submit".
4. Servlet sẽ xử lý yêu cầu và hiển thị kết quả.

**Kết quả mong đợi:**

```
Thông Tin Học Sinh Đã Nhận
Họ: John
Tên: Doe
```

---

## 5. Sự Khác Biệt Giữa `GET` và `POST` trong Servlet

### Phương Thức `GET`
- **Dữ liệu**: Gửi dữ liệu dưới dạng chuỗi truy vấn (query string) ở URL.
- **Hạn chế**: Giới hạn kích thước dữ liệu (~1000 ký tự).
- **An ninh**: Không an toàn cho dữ liệu nhạy cảm do dữ liệu hiển thị trong URL.
- **Ứng dụng**: Phù hợp cho các yêu cầu không thay đổi dữ liệu, như tìm kiếm.

### Phương Thức `POST`
- **Dữ liệu**: Gửi dữ liệu trong phần thân (body) của HTTP request.
- **Hạn chế**: Không giới hạn kích thước dữ liệu.
- **An ninh**: An toàn hơn vì dữ liệu không hiển thị trong URL.
- **Ứng dụng**: Phù hợp cho các biểu mẫu có dữ liệu nhạy cảm, như thông tin đăng nhập hoặc đăng ký.

---

## 6. Kết Luận

Trong bài viết này, chúng ta đã tìm hiểu cách sử dụng servlet để đọc dữ liệu từ biểu mẫu HTML và hiển thị kết quả trên trình duyệt. Đồng thời, chúng ta đã so sánh sự khác biệt giữa phương thức `GET` và `POST` trong servlet, giúp bạn lựa chọn phương thức phù hợp khi xây dựng ứng dụng web.

**Tài Nguyên Tham Khảo:**

- [Tài liệu Java Servlet của Oracle](https://docs.oracle.com/javaee/7/tutorial/servlets.htm)
- [Servlet API Documentation](https://javaee.github.io/javaee-spec/javadocs/javax/servlet/package-summary.html)

Chúc bạn thành công trong việc lập trình ứng dụng web với Java Servlet!
