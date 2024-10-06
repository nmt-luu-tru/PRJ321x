### Cách Định Nghĩa và Sử Dụng Tham Số Cấu Hình Theo Servlet (`ServletConfig`)

Trong Java Servlet, chúng ta có hai loại tham số cấu hình:

1. **Tham số cấp ứng dụng (`ServletContext`)**: Áp dụng cho toàn bộ ứng dụng web và có thể được truy cập bởi tất cả các servlet trong ứng dụng.
2. **Tham số cấp servlet (`ServletConfig`)**: Áp dụng riêng cho từng servlet. Chỉ servlet đó mới có thể đọc được các tham số này.

Bài viết này sẽ giải thích chi tiết về việc sử dụng **`ServletConfig`** để định nghĩa và truy cập các tham số riêng lẻ cho từng servlet.

---

### 1. Giới Thiệu Về `ServletConfig`

`ServletConfig` cho phép bạn định nghĩa và quản lý các tham số riêng cho mỗi servlet. Những tham số này không được chia sẻ giữa các servlet khác và thường được sử dụng để cung cấp những cấu hình đặc biệt cho từng servlet.

Khi sử dụng `ServletConfig`, bạn cần cấu hình trong tệp `web.xml` và đọc các tham số đó bằng cách sử dụng phương thức `getInitParameter("parameterName")`.

---

### 2. Tạo Tham Số Cấu Hình Cho Từng Servlet Trong `web.xml`

Bạn cần khai báo từng servlet và định nghĩa các tham số riêng của nó trong tệp `web.xml`. Ví dụ:

```xml
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" 
         version="3.1">
  
  <!-- Các tham số cấu hình cấp ứng dụng -->
  <context-param>
    <param-name>max-shopping-cart-size</param-name>
    <param-value>99</param-value>
  </context-param>
  
  <context-param>
    <param-name>project-team-name</param-name>
    <param-value>The Coding Gurus</param-value>
  </context-param>
  
  <!-- Cấu hình Servlet và các tham số riêng -->
  <servlet>
    <servlet-name>TestParamServlet</servlet-name>
    <servlet-class>com.luv2code.servletdemo.TestParamServlet</servlet-class>
    
    <!-- Định nghĩa các tham số cấu hình riêng cho TestParamServlet -->
    <init-param>
      <param-name>greeting</param-name>
      <param-value>Welcome</param-value>
    </init-param>
    
    <init-param>
      <param-name>serviceLevel</param-name>
      <param-value>Platinum</param-value>
    </init-param>
  </servlet>
  
  <!-- Mappings cho Servlet -->
  <servlet-mapping>
    <servlet-name>TestParamServlet</servlet-name>
    <url-pattern>/demo</url-pattern>
  </servlet-mapping>
  
</web-app>
```

#### Giải thích:

- **`<servlet>`**: Định nghĩa một servlet với tên là `TestParamServlet` và lớp xử lý là `com.luv2code.servletdemo.TestParamServlet`.
- **`<init-param>`**: Khai báo các tham số riêng cho servlet `TestParamServlet`:
  - `greeting`: Chứa thông báo chào mừng.
  - `serviceLevel`: Định nghĩa mức độ dịch vụ của ứng dụng.

#### Lưu ý:

- Để sử dụng `ServletConfig`, bạn cần định nghĩa servlet trong `web.xml` và không sử dụng các annotation như `@WebServlet`.
- Các tham số này chỉ có hiệu lực và có thể được truy cập trong `TestParamServlet`. Các servlet khác không thể truy cập các tham số này.

---

### 3. Đọc Tham Số Cấu Hình Riêng Trong Servlet

Trong servlet `TestParamServlet`, bạn có thể đọc các tham số riêng lẻ này bằng cách sử dụng phương thức `getInitParameter("parameterName")`. Đây là phương thức được cung cấp bởi `HttpServlet`.

```java
package com.luv2code.servletdemo;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class TestParamServlet extends HttpServlet {
    
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        
        // Bước 1: Thiết lập kiểu dữ liệu trả về cho trình duyệt
        response.setContentType("text/html");
        
        // Bước 2: Lấy PrintWriter để gửi phản hồi tới trình duyệt
        PrintWriter out = response.getWriter();
        
        // Bước 3: Đọc các tham số cấu hình cấp ứng dụng (ServletContext)
        ServletContext context = getServletContext();
        String maxCartSize = context.getInitParameter("max-shopping-cart-size");
        String teamName = context.getInitParameter("project-team-name");
        
        // Bước 4: Đọc các tham số cấu hình riêng của servlet (ServletConfig)
        String theGreetingMessage = getInitParameter("greeting");
        String theServiceLevel = getInitParameter("serviceLevel");
        
        // Bước 5: Tạo nội dung HTML trả về cho trình duyệt
        out.println("<html><body>");
        out.println("<h2>Các Tham Số Cấu Hình</h2>");
        out.println("<p>Max Shopping Cart Size: " + maxCartSize + "</p>");
        out.println("<p>Project Team Name: " + teamName + "</p>");
        out.println("<hr>");
        out.println("<h3>Tham Số Cấu Hình Riêng Của TestParamServlet</h3>");
        out.println("<p>Greeting: " + theGreetingMessage + "</p>");
        out.println("<p>Service Level: " + theServiceLevel + "</p>");
        out.println("</body></html>");
    }
}
```

#### Giải thích mã nguồn:

1. **Đọc các tham số cấp ứng dụng (`ServletContext`)**:
   - `context.getInitParameter("max-shopping-cart-size")`: Lấy giá trị `max-shopping-cart-size` từ `web.xml`.
   - `context.getInitParameter("project-team-name")`: Lấy giá trị `project-team-name` từ `web.xml`.

2. **Đọc các tham số cấp servlet (`ServletConfig`)**:
   - `getInitParameter("greeting")`: Lấy giá trị `greeting` đã định nghĩa riêng cho `TestParamServlet`.
   - `getInitParameter("serviceLevel")`: Lấy giá trị `serviceLevel` của `TestParamServlet`.

3. **Tạo nội dung HTML**: Các giá trị cấu hình được hiển thị trong trang HTML để dễ dàng kiểm tra.

---

### 4. Kiểm Tra Servlet

- Truy cập servlet bằng URL:

  ```
  http://localhost:8080/demo
  ```

- **Kết quả**: Trang HTML hiển thị thông tin cấu hình:

  ```
  Các Tham Số Cấu Hình
  Max Shopping Cart Size: 99
  Project Team Name: The Coding Gurus

  Tham Số Cấu Hình Riêng Của TestParamServlet
  Greeting: Welcome
  Service Level: Platinum
  ```

---

### 5. Lợi Ích Của `ServletConfig`

1. **Phân tách thông tin cấu hình giữa các servlet**: Mỗi servlet có thể có cấu hình riêng, giúp dễ dàng quản lý và bảo trì.
2. **Tăng tính linh hoạt**: Có thể cập nhật thông tin cấu hình cho từng servlet mà không ảnh hưởng đến các servlet khác.
3. **Dễ dàng kiểm soát và mở rộng**: Thêm hoặc sửa đổi các tham số mà không cần thay đổi mã nguồn của servlet.

---

### 6. Kết Luận

`ServletConfig` là một công cụ mạnh mẽ giúp bạn cấu hình riêng cho từng servlet trong một ứng dụng Java web. Bằng cách sử dụng `ServletConfig` và `ServletContext`, bạn có thể dễ dàng quản lý các thông tin cấu hình cho toàn bộ ứng dụng hoặc chỉ cho một servlet cụ thể, giúp tăng tính linh hoạt và dễ bảo trì của ứng dụng.
