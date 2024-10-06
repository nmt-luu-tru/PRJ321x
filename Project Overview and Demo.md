### Hướng Dẫn Tạo Ứng Dụng Quản Lý Sinh Viên Sử Dụng Servlet, JSP và MySQL

Trong bài hướng dẫn này, chúng ta sẽ cùng nhau xây dựng một ứng dụng web quản lý sinh viên hoàn chỉnh với các tính năng CRUD (Create, Read, Update, Delete) sử dụng **Servlets**, **JSP** và **MySQL**. Đây là một dự án thực tế giúp bạn hiểu rõ hơn về cách tương tác với cơ sở dữ liệu từ ứng dụng Java Web và quản lý dữ liệu sinh viên.

### 1. **Mô Tả Ứng Dụng**

Ứng dụng mà chúng ta sẽ xây dựng là một hệ thống quản lý sinh viên cho một trường đại học hư cấu mang tên **FooBar University**. Ứng dụng này sẽ có các tính năng chính sau:

- **Hiển thị danh sách sinh viên**: Cho phép người dùng xem tất cả sinh viên đang có trong cơ sở dữ liệu.
- **Thêm sinh viên mới**: Người dùng có thể thêm sinh viên mới với thông tin như tên, họ, và email.
- **Cập nhật thông tin sinh viên**: Người dùng có thể chỉnh sửa thông tin sinh viên, ví dụ như cập nhật email hoặc tên.
- **Xóa sinh viên**: Cho phép người dùng xóa sinh viên khỏi danh sách.

### 2. **Thiết Kế Cơ Sở Dữ Liệu**

Đầu tiên, chúng ta cần tạo một cơ sở dữ liệu trong MySQL. Cơ sở dữ liệu này sẽ có một bảng tên là `student` với các cột như sau:

| **ID**     | **First Name** | **Last Name** | **Email**           |
|------------|----------------|---------------|---------------------|
| 1          | John           | Doe           | john@example.com    |
| 2          | Jane           | Smith         | jane@example.com    |
| 3          | Peter          | Parker        | peter@example.com   |

**Câu lệnh tạo bảng trong MySQL:**

```sql
CREATE DATABASE student_db;

USE student_db;

CREATE TABLE student (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100)
);

INSERT INTO student (first_name, last_name, email) VALUES ('John', 'Doe', 'john@example.com');
INSERT INTO student (first_name, last_name, email) VALUES ('Jane', 'Smith', 'jane@example.com');
INSERT INTO student (first_name, last_name, email) VALUES ('Peter', 'Parker', 'peter@example.com');
```

### 3. **Cấu Trúc Dự Án**

Dự án sẽ được tổ chức như sau:

- **Model (`Student.java`)**: Đại diện cho đối tượng sinh viên.
- **DAO (Data Access Object) (`StudentDAO.java`)**: Chịu trách nhiệm tương tác với cơ sở dữ liệu.
- **Servlet (`StudentControllerServlet.java`)**: Điều hướng yêu cầu từ người dùng và gọi các phương thức tương ứng từ `StudentDAO`.
- **JSP**:
  - `student-list.jsp`: Hiển thị danh sách sinh viên.
  - `student-form.jsp`: Form để thêm hoặc chỉnh sửa thông tin sinh viên.
  - `index.jsp`: Trang chủ với liên kết đến danh sách sinh viên.
