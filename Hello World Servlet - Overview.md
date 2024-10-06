# Hướng Dẫn Về Servlets Trong Java: Tổng Quan, Cách Thức Hoạt Động và Triển Khai

Trong bài viết này, chúng ta sẽ tìm hiểu về khái niệm **servlet** trong Java và cách tạo một servlet cơ bản để hiển thị thông điệp "Hello World". Chúng ta cũng sẽ so sánh sự khác biệt giữa **Servlet** và **JSP** để hiểu rõ hơn cách áp dụng chúng trong phát triển ứng dụng web.

## Mục Lục

1. [Servlet Là Gì?](#servlet-la-gi)
2. [Cách Thức Hoạt Động Của Servlet](#cach-thuc-hoat-dong)
3. [Tạo Một Servlet Đơn Giản](#tao-servlet)
   - [Bước 1: Định Nghĩa Servlet](#buoc-1)
   - [Bước 2: Cấu Hình `doGet` Method](#buoc-2)
   - [Bước 3: Chạy Và Kiểm Tra Servlet](#buoc-3)
4. [So Sánh Giữa Servlet và JSP](#so-sanh)
5. [Kết Luận](#ket-luan)

---

## 1. Servlet Là Gì?

Servlet là một lớp Java được chạy trên máy chủ (server) và có nhiệm vụ xử lý các yêu cầu HTTP từ trình duyệt (client). Khi có một yêu cầu từ trình duyệt gửi đến, servlet sẽ xử lý yêu cầu đó và trả lại kết quả (response) dưới dạng HTML hoặc JSON về phía trình duyệt.

### Các chức năng chính của servlet:

- **Đọc dữ liệu từ form HTML**: Ví dụ, khi bạn điền thông tin vào form trên trang web và nhấn submit, servlet có thể đọc được các dữ liệu đó.
- **Quản lý phiên làm việc (session)**: Giữ trạng thái đăng nhập của người dùng.
- **Sử dụng cookies**: Lưu trữ và đọc thông tin từ cookie của người dùng.

Servlet và JSP (JavaServer Pages) có sự tương đồng ở một số điểm, tuy nhiên chúng khác nhau về cách thức hoạt động và triển khai. Chúng ta sẽ đi sâu vào sự khác biệt này ở phần sau của bài viết.

---

## 2. Cách Thức Hoạt Động Của Servlet

Servlet hoạt động theo cơ chế sau:

1. **Trình duyệt (client)** gửi một yêu cầu HTTP (request) đến máy chủ (server).
2. **Servlet** trên máy chủ xử lý yêu cầu đó.
3. Servlet tạo ra nội dung HTML hoặc JSON.
4. Máy chủ gửi nội dung HTML hoặc JSON về lại trình duyệt dưới dạng **HTTP response**.

Quá trình này diễn ra nhanh chóng và giúp tạo ra các trang web động dựa trên dữ liệu đầu vào hoặc trạng thái người dùng.

---

## 3. Tạo Một Servlet Đơn Giản

Trong phần này, chúng ta sẽ tạo một servlet đơn giản hiển thị thông điệp "Hello World".

### Bước 1: Định Nghĩa Servlet

**Mã nguồn:**

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// Định nghĩa servlet với đường dẫn /HelloWorldServlet
@WebServlet("/HelloWorldServlet")
public class HelloWorldServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // Phương thức doGet xử lý các yêu cầu GET từ trình duyệt
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        // Thiết lập kiểu nội dung trả về cho trình duyệt
        response.setContentType("text/html");

        // Lấy đối tượng PrintWriter để xuất nội dung ra trình duyệt
        PrintWriter out = response.getWriter();

        // Tạo nội dung HTML
        out.println("<html><body>");
        out.println("<h2>Hello World</h2>");
        out.println("<hr>");
        out.println("Time on the server is: " + new java.util.Date());
        out.println("</body></html>");
    }
}
```

**Giải thích:**

- **`@WebServlet("/HelloWorldServlet")`**: Annotation dùng để cấu hình đường dẫn của servlet. Khi bạn truy cập `http://localhost:8080/HelloWorldServlet`, servlet này sẽ được gọi.
- **`extends HttpServlet`**: Lớp `HelloWorldServlet` kế thừa từ `HttpServlet` để có thể xử lý các yêu cầu HTTP.
- **`doGet` method**: Đây là phương thức được ghi đè (override) để xử lý các yêu cầu GET từ trình duyệt.

### Bước 2: Cấu Hình `doGet` Method

Trong phương thức `doGet`:

1. **Thiết lập loại nội dung trả về**:
   ```java
   response.setContentType("text/html");
   ```
   Điều này cho trình duyệt biết rằng dữ liệu trả về là HTML.

2. **Tạo đối tượng `PrintWriter` để gửi dữ liệu HTML về trình duyệt**:
   ```java
   PrintWriter out = response.getWriter();
   ```

3. **Tạo nội dung HTML**:
   ```java
   out.println("<html><body>");
   out.println("<h2>Hello World</h2>");
   out.println("<hr>");
   out.println("Time on the server is: " + new java.util.Date());
   out.println("</body></html>");
   ```

### Bước 3: Chạy Và Kiểm Tra Servlet

1. Triển khai servlet trên máy chủ (server) như Apache Tomcat.
2. Truy cập đường dẫn `http://localhost:8080/HelloWorldServlet` trên trình duyệt.
3. Kết quả mong đợi:

```
Hello World
---------------------------------------------------
Time on the server is: <Thời gian hiện tại trên máy chủ>
```

---

## 4. So Sánh Giữa Servlet và JSP

### Servlet

- Là một lớp Java xử lý logic trên máy chủ.
- Phù hợp cho các thao tác xử lý logic phức tạp.
- Để tạo HTML, bạn phải dùng `out.println()` cho từng dòng HTML.

### JSP (JavaServer Pages)

- Là một trang HTML với mã Java nhúng.
- Thường được sử dụng để hiển thị dữ liệu (view) hơn là xử lý logic.
- Có thể dễ dàng nhúng mã Java bên trong các thẻ HTML mà không cần `out.println()`.

### Khi Nào Sử Dụng Servlet và Khi Nào Sử Dụng JSP?

- **Servlet**: Sử dụng khi bạn cần xử lý logic phức tạp, tương tác với cơ sở dữ liệu hoặc quản lý session và cookies.
- **JSP**: Sử dụng cho việc hiển thị giao diện và tạo trang web tĩnh.

Một trong những phương pháp hay nhất khi phát triển ứng dụng web là sử dụng mô hình **Model-View-Controller (MVC)**, nơi servlet được sử dụng để xử lý logic (Model) và JSP để hiển thị kết quả (View).

---

## 5. Kết Luận

Servlet là một thành phần quan trọng trong ứng dụng web Java, giúp bạn xử lý các yêu cầu HTTP và trả về các kết quả động dựa trên dữ liệu đầu vào. Việc hiểu và biết cách sử dụng servlet sẽ giúp bạn xây dựng các ứng dụng web mạnh mẽ và linh hoạt hơn.

Bằng cách kết hợp servlet với JSP và các thành phần khác như Filter và Listener, bạn có thể tạo ra các ứng dụng web hoàn chỉnh và mạnh mẽ hơn.

**Tài Nguyên Tham Khảo:**

- [Tài liệu Java Servlet của Oracle](https://docs.oracle.com/javaee/7/tutorial/servlets.htm)
- [Servlet API Documentation](https://javaee.github.io/javaee-spec/javadocs/javax/servlet/package-summary.html)
