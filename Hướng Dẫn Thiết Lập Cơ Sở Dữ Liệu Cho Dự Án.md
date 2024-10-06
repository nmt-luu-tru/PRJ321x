### Hướng Dẫn Thiết Lập Cơ Sở Dữ Liệu Cho Dự Án Theo Dõi Sinh Viên Bằng SQL

Trong video này, chúng ta sẽ học cách thiết lập bảng cơ sở dữ liệu cho dự án quản lý sinh viên bằng cách sử dụng các **script SQL**. Chúng ta sẽ chạy hai script chính:

1. **Script tạo người dùng (`01-create-user.sql`)**: Tạo một tài khoản người dùng mới với quyền quản trị cơ sở dữ liệu.
2. **Script tạo cơ sở dữ liệu và bảng sinh viên (`02-student-tracker.sql`)**: Tạo cơ sở dữ liệu và bảng `student` để lưu trữ thông tin sinh viên cùng với một số dữ liệu mẫu.

Trong hướng dẫn này, tôi sẽ mô tả chi tiết từng script và cách thực thi chúng trong **MySQL Workbench**.

---

### Bước 1: Tạo Người Dùng Mới Cho Cơ Sở Dữ Liệu

#### Mục đích:
- Tạo một người dùng mới có quyền truy cập cơ sở dữ liệu để chúng ta có thể bảo mật tài nguyên cơ sở dữ liệu tốt hơn, thay vì sử dụng tài khoản `root` mặc định.

#### Cấu Trúc Script (`01-create-user.sql`):
```sql
-- Tạo người dùng 'webstudent' với mật khẩu là 'webstudent'
CREATE USER 'webstudent'@'localhost' IDENTIFIED BY 'webstudent';

-- Cấp quyền truy cập toàn bộ cơ sở dữ liệu cho người dùng 'webstudent'
GRANT ALL PRIVILEGES ON *.* TO 'webstudent'@'localhost';
```
- Lệnh **`CREATE USER`** tạo một tài khoản mới với tên đăng nhập là `webstudent` và mật khẩu là `webstudent`.
- Lệnh **`GRANT ALL PRIVILEGES`** cho phép tài khoản này có đầy đủ quyền truy cập và quản lý các bảng trong cơ sở dữ liệu.

#### Thực Thi Script Trong MySQL Workbench:
1. **Mở MySQL Workbench** và đăng nhập vào với tài khoản `root`.
2. Từ thanh công cụ, chọn **File > Open SQL Script** và chọn tệp **`01-create-user.sql`**.
3. Nhấn vào nút **Execute (Lightning Bolt)** để thực thi lệnh SQL.
4. Kiểm tra kết quả ở phần dưới của giao diện, nếu có dấu tích xanh, tức là lệnh đã thực thi thành công.

---

### Bước 2: Tạo Cơ Sở Dữ Liệu Và Bảng Sinh Viên

#### Mục đích:
- Tạo cơ sở dữ liệu mới có tên là `web_student_tracker` và bảng `student` để lưu trữ thông tin sinh viên như mã sinh viên, họ tên và email.

#### Cấu Trúc Script (`02-student-tracker.sql`):
```sql
-- Tạo cơ sở dữ liệu 'web_student_tracker'
CREATE DATABASE IF NOT EXISTS web_student_tracker;

-- Sử dụng cơ sở dữ liệu 'web_student_tracker'
USE web_student_tracker;

-- Tạo bảng 'student' với các cột id, first_name, last_name và email
CREATE TABLE IF NOT EXISTS student (
    id INT NOT NULL AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    PRIMARY KEY (id)
);

-- Thêm dữ liệu mẫu vào bảng 'student'
INSERT INTO student (first_name, last_name, email)
VALUES
    ('Mary', 'Public', 'mary@luv2code.com'),
    ('John', 'Doe', 'john@luv2code.com'),
    ('Ajay', 'Rao', 'ajay@luv2code.com'),
    ('Bill', 'Appleby', 'bill@luv2code.com'),
    ('Maxwell', 'Johnson', 'maxwell@luv2code.com');
```
- **`CREATE DATABASE IF NOT EXISTS web_student_tracker`**: Tạo cơ sở dữ liệu mới nếu chưa tồn tại.
- **`USE web_student_tracker`**: Chuyển sang sử dụng cơ sở dữ liệu `web_student_tracker`.
- **`CREATE TABLE student`**: Tạo bảng `student` với các cột `id`, `first_name`, `last_name` và `email`. Mỗi sinh viên sẽ có một `id` duy nhất (khóa chính).
- **`INSERT INTO student`**: Thêm năm dòng dữ liệu mẫu vào bảng `student`.

#### Thực Thi Script Trong MySQL Workbench:
1. Từ thanh công cụ, chọn **File > Open SQL Script** và chọn tệp **`02-student-tracker.sql`**.
2. Nhấn vào nút **Execute** để thực thi lệnh SQL.
3. Kiểm tra phần **SCHEMAS** bên trái của MySQL Workbench:
   - Nhấp chuột phải vào **SCHEMAS** và chọn **Refresh All**.
   - Bảng `student` sẽ xuất hiện trong cơ sở dữ liệu `web_student_tracker`.
4. Nhấp chuột phải vào bảng `student` và chọn **Select Rows** để xem dữ liệu mẫu đã được thêm vào thành công hay chưa.

---

### Bước 3: Kiểm Tra Và Xem Dữ Liệu Mẫu

Sau khi đã thực thi các script SQL, bạn có thể kiểm tra lại dữ liệu như sau:

1. **Mở bảng `student`** trong cơ sở dữ liệu `web_student_tracker`.
2. Nhấp chuột phải vào bảng `student` và chọn **Select Rows**.
3. Kiểm tra danh sách sinh viên mẫu (5 sinh viên) đã được thêm vào bảng:
   - Mary Public
   - John Doe
   - Ajay Rao
   - Bill Appleby
   - Maxwell Johnson

Nếu bạn thấy danh sách sinh viên xuất hiện đầy đủ như trên, thì việc tạo cơ sở dữ liệu và bảng đã thành công.

---

### Kết Luận

Việc thiết lập cơ sở dữ liệu cho dự án quản lý sinh viên với hai script SQL cơ bản giúp bạn tạo ra một môi trường dữ liệu sẵn sàng để sử dụng trong các ứng dụng web. Sau khi hoàn tất bước này, chúng ta đã có một bảng `student` chứa dữ liệu mẫu để sử dụng cho các thao tác CRUD (Create, Read, Update, Delete) trong các video tiếp theo. Hãy đảm bảo rằng cơ sở dữ liệu đã được tạo đúng cách trước khi tiếp tục phát triển ứng dụng.

Trong những video tiếp theo, chúng ta sẽ tiếp tục xây dựng chức năng thêm, sửa, xóa và cập nhật sinh viên bằng cách kết nối ứng dụng web với cơ sở dữ liệu này. Chúc bạn thực hành thành công và có trải nghiệm thú vị! 😄
