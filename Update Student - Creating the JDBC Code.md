### Cập Nhật `StudentDbUtil` Với Phương Thức `updateStudent`

Trong bước này, chúng ta sẽ thêm mã lệnh JDBC vào phương thức `updateStudent` trong lớp `StudentDbUtil`. Phương thức này sẽ thực hiện câu lệnh `UPDATE` trên cơ sở dữ liệu để cập nhật thông tin sinh viên dựa trên dữ liệu được cung cấp từ form.

#### **1. Cấu Trúc Cơ Bản Của Phương Thức `updateStudent`**

Phương thức `updateStudent` sẽ làm những nhiệm vụ chính sau:

1. **Lấy kết nối đến cơ sở dữ liệu**: Sử dụng `DataSource` để lấy `Connection`.
2. **Tạo câu lệnh SQL `UPDATE`**: Câu lệnh `UPDATE` sẽ cập nhật thông tin sinh viên theo `id`.
3. **Chuẩn bị `PreparedStatement` và thiết lập tham số**: Sử dụng các phương thức như `setString()` và `setInt()` để thiết lập các giá trị tương ứng với các tham số trong câu lệnh SQL.
4. **Thực thi câu lệnh SQL**: Gọi `execute()` để thực hiện cập nhật dữ liệu trong cơ sở dữ liệu.
5. **Đóng kết nối và tài nguyên JDBC**: Đảm bảo rằng tất cả các kết nối và tài nguyên JDBC được đóng đúng cách để tránh rò rỉ tài nguyên.

#### **2. Viết Mã Lệnh Cho Phương Thức `updateStudent`**

**Cập nhật phương thức `updateStudent` trong lớp `StudentDbUtil` như sau:**

```java
public void updateStudent(Student theStudent) throws Exception {
    Connection myConn = null;
    PreparedStatement myStmt = null;

    try {
        // 1. Lấy kết nối từ DataSource
        myConn = dataSource.getConnection();

        // 2. Tạo câu lệnh SQL để cập nhật thông tin sinh viên
        String sql = "UPDATE student SET first_name=?, last_name=?, email=? WHERE id=?";

        // 3. Chuẩn bị câu lệnh
        myStmt = myConn.prepareStatement(sql);

        // 4. Thiết lập tham số cho câu lệnh SQL
        myStmt.setString(1, theStudent.getFirstName());
        myStmt.setString(2, theStudent.getLastName());
        myStmt.setString(3, theStudent.getEmail());
        myStmt.setInt(4, theStudent.getId());

        // 5. Thực thi câu lệnh SQL
        myStmt.execute();
    } finally {
        // 6. Đóng các tài nguyên JDBC
        close(myConn, myStmt, null);
    }
}
```

#### **3. Giải Thích Từng Bước Mã Lệnh**

1. **Lấy kết nối từ `DataSource`**:

   ```java
   myConn = dataSource.getConnection();
   ```

   - Sử dụng `DataSource` để lấy kết nối đến cơ sở dữ liệu.

2. **Tạo câu lệnh SQL `UPDATE`**:

   ```java
   String sql = "UPDATE student SET first_name=?, last_name=?, email=? WHERE id=?";
   ```

   - Câu lệnh `UPDATE` sẽ cập nhật cột `first_name`, `last_name`, và `email` của bảng `student` dựa trên giá trị `id` của sinh viên.

3. **Chuẩn bị `PreparedStatement` và thiết lập tham số**:

   ```java
   myStmt = myConn.prepareStatement(sql);
   ```

   - `myStmt` là đối tượng `PreparedStatement` để thực thi câu lệnh SQL `UPDATE`.

   ```java
   myStmt.setString(1, theStudent.getFirstName());
   myStmt.setString(2, theStudent.getLastName());
   myStmt.setString(3, theStudent.getEmail());
   myStmt.setInt(4, theStudent.getId());
   ```

   - Thiết lập các tham số cho câu lệnh SQL bằng cách sử dụng `theStudent`:
     - `setString(1, ...)` thiết lập `first_name`.
     - `setString(2, ...)` thiết lập `last_name`.
     - `setString(3, ...)` thiết lập `email`.
     - `setInt(4, ...)` thiết lập `id` của sinh viên cần cập nhật.

4. **Thực thi câu lệnh `UPDATE`**:

   ```java
   myStmt.execute();
   ```

   - Gọi `execute()` để thực thi câu lệnh SQL.

5. **Đóng các tài nguyên JDBC**:

   ```java
   close(myConn, myStmt, null);
   ```

   - Đảm bảo đóng tất cả tài nguyên như `Connection`, `PreparedStatement` và `ResultSet` (nếu có). Trong trường hợp này, `ResultSet` là `null` vì không có kết quả trả về từ câu lệnh `UPDATE`.

#### **4. Kiểm Tra Tính Năng Cập Nhật**

Sau khi hoàn tất việc triển khai phương thức `updateStudent`, chúng ta có thể thực hiện các bước kiểm tra sau để đảm bảo tính năng hoạt động đúng cách.

**Các bước kiểm tra:**

1. **Khởi động lại ứng dụng:**

   - Đảm bảo rằng các thay đổi trong mã đã được biên dịch và áp dụng.

2. **Truy cập trang danh sách sinh viên:**

   - Kiểm tra xem danh sách sinh viên có hiển thị đúng không.

3. **Chọn một sinh viên và nhấp vào "Update":**

   - Nhấp vào liên kết "Update" bên cạnh tên của sinh viên để mở form cập nhật.

4. **Thay đổi thông tin sinh viên:**

   - Cập nhật `First Name`, `Last Name`, và `Email` của sinh viên.

5. **Nhấn nút "Save":**

   - Gửi form và đảm bảo rằng bạn được chuyển hướng trở lại trang danh sách sinh viên.

6. **Kiểm tra kết quả:**

   - Xác nhận rằng thông tin sinh viên đã được cập nhật trong danh sách và cũng có thể kiểm tra trực tiếp trong cơ sở dữ liệu.

**Ví dụ:**

- **Trước khi cập nhật:**

  - Sinh viên: **John Doe**
  - Email: **john@example.com**

- **Cập nhật thông tin:**

  - Thay đổi `First Name` thành **Johnny**
  - Thay đổi `Email` thành **johnny@example.com**

- **Sau khi cập nhật:**

  - Danh sách sinh viên hiển thị **Johnny Doe** với email mới.

#### **5. Kết Luận**

- Chúng ta đã hoàn tất việc cập nhật phương thức `updateStudent` trong `StudentDbUtil`.
- Phương thức này thực hiện việc cập nhật thông tin sinh viên trong cơ sở dữ liệu dựa trên các dữ liệu được cung cấp từ form `update-student-form.jsp`.
- Giờ đây, tính năng cập nhật sinh viên đã hoàn chỉnh và có thể sử dụng để chỉnh sửa thông tin sinh viên từ giao diện người dùng.

### **Tiếp Theo**

- **Thêm thông báo xác nhận cập nhật thành công**: Hiển thị thông báo cho người dùng biết rằng việc cập nhật đã thành công.
- **Kiểm tra và xử lý các lỗi tiềm ẩn**: Đảm bảo rằng các lỗi liên quan đến kết nối cơ sở dữ liệu và lỗi cập nhật dữ liệu đều được xử lý một cách chính xác.
