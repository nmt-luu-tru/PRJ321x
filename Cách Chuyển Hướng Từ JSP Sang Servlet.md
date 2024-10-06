### Cách Chuyển Hướng Từ JSP Sang Servlet

Trong Java web development, có nhiều cách để chuyển hướng (redirect) từ một JSP sang một Servlet. Một trong những phương pháp phổ biến nhất là sử dụng phương thức `response.sendRedirect( <thePageUrl> )`.

Phương thức `sendRedirect()` giúp chuyển hướng trình duyệt của người dùng từ một trang web hiện tại sang một URL khác. Khi sử dụng `sendRedirect`, trình duyệt sẽ gửi một yêu cầu HTTP GET mới tới URL được chỉ định.

Dưới đây là một ví dụ đơn giản về cách chuyển hướng từ một tệp JSP sang một Servlet.

### Ví dụ: Chuyển Hướng Từ JSP Sang Servlet

**1. Tạo Tệp `test-redirect.jsp`**

Tệp JSP này sẽ gửi yêu cầu chuyển hướng tới một Servlet có URL mapping là `/hello`.

```jsp
<%
    // Chuyển hướng tới HelloWorldServlet với URL mapping là "/hello"
    response.sendRedirect(request.getContextPath() + "/hello");
%>
```

**Giải Thích:**
- Phương thức `response.sendRedirect(request.getContextPath() + "/hello");` sẽ gửi yêu cầu HTTP GET đến Servlet có URL mapping là `/hello`.
- `request.getContextPath()` trả về đường dẫn ngữ cảnh hiện tại của ứng dụng (ví dụ: `/myApp`), sau đó nối với `/hello` để tạo thành URL đầy đủ cho Servlet.

**2. Tạo Servlet `HelloWorldServlet.java`**

Tạo một Servlet có URL mapping là `/hello`. Khi trình duyệt được chuyển hướng đến Servlet này, nó sẽ xử lý yêu cầu và trả về kết quả.

```java
package com.luv2code.demo;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// Định nghĩa URL mapping cho Servlet này là "/hello"
@WebServlet("/hello")
public class HelloWorldServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // Override phương thức doGet để xử lý yêu cầu GET
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // Thiết lập kiểu dữ liệu phản hồi là "text/html"
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        
        // Gửi nội dung HTML đơn giản về cho trình duyệt
        out.println("Hello world: " + new java.util.Date());
    }
}
```

**Giải Thích:**
- Annotation `@WebServlet("/hello")` chỉ định URL mapping cho Servlet này là `/hello`.
- Phương thức `doGet()` được override để xử lý các yêu cầu HTTP GET. Nội dung HTML sẽ được tạo động và trả về cho trình duyệt.
- Trong ví dụ trên, khi một yêu cầu được gửi tới `/hello`, servlet sẽ phản hồi bằng thông báo "Hello world" cùng với thời gian hiện tại.

### Cách Thức Hoạt Động
1. Khi người dùng truy cập vào `test-redirect.jsp`, phương thức `sendRedirect()` sẽ được gọi và tạo ra một yêu cầu HTTP GET mới đến `HelloWorldServlet`.
2. Trình duyệt của người dùng sẽ tự động được chuyển hướng đến URL `/hello`.
3. `HelloWorldServlet` sẽ xử lý yêu cầu, tạo nội dung HTML và gửi phản hồi về cho trình duyệt.

### Một Số Lưu Ý
- **Khi sử dụng `sendRedirect`**, trình duyệt sẽ gửi yêu cầu HTTP mới, do đó mọi thông tin về yêu cầu ban đầu (như `request` và `response`) sẽ bị mất.
- **Không sử dụng `@WebServlet` với `sendRedirect`** khi bạn đã định nghĩa cấu hình Servlet trong tệp `web.xml`. Điều này có thể gây ra lỗi trùng lặp cấu hình.

### Tổng Kết
Khi bạn chạy tệp JSP `test-redirect.jsp`, nó sẽ tự động chuyển hướng tới `HelloWorldServlet`, hiển thị nội dung `"Hello world"` cùng với thời gian hiện tại.

Chúc bạn thành công trong việc sử dụng `sendRedirect` để chuyển hướng từ JSP sang Servlet! Nếu có thêm câu hỏi hoặc vấn đề, đừng ngần ngại liên hệ nhé! 😄
