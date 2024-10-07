### Đối Tượng `DataSource` Trong Java

`DataSource` là một interface được định nghĩa trong gói **javax.sql** của Java, cung cấp một cách tiếp cận tiêu chuẩn để quản lý và sử dụng các kết nối cơ sở dữ liệu. `DataSource` thường được sử dụng trong các ứng dụng Java EE (Enterprise Edition) hoặc Jakarta EE để thay thế cho cách kết nối cơ sở dữ liệu truyền thống qua `DriverManager`.

Dưới đây là giải thích chi tiết về đối tượng `DataSource`:

---

### 1. **Khái Niệm `DataSource`**
`DataSource` là một đối tượng đại diện cho nguồn dữ liệu (thường là cơ sở dữ liệu) và là một thành phần quan trọng trong quản lý kết nối cơ sở dữ liệu. Thay vì phải sử dụng `DriverManager` để tạo kết nối trực tiếp với cơ sở dữ liệu, `DataSource` cung cấp một phương thức trừu tượng giúp bạn dễ dàng quản lý và sử dụng các kết nối.

Các chức năng chính của `DataSource` bao gồm:

- **Quản lý kết nối cơ sở dữ liệu**: Thay vì tạo kết nối mới mỗi khi cần, `DataSource` có thể sử dụng kết nối từ một **connection pool** (bể kết nối) đã được tạo sẵn.
- **Hỗ trợ kết nối bảo mật**: `DataSource` hỗ trợ quản lý thông tin đăng nhập an toàn và cho phép cấu hình bảo mật.
- **Quản lý tài nguyên**: `DataSource` giúp quản lý và tối ưu hóa tài nguyên của hệ thống, đặc biệt là trong các ứng dụng lớn.

---

### 2. **Vì Sao Nên Sử Dụng `DataSource`?**

- **Dễ Quản Lý**: `DataSource` cho phép quản lý tất cả các thuộc tính của kết nối tại một nơi duy nhất (ví dụ như tệp cấu hình), giúp thay đổi cấu hình kết nối dễ dàng mà không cần phải thay đổi mã nguồn.
  
- **Tái Sử Dụng Kết Nối**: Sử dụng `DataSource` cùng với một connection pool giúp tối ưu hóa hiệu suất bằng cách tái sử dụng các kết nối đã được tạo sẵn, thay vì tạo mới mỗi lần cần kết nối cơ sở dữ liệu.

- **Hỗ Trợ Connection Pooling**: `DataSource` cung cấp khả năng quản lý connection pool một cách hiệu quả. Connection pooling giúp giảm thiểu chi phí tài nguyên và thời gian chờ khi tạo kết nối mới.

- **Tăng Cường Bảo Mật**: `DataSource` cho phép quản lý bảo mật tập trung, bao gồm việc quản lý thông tin đăng nhập và sử dụng các kỹ thuật bảo mật kết nối cơ sở dữ liệu.

---

### 3. **Các Phương Thức Chính Của `DataSource`**

Interface `DataSource` định nghĩa các phương thức mà bạn có thể sử dụng để lấy kết nối đến cơ sở dữ liệu:

1. **`Connection getConnection()`**: Phương thức này sẽ trả về một đối tượng `Connection` đã được kết nối tới cơ sở dữ liệu mà không cần thông tin người dùng và mật khẩu.

   ```java
   Connection conn = dataSource.getConnection();
   ```

2. **`Connection getConnection(String username, String password)`**: Phương thức này trả về một đối tượng `Connection` với thông tin đăng nhập của người dùng cụ thể.

   ```java
   Connection conn = dataSource.getConnection("myUser", "myPassword");
   ```

3. **`PrintWriter getLogWriter()`**: Lấy đối tượng `PrintWriter` dùng để ghi log thông tin từ `DataSource`.

4. **`void setLogWriter(PrintWriter out)`**: Cài đặt `PrintWriter` để ghi log.

5. **`int getLoginTimeout()`**: Lấy thời gian chờ kết nối của `DataSource`.

6. **`void setLoginTimeout(int seconds)`**: Cài đặt thời gian chờ kết nối cho `DataSource`.

Các phương thức trên cho phép bạn dễ dàng lấy kết nối, thiết lập thời gian chờ và cấu hình log một cách dễ dàng hơn.

---

### 4. **Các Loại `DataSource`**

Có ba loại `DataSource` chính:

1. **Basic DataSource**:
   - Cung cấp các kết nối trực tiếp đến cơ sở dữ liệu mà không sử dụng connection pool.
   - Thường chỉ sử dụng cho các ứng dụng đơn giản hoặc trong môi trường thử nghiệm.

2. **Connection Pool DataSource**:
   - Cung cấp các kết nối từ một connection pool.
   - Connection pool chứa các kết nối đã được tạo sẵn và tái sử dụng để tăng hiệu năng.
   - Phù hợp cho các ứng dụng web và hệ thống lớn, nơi mà hiệu năng và tài nguyên cần được tối ưu hóa.

3. **Distributed DataSource (XA DataSource)**:
   - Được sử dụng trong các giao dịch phân tán (distributed transactions).
   - Phù hợp cho các ứng dụng phức tạp, nơi cần thực hiện các giao dịch trên nhiều nguồn dữ liệu khác nhau.

---

### 5. **Ví Dụ Sử Dụng `DataSource`**
Ví dụ sau minh họa cách sử dụng `DataSource` để lấy một kết nối đến cơ sở dữ liệu và thực hiện một truy vấn đơn giản:

```java
import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

public class DataSourceExample {
    private DataSource dataSource;

    // Constructor nhận vào một DataSource
    public DataSourceExample(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void printStudentNames() {
        try (Connection conn = dataSource.getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT first_name, last_name FROM student")) {

            while (rs.next()) {
                System.out.println("Student: " + rs.getString("first_name") + " " + rs.getString("last_name"));
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Giải thích**:
- Đầu tiên, `DataSource` được truyền vào qua Constructor `DataSourceExample`.
- Phương thức `printStudentNames()` lấy một kết nối (`Connection`) từ `DataSource`.
- Sau đó, `Statement` và `ResultSet` được sử dụng để thực thi truy vấn và lấy dữ liệu từ bảng `student`.
- Kết nối được đóng tự động nhờ `try-with-resources`.

---

### 6. **Cách Tạo `DataSource`**
Bạn có thể tạo một `DataSource` thông qua các cách khác nhau như:
- Cấu hình `DataSource` trong tệp `context.xml` của Tomcat.
- Sử dụng các thư viện bên thứ ba như **Apache DBCP** hoặc **HikariCP**.
- Khởi tạo `DataSource` bằng mã nguồn Java (ít phổ biến hơn).

Ví dụ cấu hình `DataSource` trong Tomcat (`context.xml`):

```xml
<Resource name="jdbc/mydb"
          auth="Container"
          type="javax.sql.DataSource"
          maxTotal="20"
          maxIdle="10"
          maxWaitMillis="-1"
          username="dbuser"
          password="dbpassword"
          driverClassName="com.mysql.cj.jdbc.Driver"
          url="jdbc:mysql://localhost:3306/mydb"/>
```

---

### 7. **Kết Luận**
`DataSource` là một interface mạnh mẽ trong Java, giúp quản lý và kết nối cơ sở dữ liệu một cách dễ dàng và hiệu quả. Việc sử dụng `DataSource` cùng với connection pooling giúp tối ưu hóa tài nguyên và tăng hiệu suất cho các ứng dụng lớn. Việc hiểu rõ về `DataSource` là nền tảng quan trọng để xây dựng các ứng dụng Java EE hiệu quả và dễ bảo trì hơn.
