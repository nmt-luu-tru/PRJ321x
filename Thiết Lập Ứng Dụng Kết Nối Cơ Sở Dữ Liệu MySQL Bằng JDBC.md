### Thiết Lập Ứng Dụng Kết Nối Cơ Sở Dữ Liệu MySQL Bằng JDBC Trong Dự Án JSP/Servlet

Trong bài viết này, chúng ta sẽ tạo một ứng dụng web đơn giản với **JSP** và **Servlet** để kết nối tới cơ sở dữ liệu MySQL. Mục tiêu của bài viết là giúp bạn hiểu cách thức xây dựng một ứng dụng web kết nối với cơ sở dữ liệu thông qua **JDBC**. Đồng thời, chúng ta sẽ tạo một servlet để hiển thị dữ liệu sinh viên từ cơ sở dữ liệu trên giao diện web.

#### Mục Tiêu Cụ Thể:
1. **Tạo một dự án Web động (Dynamic Web Project)**.
2. **Cấu hình thư viện JDBC** để kết nối cơ sở dữ liệu MySQL.
3. **Thiết lập Context Configuration** cho kết nối cơ sở dữ liệu.
4. **Tạo servlet kiểm tra kết nối** và thực hiện truy vấn dữ liệu.
5. **Kiểm tra kết nối** và hiển thị dữ liệu trên trình duyệt.

Hãy cùng tìm hiểu chi tiết từng bước!

---

### Bước 1: Tạo Dự Án Web Động Mới
1. **Mở Eclipse IDE** và chọn **File > New > Dynamic Web Project**.
2. Đặt tên dự án là `Web Student Tracker`.
3. Thay đổi cấu hình thư mục nguồn và thư mục nội dung (source folder và content directory) để đảm bảo phù hợp với các dự án khác:
   - **Source folder**: Tạo một folder mới với tên `src`.
   - **Content directory**: Đặt tên là `WebContent`.
4. Nhấn **Finish** để hoàn tất quá trình tạo dự án.

---

### Bước 2: Thêm JDBC JAR File Vào Dự Án
1. Mở thư mục **WebContent/WEB-INF/lib** trong dự án của bạn.
2. Sao chép file **MySQL JDBC Connector JAR** vào thư mục này. Đây là file `mysql-connector-java-X.X.X.jar`, bạn có thể tải file này từ trang chủ MySQL hoặc sử dụng file có sẵn trong thư mục `starter-files` của dự án.

**Lưu Ý**: JDBC JAR file này sẽ cung cấp các API cần thiết để Java có thể kết nối và tương tác với cơ sở dữ liệu MySQL.

---

### Bước 3: Cấu Hình `context.xml` Để Thiết Lập Kết Nối
1. Mở thư mục `WebContent/META-INF` trong dự án.
2. Thêm file `context.xml` với nội dung như sau:

```xml
<Context>
    <Resource name="jdbc/web_student_tracker"
              auth="Container"
              type="javax.sql.DataSource"
              maxActive="20"
              maxIdle="5"
              maxWait="10000"
              username="webstudent"
              password="webstudent"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://localhost:3306/web_student_tracker"/>
</Context>
```

**Giải Thích**:
- `name="jdbc/web_student_tracker"`: Đây là tên của connection pool mà chúng ta sẽ sử dụng trong mã nguồn Java.
- `auth="Container"`: Quản lý việc xác thực (authentication) bằng Tomcat container.
- `maxActive="20"`: Số lượng kết nối tối đa trong pool.
- `username`, `password`: Thông tin đăng nhập của người dùng cơ sở dữ liệu.
- `driverClassName`: Lớp trình điều khiển JDBC của MySQL.
- `url`: Đường dẫn kết nối tới cơ sở dữ liệu.

---

### Bước 4: Tạo Một Servlet Để Kiểm Tra Kết Nối Cơ Sở Dữ Liệu
1. Trong thư mục `src`, tạo một **Java package** mới với tên `com.luv2code.web.jdbc`.
2. Tạo một servlet mới với tên `TestServlet`.
3. Cấu hình servlet này với URL mapping là `/TestServlet`.
4. Chỉnh sửa servlet để thực hiện kết nối và truy vấn dữ liệu như sau:

```java
package com.luv2code.web.jdbc;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
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

    // Khai báo resource với tên đã định nghĩa trong context.xml
    @Resource(name="jdbc/web_student_tracker")
    private DataSource dataSource;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // Thiết lập response
        response.setContentType("text/plain");
        PrintWriter out = response.getWriter();

        Connection myConn = null;
        Statement myStmt = null;
        ResultSet myRs = null;

        try {
            // Lấy connection từ connection pool
            myConn = dataSource.getConnection();

            // Tạo một câu lệnh SQL
            String sql = "SELECT email FROM student";
            myStmt = myConn.createStatement();

            // Thực thi câu lệnh SQL
            myRs = myStmt.executeQuery(sql);

            // Xử lý kết quả
            while (myRs.next()) {
                String email = myRs.getString("email");
                out.println(email);
            }
        } catch (Exception exc) {
            exc.printStackTrace();
        } finally {
            // Đóng các tài nguyên
            try { if (myRs != null) myRs.close(); } catch (Exception e) {}
            try { if (myStmt != null) myStmt.close(); } catch (Exception e) {}
            try { if (myConn != null) myConn.close(); } catch (Exception e) {}
        }
    }
}
```

**Giải Thích**:
- `@Resource(name="jdbc/web_student_tracker")`: Định nghĩa biến `dataSource` sẽ nhận đối tượng connection pool từ Tomcat.
- **`dataSource.getConnection()`**: Lấy một kết nối từ connection pool.
- **Thực hiện truy vấn và in kết quả**: Truy vấn tất cả các email từ bảng `student` và hiển thị chúng trên trang web dưới dạng văn bản.

---

### Bước 5: Kiểm Tra Kết Nối Và Chạy Ứng Dụng
1. Nhấn chuột phải vào `TestServlet.java` và chọn **Run As > Run on Server**.
2. Chọn server Tomcat và nhấn **Finish**.
3. Mở trình duyệt và truy cập vào URL `http://localhost:8080/WebStudentTracker/TestServlet`.
4. Kiểm tra kết quả hiển thị:
   - Nếu hiển thị danh sách email sinh viên, điều đó có nghĩa là kết nối tới cơ sở dữ liệu đã thành công.

---

### Kết Luận
Với các bước trên, chúng ta đã xây dựng thành công một ứng dụng web sử dụng JSP và Servlet để kết nối với cơ sở dữ liệu MySQL thông qua JDBC. Bài viết không chỉ giúp bạn hiểu cách thiết lập cơ sở dữ liệu mà còn cung cấp cách kiểm tra kết nối và lấy dữ liệu từ cơ sở dữ liệu.

Trong những phần tiếp theo, chúng ta sẽ tiếp tục phát triển ứng dụng với các tính năng như thêm, sửa, xóa sinh viên và cải thiện giao diện hiển thị với JSP và HTML. Chúc bạn thực hành thành công và tiếp tục khám phá các phần tiếp theo! 😄
