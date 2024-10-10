### Cập Nhật `StudentDbUtil` Để Thêm Sinh Viên Vào Cơ Sở Dữ Liệu

Trong bước cuối cùng này, chúng ta sẽ hoàn thiện tính năng thêm sinh viên bằng cách cập nhật phương thức `addStudent` trong lớp `StudentDbUtil`. Đây là nơi chúng ta sẽ viết mã JDBC thực hiện việc chèn thông tin sinh viên mới vào cơ sở dữ liệu.

#### **1. Mục Tiêu**

- **Hoàn thiện phương thức `addStudent`**: Viết mã JDBC để chèn một bản ghi sinh viên mới vào bảng `student` trong cơ sở dữ liệu.
- **Kết nối cơ sở dữ liệu**: Sử dụng kết nối từ `DataSource` để thực hiện các thao tác với cơ sở dữ liệu.
- **Thực thi câu lệnh SQL**: Sử dụng `PreparedStatement` để thực thi câu lệnh SQL `INSERT`.
- **Đảm bảo quản lý tài nguyên đúng cách**: Đóng các kết nối và tài nguyên JDBC sau khi hoàn thành.

#### **2. Cập Nhật Phương Thức `addStudent` Trong `StudentDbUtil`**

**Mã Nguồn Hoàn Chỉnh Cho Phương Thức `addStudent`:**

```java
public void addStudent(Student theStudent) throws Exception {
    Connection myConn = null;
    PreparedStatement myStmt = null;

    try {
        // Lấy kết nối từ DataSource
        myConn = dataSource.getConnection();

        // Tạo câu lệnh SQL cho việc chèn dữ liệu
        String sql = "INSERT INTO student "
                   + "(first_name, last_name, email) "
                   + "VALUES (?, ?, ?)";

        // Chuẩn bị câu lệnh SQL
        myStmt = myConn.prepareStatement(sql);

        // Thiết lập các giá trị cho tham số
        myStmt.setString(1, theStudent.getFirstName());
        myStmt.setString(2, theStudent.getLastName());
        myStmt.setString(3, theStudent.getEmail());

        // Thực thi câu lệnh SQL
        myStmt.execute();
    } finally {
        // Đóng các tài nguyên JDBC
        close(myConn, myStmt, null);
    }
}
```

**Giải Thích Chi Tiết:**

1. **Khai Báo Các Biến JDBC:**

   ```java
   Connection myConn = null;
   PreparedStatement myStmt = null;
   ```

   - **`Connection myConn`**: Đối tượng kết nối đến cơ sở dữ liệu.
   - **`PreparedStatement myStmt`**: Đối tượng thực thi câu lệnh SQL với các tham số.

2. **Lấy Kết Nối Từ `DataSource`:**

   ```java
   myConn = dataSource.getConnection();
   ```

   - Sử dụng `dataSource` để lấy một kết nối từ connection pool.

3. **Tạo Câu Lệnh SQL:**

   ```java
   String sql = "INSERT INTO student "
              + "(first_name, last_name, email) "
              + "VALUES (?, ?, ?)";
   ```

   - Câu lệnh SQL sử dụng các dấu `?` để đại diện cho các tham số sẽ được thiết lập sau.

4. **Chuẩn Bị Câu Lệnh SQL:**

   ```java
   myStmt = myConn.prepareStatement(sql);
   ```

   - Chuẩn bị `PreparedStatement` với câu lệnh SQL đã định nghĩa.

5. **Thiết Lập Giá Trị Cho Các Tham Số:**

   ```java
   myStmt.setString(1, theStudent.getFirstName());
   myStmt.setString(2, theStudent.getLastName());
   myStmt.setString(3, theStudent.getEmail());
   ```

   - **Tham số 1** (`?` đầu tiên): Giá trị của `first_name`.
   - **Tham số 2** (`?` thứ hai): Giá trị của `last_name`.
   - **Tham số 3** (`?` thứ ba): Giá trị của `email`.

   > **Lưu ý:** Chỉ số của các tham số trong `PreparedStatement` bắt đầu từ **1**, không phải **0**.

6. **Thực Thi Câu Lệnh SQL:**

   ```java
   myStmt.execute();
   ```

   - Thực thi câu lệnh `INSERT` để thêm sinh viên vào cơ sở dữ liệu.

7. **Đóng Các Tài Nguyên JDBC Trong Khối `finally`:**

   ```java
   close(myConn, myStmt, null);
   ```

   - Đảm bảo rằng các tài nguyên JDBC được đóng lại sau khi sử dụng, ngay cả khi có ngoại lệ xảy ra.
   - Truyền `null` cho `ResultSet` vì trong trường hợp này chúng ta không sử dụng `ResultSet`.

#### **3. Cập Nhật Phương Thức `close` Nếu Cần Thiết**

Đảm bảo rằng phương thức `close` được định nghĩa trong `StudentDbUtil` có khả năng xử lý việc đóng `Connection`, `Statement`, và `ResultSet`. Ví dụ:

```java
private void close(Connection myConn, Statement myStmt, ResultSet myRs) {
    try {
        if (myRs != null) {
            myRs.close();
        }
        if (myStmt != null) {
            myStmt.close();
        }
        if (myConn != null) {
            myConn.close(); // Trả kết nối về pool
        }
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}
```

#### **4. Kiểm Tra Tính Năng Thêm Sinh Viên**

1. **Khởi Động Lại Ứng Dụng:**
   - Triển khai lại ứng dụng hoặc khởi động lại máy chủ để đảm bảo các thay đổi được áp dụng.

2. **Thêm Sinh Viên Mới Từ Giao Diện Web:**
   - Truy cập trang `list-students.jsp`.
   - Nhấn nút **Add Student** để mở form thêm sinh viên.
   - Nhập thông tin sinh viên mới và nhấn **Save**.

3. **Kiểm Tra Kết Quả:**
   - Sau khi nhấn **Save**, bạn sẽ được chuyển về trang danh sách sinh viên.
   - Kiểm tra xem sinh viên mới đã xuất hiện trong danh sách chưa.
   - **Xác nhận trong cơ sở dữ liệu:**
     - Kết nối đến cơ sở dữ liệu (ví dụ: bằng công cụ quản lý cơ sở dữ liệu như MySQL Workbench).
     - Thực hiện câu lệnh `SELECT * FROM student` để kiểm tra xem bản ghi mới đã được thêm vào chưa.

#### **5. Ví Dụ Minh Họa**

**Giả Sử Bạn Thêm Sinh Viên:**

- **First Name:** Andrew
- **Last Name:** Whittaker
- **Email:** andy@luv2code.com

**Sau Khi Thêm, Bạn Sẽ Thấy:**

- Sinh viên Andrew Whittaker xuất hiện trong danh sách trên trang web.
- Trong cơ sở dữ liệu, một bản ghi mới với thông tin trên được thêm vào bảng `student`.

#### **6. Kết Luận**

- Bạn đã hoàn thành việc triển khai tính năng **Add Student** cho ứng dụng quản lý sinh viên.
- Từ giao diện web, bạn có thể thêm sinh viên mới, và thông tin này sẽ được lưu trữ trong cơ sở dữ liệu.
- Tính năng này hoàn thiện quy trình thêm dữ liệu, từ giao diện người dùng đến tầng cơ sở dữ liệu.

### Tổng Kết Toàn Bộ Quy Trình

- **Bước 1:** Thêm nút **Add Student** vào trang `list-students.jsp`.
- **Bước 2:** Tạo form `add-student-form.jsp` để nhập thông tin sinh viên mới.
- **Bước 3:** Cập nhật `StudentControllerServlet` để xử lý yêu cầu thêm sinh viên.
- **Bước 4:** Cập nhật `StudentDbUtil` với mã JDBC để chèn dữ liệu vào cơ sở dữ liệu.

### Chúc Mừng!

- Bạn đã hoàn thành việc xây dựng tính năng thêm sinh viên cho ứng dụng.
- Ứng dụng của bạn bây giờ đã có khả năng **Create** trong mô hình **CRUD**.
- Trong các bước tiếp theo, bạn có thể tiếp tục mở rộng ứng dụng bằng cách thêm các tính năng **Update** và **Delete**.

---

Nếu bạn có bất kỳ câu hỏi nào hoặc cần giải thích thêm về bất kỳ phần nào, đừng ngần ngại hỏi!
