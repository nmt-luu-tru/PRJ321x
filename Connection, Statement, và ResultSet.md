Trong lập trình JDBC (Java Database Connectivity), khi làm việc với cơ sở dữ liệu, chúng ta sử dụng một số đối tượng quan trọng để quản lý kết nối, thực hiện các truy vấn và lấy kết quả từ cơ sở dữ liệu. Ba đối tượng cốt lõi trong quá trình này là: **`Connection`**, **`Statement`**, và **`ResultSet`**. Hãy cùng tìm hiểu sâu hơn về vai trò và cách hoạt động của chúng.

### 1. **Đối tượng `Connection`**
#### Mục đích:
- **`Connection`** đại diện cho kết nối giữa ứng dụng Java và cơ sở dữ liệu. Nó cho phép ứng dụng giao tiếp với cơ sở dữ liệu, thực hiện các truy vấn SQL, cập nhật dữ liệu, và quản lý giao dịch.

#### Cách tạo:
Để tạo một kết nối, chúng ta thường sử dụng phương thức `getConnection()` của lớp `DriverManager` hoặc lấy kết nối từ một **connection pool** (nếu sử dụng).

```java
Connection conn = DriverManager.getConnection(url, user, password);
```

Hoặc lấy kết nối từ **Connection Pool**:
```java
// Tạo nguồn dữ liệu (DataSource) từ connection pool được cấu hình trong server (ví dụ Tomcat)
Context ctx = new InitialContext();
DataSource ds = (DataSource) ctx.lookup("java:comp/env/jdbc/myDataSource");

// Lấy kết nối từ pool
Connection conn = ds.getConnection();
```
> Lưu ý: Việc sử dụng connection pool giúp tối ưu hóa hiệu suất bằng cách tái sử dụng các kết nối thay vì tạo và đóng kết nối liên tục, giúp giảm tải cho cơ sở dữ liệu và tăng khả năng đáp ứng của ứng dụng.

#### Các tính năng quan trọng:
- **Quản lý kết nối với cơ sở dữ liệu**: Đối tượng `Connection` giúp thiết lập kết nối với cơ sở dữ liệu thông qua thông tin URL, username, và password.
- **Giao dịch (Transaction Management)**: Có thể quản lý giao dịch cơ sở dữ liệu bằng cách bật hoặc tắt chế độ **auto-commit** (mặc định là auto-commit).
  - `conn.setAutoCommit(false);` để tắt chế độ tự động commit.
  - Sau đó có thể sử dụng `conn.commit();` hoặc `conn.rollback();` để xác nhận hoặc hủy bỏ các thay đổi.
- **Đóng kết nối**: Khi hoàn thành công việc với cơ sở dữ liệu, cần phải đóng kết nối để giải phóng tài nguyên.
  ```java
  conn.close();
  ```
> Lưu ý: Khi sử dụng connection pool, phương thức `conn.close()` sẽ không thực sự đóng kết nối mà chỉ đưa kết nối trở lại pool, để nó có thể được tái sử dụng bởi các yêu cầu sau này.

### 2. **Đối tượng `Statement`**
#### Mục đích:
- **`Statement`** được sử dụng để gửi các câu lệnh SQL tới cơ sở dữ liệu từ ứng dụng Java. Nó thực hiện truy vấn và trả về kết quả hoặc cập nhật cơ sở dữ liệu.

#### Các loại `Statement`:
1. **`Statement`**: Được sử dụng để thực hiện các truy vấn SQL đơn giản mà không có tham số.
   ```java
   Statement stmt = conn.createStatement();
   ResultSet rs = stmt.executeQuery("SELECT * FROM students");
   ```

2. **`PreparedStatement`**: Sử dụng cho các truy vấn có tham số, giúp tăng hiệu suất và bảo mật (tránh SQL Injection).
   ```java
   PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM students WHERE id = ?");
   pstmt.setInt(1, 123);
   ResultSet rs = pstmt.executeQuery();
   ```

3. **`CallableStatement`**: Dùng để gọi các thủ tục (stored procedures) trong cơ sở dữ liệu.
   ```java
   CallableStatement cstmt = conn.prepareCall("{call getStudent(?)}");
   cstmt.setInt(1, 123);
   ResultSet rs = cstmt.executeQuery();
   ```

#### Phương thức chính:
- **`executeQuery()`**: Sử dụng cho các truy vấn SQL trả về dữ liệu (như `SELECT`).
  ```java
  ResultSet rs = stmt.executeQuery("SELECT * FROM students");
  ```

- **`executeUpdate()`**: Sử dụng cho các câu lệnh SQL thay đổi dữ liệu trong cơ sở dữ liệu (như `INSERT`, `UPDATE`, `DELETE`), trả về số hàng bị ảnh hưởng.
  ```java
  int rowsAffected = stmt.executeUpdate("UPDATE students SET email='test@example.com' WHERE id=1");
  ```

- **`execute()`**: Sử dụng cho các câu lệnh SQL không rõ ràng về loại (dùng khi không chắc chắn truy vấn sẽ trả về dữ liệu hay cập nhật dữ liệu).
  ```java
  boolean hasResultSet = stmt.execute("SELECT * FROM students");
  ```

### 3. **Đối tượng `ResultSet`**
#### Mục đích:
- **`ResultSet`** đại diện cho tập kết quả của một truy vấn SQL `SELECT`. Nó cho phép bạn duyệt qua các hàng dữ liệu được trả về từ cơ sở dữ liệu.

#### Cách duyệt dữ liệu:
- `ResultSet` có một con trỏ bắt đầu từ trước hàng đầu tiên. Bạn sử dụng phương thức `next()` để di chuyển con trỏ đến hàng tiếp theo.

```java
while (rs.next()) {
    int id = rs.getInt("id");
    String firstName = rs.getString("first_name");
    String email = rs.getString("email");
    // Xử lý dữ liệu...
}
```

#### Các phương thức quan trọng:
- **`getInt()`**, **`getString()`**, **`getDouble()`**,...: Các phương thức này được dùng để lấy dữ liệu từ các cột của hàng hiện tại. Bạn có thể truy xuất theo tên cột hoặc chỉ số cột (bắt đầu từ 1).

- **`next()`**: Di chuyển con trỏ đến hàng tiếp theo. Trả về `true` nếu còn hàng, và `false` nếu hết dữ liệu.

- **`close()`**: Sau khi xử lý xong dữ liệu, cần đóng `ResultSet` để giải phóng tài nguyên.

#### Duyệt `ResultSet` theo nhiều hướng:
- Mặc định, `ResultSet` chỉ duyệt tiến (forward-only), nhưng có thể cấu hình để duyệt ngược hoặc di chuyển tới một hàng cụ thể với các `ResultSet` có kiểu cuộn (scrollable):
  ```java
  Statement stmt = conn.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_READ_ONLY);
  ResultSet rs = stmt.executeQuery("SELECT * FROM students");

  rs.first();  // Di chuyển đến hàng đầu tiên
  rs.last();   // Di chuyển đến hàng cuối cùng
  rs.previous(); // Di chuyển ngược lại
  ```

### Tóm tắt sự liên kết giữa các đối tượng:
- **`Connection`**: Quản lý kết nối đến cơ sở dữ liệu.
- **`Statement`**: Gửi các câu lệnh SQL từ ứng dụng đến cơ sở dữ liệu.
- **`ResultSet`**: Chứa dữ liệu trả về từ câu lệnh `SELECT`, cho phép duyệt và truy xuất dữ liệu.

### Quy trình cơ bản:
1. **Mở kết nối** với cơ sở dữ liệu bằng `Connection`.
2. **Tạo và thực hiện câu lệnh SQL** với `Statement` hoặc `PreparedStatement`.
3. **Lấy kết quả** (nếu có) bằng `ResultSet` và duyệt qua các hàng dữ liệu.
4. **Đóng các tài nguyên** (`ResultSet`, `Statement`, `Connection`) để giải phóng tài nguyên.

### Lấy kết nối từ Connection Pool:
- Trong các ứng dụng lớn hoặc có nhiều người dùng, việc sử dụng **Connection Pool** là rất quan trọng. **Connection Pool** là một tập hợp các kết nối đã được mở sẵn để tái sử dụng nhiều lần, giúp giảm thời gian tạo kết nối mới và tăng hiệu suất của ứng dụng.

- Khi bạn lấy một kết nối từ Connection Pool:
  - Bạn không cần tạo mới một kết nối từ đầu (tránh mất thời gian và tài nguyên).
  - Kết nối đã được mở sẵn và có thể sử dụng ngay lập tức.
  - Sau khi sử dụng xong, thay vì đóng kết nối (`close()`), bạn chỉ cần trả lại kết nối đó về Pool để nó có thể được tái sử dụng.

#### Cách lấy kết nối từ Connection Pool:
- Trong Java, bạn có thể sử dụng một số thư viện và framework hỗ trợ việc tạo và quản lý Connection Pool, như **Apache DBCP**, **C3P0**, hoặc **HikariCP**. Trong ứng dụng Java web với Tomcat hoặc các server khác, ta có thể cấu hình Connection Pool trong file `context.xml` hoặc `web.xml`, sau đó sử dụng `DataSource` để lấy kết nối:

```java
// Tạo nguồn dữ liệu từ Connection Pool được định nghĩa trong context.xml
DataSource ds = (DataSource) new InitialContext().lookup("java:comp/env/jdbc/myDB");

// Lấy kết nối từ pool
Connection conn = ds.getConnection();
```

- Khi bạn gọi `conn.close()`, kết nối này sẽ không thực sự bị đóng mà sẽ được trả lại Connection Pool để sử dụng cho các yêu cầu tiếp theo.

Việc hiểu rõ và quản lý tốt các đối tượng này là chìa khóa để xây

 dựng các ứng dụng cơ sở dữ liệu hiệu quả và đáng tin cậy trong Java.
