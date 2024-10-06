### Tìm Hiểu Về Servlet Filter: Khái Niệm, API Và Ví Dụ Chi Tiết

#### 1. **Servlet Filter Là Gì?**
Servlet Filter là một đối tượng trong lập trình Java được sử dụng để thực hiện **tiền xử lý (pre-processing)** và **hậu xử lý (post-processing)** cho các yêu cầu (request) và phản hồi (response) trong ứng dụng web. Khi một yêu cầu được gửi đến một tài nguyên nào đó (Servlet, JSP hoặc HTML), Filter sẽ được kích hoạt tự động trước khi yêu cầu đến tài nguyên và sau khi phản hồi được gửi đi.

#### 2. **Mục Đích Và Công Dụng Của Filter**
Filter có rất nhiều mục đích và công dụng, giúp ứng dụng web trở nên linh hoạt và mạnh mẽ hơn, bao gồm:
- **Ghi nhật ký (Logging)**: Ghi lại các yêu cầu đến từ địa chỉ IP nào, thời gian, và loại tài nguyên được truy cập.
- **Nén dữ liệu (Data Compression)**: Nén hoặc giải nén dữ liệu trước khi gửi tới client hoặc sau khi nhận từ client.
- **Chuyển đổi dữ liệu (Data Conversion)**: Chuyển đổi dữ liệu từ định dạng này sang định dạng khác.
- **Xác thực đầu vào (Input Validation)**: Kiểm tra và xác thực dữ liệu đầu vào từ người dùng.
- **Mã hóa và giải mã (Encryption & Decryption)**: Mã hóa và giải mã dữ liệu để đảm bảo tính bảo mật.

#### 3. **Ưu Điểm Của Filter**
- **Tính Pluggable**: Filter có thể dễ dàng thêm hoặc gỡ bỏ khỏi ứng dụng thông qua file cấu hình `web.xml` mà không cần thay đổi mã nguồn của Servlet.
- **Không phụ thuộc lẫn nhau**: Các filter không có sự phụ thuộc trực tiếp vào nhau, giúp hệ thống dễ bảo trì và mở rộng.
- **Tính tái sử dụng**: Filter có thể được sử dụng cho nhiều tài nguyên khác nhau như Servlet, JSP hoặc thậm chí là các trang HTML thông thường.
- **Chi phí bảo trì thấp**: Vì Filter không phụ thuộc trực tiếp vào các tài nguyên khác nên dễ dàng thêm mới hoặc thay đổi mà không làm ảnh hưởng đến các phần khác của hệ thống.

#### 4. **Cấu Trúc Của Filter API**
Filter API bao gồm ba giao diện chính nằm trong gói `javax.servlet`:
- **Filter**: Giao diện cơ bản để tạo một Filter.
- **FilterChain**: Điều phối việc gọi các Filter khác trong chuỗi hoặc gọi đến tài nguyên cuối cùng.
- **FilterConfig**: Cung cấp các thông tin cấu hình cho Filter.

Dưới đây là chi tiết về từng giao diện:

#### 4.1 **Giao Diện `Filter`**
Giao diện `Filter` bao gồm các phương thức vòng đời của một Filter, cụ thể như sau:
- `void init(FilterConfig config)`: Được gọi một lần duy nhất khi Filter được khởi tạo. Sử dụng để cấu hình hoặc khởi tạo các tài nguyên cần thiết.
- `void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)`: Được gọi mỗi khi có yêu cầu đến tài nguyên mà Filter được gán vào. Phương thức này giúp xử lý các tác vụ tiền xử lý và hậu xử lý.
- `void destroy()`: Được gọi khi Filter bị gỡ bỏ khỏi ứng dụng, sử dụng để giải phóng các tài nguyên đã sử dụng.

#### 4.2 **Giao Diện `FilterChain`**
Giao diện `FilterChain` chịu trách nhiệm gọi Filter tiếp theo trong chuỗi hoặc gọi tài nguyên cuối cùng (Servlet, JSP,...). Nó chỉ có một phương thức duy nhất:
- `void doFilter(ServletRequest request, ServletResponse response)`: Chuyển tiếp yêu cầu tới Filter tiếp theo hoặc tới tài nguyên đích.

#### 4.3 **Giao Diện `FilterConfig`**
`FilterConfig` cung cấp thông tin cấu hình cho Filter từ file `web.xml`. Các phương thức của `FilterConfig` bao gồm:
- `String getFilterName()`: Trả về tên của Filter.
- `ServletContext getServletContext()`: Trả về đối tượng `ServletContext`.
- `String getInitParameter(String name)`: Trả về tham số khởi tạo được định nghĩa trong `web.xml`.
- `Enumeration<String> getInitParameterNames()`: Trả về danh sách các tham số khởi tạo.

#### 5. **Cách Định Nghĩa Và Cấu Hình Filter Trong `web.xml`**
Filter được cấu hình trong file `web.xml` tương tự như Servlet. Dưới đây là các thành phần cấu trúc của `web.xml` để định nghĩa Filter:

```xml
<web-app>  
    <!-- Định nghĩa Filter -->
    <filter>  
        <filter-name>f1</filter-name>  
        <filter-class>MyFilter</filter-class>  
    </filter>  
   
    <!-- Định nghĩa bộ lọc (mapping) cho Filter -->
    <filter-mapping>  
        <filter-name>f1</filter-name>  
        <url-pattern>/servlet1</url-pattern>  
    </filter-mapping>  
</web-app>  
```
- **`<filter-name>`**: Tên của Filter.
- **`<filter-class>`**: Lớp thực thi của Filter (implement `Filter` interface).
- **`<filter-mapping>`**: Ánh xạ Filter tới tài nguyên đích, có thể sử dụng `url-pattern` hoặc `servlet-name` để chỉ định.

#### 6. **Ví Dụ Cụ Thể: Tạo Và Ánh Xạ Filter**
**6.1. Tạo Filter**
Tạo một lớp Filter cơ bản để hiển thị thông báo trước và sau khi gọi tài nguyên (Servlet, JSP,...).

```java
import java.io.IOException;  
import java.io.PrintWriter;  
import javax.servlet.*;  

public class MyFilter implements Filter {  

    public void init(FilterConfig config) throws ServletException {}  

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {  
        // Hiển thị thông báo trước khi yêu cầu đến tài nguyên đích
        PrintWriter out = resp.getWriter();  
        out.print("Filter is invoked before");  
          
        // Chuyển tiếp yêu cầu đến tài nguyên tiếp theo hoặc tài nguyên đích
        chain.doFilter(req, resp);  
          
        // Hiển thị thông báo sau khi tài nguyên đích đã xử lý xong yêu cầu
        out.print("Filter is invoked after");  
    }  
    
    public void destroy() {}  
}  
```
**6.2. Tạo Servlet**
Tạo một lớp Servlet để hiển thị nội dung đơn giản.

```java
import java.io.IOException;  
import java.io.PrintWriter;  
import javax.servlet.ServletException;  
import javax.servlet.http.*;  

public class HelloServlet extends HttpServlet {  
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
        response.setContentType("text/html");  
        PrintWriter out = response.getWriter();  
        out.print("<br>Welcome to Servlet<br>");  
    }  
}  
```
**6.3. Cấu Hình `web.xml`**
Cấu hình `web.xml` để định nghĩa Filter và ánh xạ Filter với Servlet.

```xml
<web-app>  
    <!-- Định nghĩa Servlet -->
    <servlet>  
        <servlet-name>s1</servlet-name>  
        <servlet-class>HelloServlet</servlet-class>  
    </servlet>  
  
    <servlet-mapping>  
        <servlet-name>s1</servlet-name>  
        <url-pattern>/servlet1</url-pattern>  
    </servlet-mapping>  
  
    <!-- Định nghĩa Filter -->
    <filter>  
        <filter-name>f1</filter-name>  
        <filter-class>MyFilter</filter-class>  
    </filter>  
   
    <filter-mapping>  
        <filter-name>f1</filter-name>  
        <url-pattern>/servlet1</url-pattern>  
    </filter-mapping>  
</web-app>  
```

#### 7. **Kết Quả Khi Chạy Ứng Dụng**
Khi truy cập vào đường dẫn `http://localhost:8080/YourAppName/servlet1`, kết quả sẽ hiển thị như sau:

```
Filter is invoked before
Welcome to Servlet
Filter is invoked after
```
- **`Filter is invoked before`**: Thông báo từ Filter trước khi Servlet `HelloServlet` được gọi.
- **`Welcome to Servlet`**: Nội dung được tạo bởi Servlet.
- **`Filter is invoked after`**: Thông báo từ Filter sau khi Servlet đã xử lý xong.

#### 8. **Tổng Kết**
Servlet Filter là một công cụ mạnh mẽ để thực hiện các tác vụ tiền xử lý và hậu xử lý cho các yêu cầu và phản hồi trong ứng dụng web. Nó mang lại sự linh hoạt, khả năng tái sử dụng cao và dễ dàng tích hợp vào bất kỳ ứng dụng nào mà không ảnh hưởng đến mã nguồn chính.
