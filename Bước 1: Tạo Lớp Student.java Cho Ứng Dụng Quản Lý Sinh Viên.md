### Bước 1: Tạo Lớp Student.java Cho Ứng Dụng Quản Lý Sinh Viên

Trong video này, chúng ta sẽ tạo lớp `Student.java` để làm đại diện cho mỗi đối tượng sinh viên trong ứng dụng quản lý sinh viên. Lớp này sẽ giữ các thông tin cơ bản của sinh viên như mã số, họ tên và địa chỉ email. Lớp `Student` sẽ là một **POJO (Plain Old Java Object)**, giúp chúng ta dễ dàng thao tác với dữ liệu của sinh viên mà không cần phụ thuộc vào các logic phức tạp khác.

Dưới đây là chi tiết các bước thực hiện và giải thích cách xây dựng lớp `Student`:

#### 1. Tạo Lớp Student.java Trong Dự Án
1. Mở **Eclipse** hoặc **IDE** mà bạn đang sử dụng, và tạo mới một lớp Java cho đối tượng sinh viên.
2. Đặt tên lớp là `Student`.
3. Định nghĩa các thuộc tính của sinh viên:
   - **`id`**: Mã số sinh viên, sử dụng kiểu `int`.
   - **`firstName`**: Tên sinh viên, sử dụng kiểu `String`.
   - **`lastName`**: Họ sinh viên, sử dụng kiểu `String`.
   - **`email`**: Địa chỉ email của sinh viên, sử dụng kiểu `String`.

#### 2. Cấu Trúc Mã Nguồn Cho Lớp Student
Dưới đây là cấu trúc mã nguồn đầy đủ cho lớp `Student.java`:

```java
package com.luv2code.studentapp.model;

public class Student {
    private int id;             // Mã số sinh viên
    private String firstName;   // Tên sinh viên
    private String lastName;    // Họ sinh viên
    private String email;       // Địa chỉ email của sinh viên

    // Constructor đầy đủ (bao gồm id)
    public Student(int id, String firstName, String lastName, String email) {
        this.id = id;
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    // Constructor không bao gồm id
    public Student(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    // Getter và Setter cho các thuộc tính
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    // Phương thức toString() để hiển thị thông tin sinh viên dưới dạng chuỗi
    @Override
    public String toString() {
        return "Student [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email + "]";
    }
}
```

#### 3. Chi Tiết Các Thành Phần Trong Lớp Student
- **Thuộc tính**: Lớp `Student` bao gồm bốn thuộc tính là `id`, `firstName`, `lastName` và `email`. Đây là những thông tin cơ bản mà chúng ta muốn lưu trữ cho mỗi sinh viên.
- **Constructor**: Lớp có hai constructor:
  - Constructor đầu tiên nhận vào toàn bộ các giá trị cho `id`, `firstName`, `lastName` và `email`. Constructor này sẽ được sử dụng khi bạn đã có mã số sinh viên (ví dụ, khi lấy dữ liệu từ cơ sở dữ liệu).
  - Constructor thứ hai chỉ nhận `firstName`, `lastName` và `email` mà không cần `id`. Điều này rất hữu ích khi bạn tạo một sinh viên mới và `id` sẽ tự động được tạo ra từ cơ sở dữ liệu.
- **Getter và Setter**: Các phương thức getter và setter giúp truy cập và chỉnh sửa các thuộc tính của lớp `Student`.
- **Phương thức `toString()`**: Phương thức này sẽ trả về thông tin của đối tượng sinh viên dưới dạng chuỗi, giúp dễ dàng hơn trong việc in ra màn hình hoặc ghi log để kiểm tra.

#### 4. Giải Thích Cách Sử Dụng Lớp Student
- **Khi khởi tạo sinh viên**: Bạn có thể tạo một đối tượng `Student` như sau:
  ```java
  Student newStudent = new Student("John", "Doe", "john.doe@example.com");
  ```
- **Sử dụng getter và setter**: Bạn có thể sử dụng các phương thức getter và setter để truy cập hoặc cập nhật thông tin của đối tượng `Student`:
  ```java
  String firstName = newStudent.getFirstName();
  newStudent.setLastName("Smith");
  ```
- **Kiểm tra đối tượng bằng `toString()`**:
  ```java
  System.out.println(newStudent.toString());
  ```

#### 5. Kết Luận
Lớp `Student.java` đã được xây dựng hoàn chỉnh để làm đại diện cho mỗi đối tượng sinh viên trong ứng dụng quản lý sinh viên. Đây là một bước khởi đầu quan trọng cho ứng dụng, giúp chúng ta dễ dàng quản lý và xử lý thông tin sinh viên trong các phần tiếp theo. Tiếp tục video sau, chúng ta sẽ xây dựng lớp `StudentDAO` để tương tác với cơ sở dữ liệu, lấy dữ liệu sinh viên và thực hiện các thao tác CRUD. 

Hãy cùng chuẩn bị và tiếp tục xây dựng ứng dụng quản lý sinh viên hoàn chỉnh này nhé!
