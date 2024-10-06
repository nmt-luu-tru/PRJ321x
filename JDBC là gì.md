## JDBC là gì?

### 1. Giới thiệu về JDBC

**JDBC (Java Database Connectivity)** là một API (Application Programming Interface) của Java, cung cấp các phương thức và lớp cần thiết để kết nối, tương tác với các cơ sở dữ liệu (Database). Nó là cầu nối giữa các ứng dụng Java và cơ sở dữ liệu, giúp lập trình viên thực hiện các thao tác như truy vấn, thêm, xóa, cập nhật dữ liệu một cách dễ dàng mà không cần lo lắng về chi tiết cụ thể của việc giao tiếp với từng loại cơ sở dữ liệu.

JDBC là một phần của thư viện tiêu chuẩn Java, được phát triển bởi Sun Microsystems (nay là một phần của Oracle) và hiện tại là một trong những API chính trong Java để làm việc với cơ sở dữ liệu.

### 2. Kiến trúc của JDBC

JDBC sử dụng một kiến trúc gồm ba lớp:

1. **Driver Layer (Tầng điều khiển)**:
   - Đây là lớp đầu tiên và chịu trách nhiệm liên kết với cơ sở dữ liệu. Lớp này sử dụng một `JDBC Driver` (trình điều khiển JDBC) để giao tiếp với cơ sở dữ liệu tương ứng.
   - Mỗi loại cơ sở dữ liệu sẽ có một JDBC Driver riêng (ví dụ: MySQL, Oracle, SQL Server...). JDBC Driver là một tệp `.jar` cần được thêm vào dự án Java để có thể kết nối đến cơ sở dữ liệu.

2. **Connection Layer (Tầng kết nối)**:
   - Tầng này cung cấp các phương thức và lớp để mở kết nối, gửi truy vấn, và nhận phản hồi từ cơ sở dữ liệu.
   - Các lớp phổ biến trong tầng này bao gồm `Connection`, `Statement`, `PreparedStatement`, và `ResultSet`.

3. **Application Layer (Tầng ứng dụng)**:
   - Đây là lớp cao nhất, chứa mã nguồn của ứng dụng, thực hiện các thao tác truy vấn, tương tác với dữ liệu thông qua tầng kết nối mà không cần biết chi tiết cụ thể về tầng điều khiển.

### 3. Cách hoạt động của JDBC

1. **Tải JDBC Driver**:
   - Trước tiên, bạn cần tải và đăng ký JDBC Driver cho cơ sở dữ liệu mà bạn muốn kết nối. Điều này thường được thực hiện bằng cách thêm tệp `.jar` của driver vào dự án và nạp driver trong mã nguồn bằng cách sử dụng `Class.forName()` (áp dụng cho các phiên bản Java cũ) hoặc thông qua `DataSource` (trong các ứng dụng Java mới).

   ```java
   Class.forName("com.mysql.cj.jdbc.Driver");
   ```

2. **Mở kết nối đến cơ sở dữ liệu**:
   - Sử dụng lớp `DriverManager` để mở kết nối đến cơ sở dữ liệu bằng phương thức `getConnection()` với URL, tên người dùng và mật khẩu.

   ```java
   Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "username", "password");
   ```

3. **Gửi câu lệnh SQL**:
   - Sử dụng đối tượng `Statement` hoặc `PreparedStatement` để gửi các câu lệnh SQL (như `SELECT`, `INSERT`, `UPDATE`, `DELETE`).

   ```java
   Statement stmt = conn.createStatement();
   ResultSet rs = stmt.executeQuery("SELECT * FROM students");
   ```

4. **Xử lý kết quả**:
   - Kết quả của các truy vấn sẽ được lưu trong đối tượng `ResultSet`. Bạn có thể duyệt qua `ResultSet` để lấy dữ liệu.

   ```java
   while (rs.next()) {
       System.out.println("Student ID: " + rs.getInt("id"));
       System.out.println("Student Name: " + rs.getString("name"));
   }
   ```

5. **Đóng kết nối**:
   - Sau khi hoàn tất các thao tác, bạn cần đóng các kết nối để giải phóng tài nguyên.

   ```java
   rs.close();
   stmt.close();
   conn.close();
   ```

### 4. Các loại JDBC Driver

JDBC cung cấp 4 loại Driver:

1. **Type 1: JDBC-ODBC Bridge Driver**:
   - Sử dụng cầu nối ODBC để kết nối từ JDBC đến cơ sở dữ liệu. Loại driver này hiện đã bị loại bỏ trong các phiên bản Java mới.

2. **Type 2: Native-API Driver**:
   - Sử dụng các thư viện C/C++ để giao tiếp trực tiếp với cơ sở dữ liệu. Loại driver này ít phổ biến và phụ thuộc vào nền tảng.

3. **Type 3: Network Protocol Driver**:
   - Chuyển đổi các lệnh JDBC thành giao thức mạng độc lập với cơ sở dữ liệu. Loại driver này có tính di động cao và thường được sử dụng trong các hệ thống lớn.

4. **Type 4: Thin Driver**:
   - Chuyển đổi trực tiếp các lệnh JDBC thành giao thức cơ sở dữ liệu. Đây là loại driver phổ biến nhất và có hiệu năng cao, độc lập với nền tảng.

### 5. Tại sao cần sử dụng JDBC Driver?

- Mỗi cơ sở dữ liệu có cách giao tiếp riêng, vì vậy một JDBC Driver cụ thể sẽ giúp chuyển đổi các lệnh JDBC thành các lệnh mà cơ sở dữ liệu hiểu được.
- Sử dụng JDBC Driver giúp mã nguồn Java của bạn có thể kết nối và tương tác với nhiều loại cơ sở dữ liệu mà không cần phải viết lại mã nguồn.

### 6. Các ví dụ và hướng dẫn cài đặt JDBC Driver

Để cài đặt JDBC Driver, bạn cần thực hiện theo các bước sau:

1. Tải tệp `.jar` của JDBC Driver từ trang web của nhà cung cấp cơ sở dữ liệu (ví dụ: MySQL Connector/J cho MySQL, Oracle JDBC Driver cho Oracle).
2. Thêm tệp `.jar` vào dự án Java của bạn (bằng cách cấu hình trong IDE như Eclipse hoặc NetBeans).
3. Đăng ký driver và mở kết nối đến cơ sở dữ liệu trong mã nguồn.

**Ví dụ**: Đăng ký và kết nối đến cơ sở dữ liệu MySQL:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class DatabaseExample {
    public static void main(String[] args) {
        try {
            // Đăng ký JDBC Driver cho MySQL
            Class.forName("com.mysql.cj.jdbc.Driver");

            // Mở kết nối
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "username", "password");

            // Tạo đối tượng Statement
            Statement stmt = conn.createStatement();

            // Gửi truy vấn và xử lý kết quả
            ResultSet rs = stmt.executeQuery("SELECT * FROM students");
            while (rs.next()) {
                System.out.println("Student ID: " + rs.getInt("id"));
                System.out.println("Student Name: " + rs.getString("name"));
            }

            // Đóng kết nối
            rs.close();
            stmt.close();
            conn.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Trong ví dụ trên, chương trình sẽ kết nối đến cơ sở dữ liệu MySQL và hiển thị danh sách các sinh viên trong bảng `students`.

---

Đó là toàn bộ những điều cơ bản về JDBC, từ khái niệm đến cách sử dụng trong lập trình. Việc sử dụng JDBC giúp cho việc kết nối và quản lý cơ sở dữ liệu trở nên đơn giản hơn trong các ứng dụng Java.
