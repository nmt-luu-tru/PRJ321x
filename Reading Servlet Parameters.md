# Hướng Dẫn Chi Tiết Sử Dụng Servlet Để Đọc Các Tham Số Cấu Hình

## Mục Lục
1. Giới Thiệu
2. Tại Sao Sử Dụng Tham Số Cấu Hình?
3. Tạo Các Tham Số Cấu Hình Trong `web.xml`
4. Đọc Các Tham Số Cấu Hình Bằng Servlet
   - Bước 1: Thiết Lập `ServletContext`
   - Bước 2: Đọc Tham Số Cấu Hình
5. Ví Dụ Thực Tế
6. Chạy Và Kiểm Tra Servlet
7. Lợi Ích Của Việc Sử Dụng Tham Số Cấu Hình
8. Kết Luận

---

## 1. Giới Thiệu

Trong quá trình phát triển ứng dụng web bằng Java, các lập trình viên thường gặp tình huống cần thiết lập các thông tin cấu hình cho ứng dụng như kích thước giỏ hàng tối đa, tên đội phát triển, hay các URL kết nối với dịch vụ bên ngoài. Việc viết cố định (hard code) những thông tin này vào mã nguồn có thể gây khó khăn khi cần cập nhật hoặc thay đổi giá trị, đồng thời làm giảm tính linh hoạt của ứng dụng.

Vì vậy, một phương pháp phổ biến là sử dụng **các tham số cấu hình** (Configuration Parameters) để lưu trữ những giá trị này ở một vị trí trung tâm. Trong Java Servlets và JSP, các tham số cấu hình có thể được khai báo và lưu trữ trong tệp `web.xml`. Sau đó, chúng ta có thể đọc các tham số này thông qua `ServletContext` mà không cần sửa đổi mã nguồn của servlet.

---

## 2. Tại Sao Sử Dụng Tham Số Cấu Hình?

Sử dụng tham số cấu hình trong các ứng dụng web mang lại nhiều lợi ích:

1. **Quản lý thông tin dễ dàng hơn:** Khi các giá trị được lưu trữ trong tệp cấu hình, việc cập nhật, thay đổi thông tin chỉ cần thực hiện tại một vị trí duy nhất, không cần chỉnh sửa mã nguồn của ứng dụng.
2. **Tính linh hoạt cao:** Các tham số cấu hình giúp ứng dụng dễ dàng thay đổi giá trị mà không cần phải biên dịch lại mã nguồn. Ví dụ: thay đổi địa chỉ IP của máy chủ cơ sở dữ liệu hoặc cập nhật thông tin dịch vụ bên ngoài.
3. **Phân tách giữa logic ứng dụng và thông tin cấu hình:** Các tham số cấu hình giúp tách biệt rõ ràng giữa logic xử lý và dữ liệu cấu hình, làm cho mã nguồn dễ đọc, dễ bảo trì hơn.

---

## 3. Tạo Các Tham Số Cấu Hình Trong `web.xml`

Các tham số cấu hình được khai báo trong tệp `WEB-INF/web.xml` của ứng dụng web. Đây là tệp cấu hình trung tâm chứa các thông tin như phiên bản ứng dụng, khai báo các servlet, bộ lọc (filter), và cả các tham số cấu hình chung.

**Ví dụ: Cấu hình trong `web.xml`**

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
         
    <!-- Tham số cấu hình chung (Context Parameters) -->
    <context-param>
        <param-name>max-shopping-cart-size</param-name>
        <param-value>99</param-value>
    </context-param>
    
    <context-param>
        <param-name>project-team-name</param-name>
        <param-value>The Coding Gurus</param-value>
    </context-param>
    
</web-app>
```

### Giải thích:

1. **`<context-param>`**: Dùng để khai báo một tham số cấu hình chung cho toàn bộ ứng dụng.
2. **`<param-name>`**: Định nghĩa tên của tham số.
3. **`<param-value>`**: Giá trị của tham số.

Trong ví dụ trên:
- `max-shopping-cart-size` có giá trị là `99`.
- `project-team-name` có giá trị là `The Coding Gurus`.

Bất kỳ servlet nào trong ứng dụng cũng có thể truy xuất các giá trị này thông qua đối tượng `ServletContext`.

---

## 4. Đọc Các Tham Số Cấu Hình Bằng Servlet

### Bước 1: Thiết Lập `ServletContext`

Để đọc các tham số cấu hình trong `web.xml`, chúng ta cần sử dụng đối tượng `ServletContext`. Đây là một đối tượng đại diện cho toàn bộ ứng dụng web và cho phép truy cập tới nhiều thông tin như đường dẫn tệp, tham số cấu hình, và nhiều tài nguyên khác của ứng dụng.

```java
ServletContext context = getServletContext();
```

Phương thức `getServletContext()` sẽ trả về một đối tượng `ServletContext` liên kết với ứng dụng web hiện tại.

### Bước 2: Đọc Tham Số Cấu Hình

Chúng ta có thể sử dụng phương thức `context.getInitParameter("parameter-name")` để lấy giá trị của tham số cấu hình. Phương thức này trả về giá trị của tham số dưới dạng chuỗi (`String`).

```java
String maxCartSize = context.getInitParameter("max-shopping-cart-size");
String teamName = context.getInitParameter("project-team-name");
```

Nếu tham số tồn tại trong `web.xml`, phương thức này sẽ trả về giá trị tương ứng. Nếu không, nó sẽ trả về `null`.

---

## 5. Ví Dụ Thực Tế

Hãy tạo một servlet có tên là `TestParamServlet` để đọc và hiển thị các tham số cấu hình đã định nghĩa trong tệp `web.xml`:

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/TestParamServlet")
public class TestParamServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        
        // Thiết lập kiểu nội dung trả về
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // Đọc các tham số cấu hình từ ServletContext
        ServletContext context = getServletContext();
        String maxCartSize = context.getInitParameter("max-shopping-cart-size");
        String teamName = context.getInitParameter("project-team-name");

        // Tạo nội dung HTML trả về cho trình duyệt
        out.println("<html><body>");
        out.println("<h2>Các Tham Số Cấu Hình</h2>");
        out.println("<p>Max Shopping Cart Size: " + maxCartSize + "</p>");
        out.println("<p>Project Team Name: " + teamName + "</p>");
        out.println("</body></html>");
    }
}
```

**Giải thích mã nguồn:**
- `ServletContext context = getServletContext();`  
  Lấy đối tượng `ServletContext` để có thể đọc các tham số cấu hình từ `web.xml`.
  
- `context.getInitParameter("parameter-name")`  
  Đọc giá trị của tham số cấu hình với tên là `"parameter-name"`.

- `out.println(...)`  
  Hiển thị giá trị các tham số cấu hình trên trình duyệt dưới dạng HTML.

---

## 6. Chạy Và Kiểm Tra Servlet

1. Khởi động máy chủ (ví dụ: Apache Tomcat).
2. Truy cập servlet bằng cách nhập URL sau vào trình duyệt:

   ```
   http://localhost:8080/TestParamServlet
   ```

3. **Kết quả mong đợi:** Servlet sẽ hiển thị các tham số cấu hình như sau:

   ```
   Các Tham Số Cấu Hình
   Max Shopping Cart Size: 99
   Project Team Name: The Coding Gurus
   ```

---

## 7. Lợi Ích Của Việc Sử Dụng Tham Số Cấu Hình

1. **Dễ bảo trì:** Các thông tin cấu hình có thể được thay đổi mà không cần chỉnh sửa mã nguồn.
2. **Quản lý tập trung:** Tất cả thông tin cấu hình đều được lưu trữ trong một tệp duy nhất (`web.xml`).
3. **Tăng tính linh hoạt:** Khi cần thay đổi thông tin (ví dụ: cập nhật tên nhóm phát triển), chỉ cần sửa đổi `web.xml` mà không cần phải biên dịch lại ứng dụng.

---

## 8. Kết Luận

Qua bài viết này, chúng ta đã tìm hiểu cách thiết lập và đọc các tham số cấu hình từ tệp `web.xml` trong ứng dụng Java Servlet. Việc sử dụng tham số cấu hình giúp ứng dụng dễ bảo trì, linh hoạt và dễ quản lý hơn. Đây là một kỹ thuật rất hữu ích trong việc phát triển các ứng dụng web chuyên nghiệp.

**Tài nguyên tham khảo:**
- [Java Servlet Documentation](https://docs.oracle.com/javaee/7/tutorial/servlets.htm)
- [Servlet API Documentation](

https://javaee.github.io/javaee-spec/javadocs/javax/servlet/package-summary.html)
