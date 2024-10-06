### Cấu Hình Tomcat Để Sử Dụng Database Connection Pool

Trong video này, chúng ta sẽ tìm hiểu cách thiết lập **Tomcat** để sử dụng **Database Connection Pool** cho ứng dụng web của bạn. Connection Pool là một kỹ thuật quan trọng giúp ứng dụng của bạn có thể xử lý nhiều người dùng đồng thời mà không gặp phải tình trạng tắc nghẽn do thiếu kết nối với cơ sở dữ liệu. Hãy cùng khám phá các bước chi tiết sau đây để cài đặt và sử dụng Connection Pool trong Tomcat nhé!

---

### 1. **Tổng Quan Về Database Connection Pool**

Khi ứng dụng web của bạn kết nối đến cơ sở dữ liệu, việc tạo một kết nối duy nhất sẽ không thể đáp ứng được lượng người dùng lớn. Điều này tương tự như việc trong một văn phòng lớn chỉ có một chiếc điện thoại duy nhất và hàng chục người cần sử dụng nó. Kết quả là sẽ có một hàng dài chờ đợi để sử dụng điện thoại, khiến cho hiệu suất làm việc bị ảnh hưởng nghiêm trọng.

**Database Connection Pool** sẽ giải quyết vấn đề này bằng cách tạo ra một nhóm (pool) các kết nối từ trước. Khi người dùng yêu cầu kết nối, ứng dụng sẽ lấy một kết nối từ pool có sẵn. Sau khi hoàn thành yêu cầu, kết nối sẽ được trả về pool để sẵn sàng cho người dùng khác. Điều này giúp giảm thời gian chờ đợi và tăng cường hiệu suất cho ứng dụng.

#### Lợi ích của Connection Pool:
- **Tăng hiệu suất**: Không cần phải tạo kết nối mới mỗi khi có yêu cầu.
- **Giảm chi phí tài nguyên**: Tận dụng kết nối có sẵn, giảm tải cho máy chủ cơ sở dữ liệu.
- **Dễ dàng mở rộng**: Có thể điều chỉnh số lượng kết nối tối đa theo nhu cầu của ứng dụng.

---

### 2. **Các Bước Cấu Hình Connection Pool Trong Tomcat**

Để cấu hình Connection Pool trong Tomcat, chúng ta cần thực hiện các bước sau:

#### Bước 1: **Tải JDBC Driver**
- Đầu tiên, chúng ta cần tải **JDBC driver** để kết nối đến cơ sở dữ liệu.
- Truy cập trang web của MySQL tại: [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/) để tải về `mysql-connector-java.jar`.
- Sau khi tải xong, bạn cần đặt tệp JAR này vào thư mục đặc biệt của ứng dụng web là **WEB-INF/lib**. Đây là nơi chứa các thư viện bổ trợ (JAR files) mà ứng dụng sẽ sử dụng trong quá trình chạy.

#### Bước 2: **Định Nghĩa Connection Pool Trong `context.xml`**

- Tiếp theo, chúng ta sẽ cấu hình Connection Pool trong tệp **`context.xml`** của Tomcat.
- Đây là tệp đặc biệt giúp Tomcat biết cách kết nối đến cơ sở dữ liệu và thiết lập các thông số cho Connection Pool.

**Cấu trúc tệp `context.xml`:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>

    <!-- Cấu hình Connection Pool -->
    <Resource name="jdbc/web_student_tracker"
              auth="Container"
              type="javax.sql.DataSource"
              maxActive="20"
              maxIdle="5"
              maxWait="10000"
              username="root"
              password="your_password"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://localhost:3306/student_tracker?useSSL=false&serverTimezone=UTC" />

</Context>
```

- **`name="jdbc/web_student_tracker"`**: Tên của Connection Pool mà chúng ta sẽ sử dụng để truy xuất trong mã nguồn Java.
- **`auth="Container"`**: Cho phép Tomcat tự động xử lý việc xác thực kết nối.
- **`type="javax.sql.DataSource"`**: Loại tài nguyên là `DataSource`, giúp Java biết rằng đây là một Connection Pool.
- **`maxActive="20"`**: Số lượng kết nối tối đa mà pool có thể quản lý cùng một lúc.
- **`maxIdle="5"`**: Số lượng kết nối mà pool sẽ giữ lại ngay cả khi không có người dùng.
- **`maxWait="10000"`**: Thời gian chờ (tính bằng mili giây) trước khi trả về lỗi khi không có kết nối nào khả dụng.
- **`username` và `password`**: Thông tin xác thực cho cơ sở dữ liệu.
- **`driverClassName="com.mysql.cj.jdbc.Driver"`**: Tên đầy đủ của JDBC driver cho MySQL.
- **`url`**: Đường dẫn kết nối tới cơ sở dữ liệu MySQL của bạn.

#### Bước 3: **Truy Xuất Connection Pool Trong Mã Nguồn Java**

- Sau khi cấu hình xong, chúng ta cần lấy kết nối từ Connection Pool và sử dụng nó trong mã nguồn Java.

**Ví dụ truy xuất Connection Pool trong Servlet:**

```java
package com.example;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.SQLException;
import javax.annotation.Resource;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.sql.DataSource;

@WebServlet("/TestServlet")
public class TestServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // Sử dụng Resource Annotation để tiêm đối tượng DataSource
    @Resource(name = "jdbc/web_student_tracker")
    private DataSource dataSource;

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        PrintWriter out = response.getWriter();
        Connection conn = null;

        try {
            // Lấy kết nối từ Connection Pool
            conn = dataSource.getConnection();

            // Hiển thị thông báo kết nối thành công
            out.println("Connected to the database!");

        } catch (SQLException e) {
            e.printStackTrace();
            out.println("Error connecting to the database: " + e.getMessage());
        } finally {
            try {
                // Trả kết nối về lại pool sau khi sử dụng
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**Giải Thích:**
- **@Resource(name = "jdbc/web_student_tracker")**: Annotation này giúp Java tự động lấy `DataSource` từ Tomcat theo tên mà chúng ta đã định nghĩa trong `context.xml`.
- **`dataSource.getConnection()`**: Lấy một kết nối từ Connection Pool.
- Sau khi sử dụng xong kết nối, đừng quên **đóng kết nối** (`conn.close()`). Điều này sẽ trả kết nối về lại pool để người dùng khác có thể tiếp tục sử dụng.

---

### 3. **Kiểm Tra Và Triển Khai**

1. **Khởi động Tomcat**: Đảm bảo rằng Tomcat đã được cấu hình đúng và đang chạy.
2. **Triển khai ứng dụng**: Deploy ứng dụng web của bạn vào Tomcat.
3. **Kiểm tra bằng URL**: Mở trình duyệt và truy cập vào đường dẫn của Servlet, ví dụ: `http://localhost:8080/YourAppName/TestServlet`.
4. **Kết quả**: Bạn sẽ thấy thông báo `Connected to the database!` nếu kết nối thành công.

---

### 4. **Tổng Kết**

Việc cấu hình **Database Connection Pool** trong Tomcat giúp tối ưu hóa hiệu suất và quản lý kết nối hiệu quả hơn. Với Connection Pool, bạn có thể xử lý nhiều kết nối đồng thời mà không gặp phải tình trạng quá tải. Điều này đặc biệt quan trọng khi ứng dụng của bạn cần phục vụ số lượng lớn người dùng trong cùng một thời điểm.

Nếu bạn gặp bất kỳ vấn đề nào trong quá trình cấu hình hoặc triển khai, hãy để lại bình luận để chúng ta cùng thảo luận và giải quyết nhé! 😄

**Chúc bạn thành công với việc kết nối và tối ưu hóa cơ sở dữ liệu trong Tomcat!**
