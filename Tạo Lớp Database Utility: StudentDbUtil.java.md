### Tạo Lớp Database Utility: StudentDbUtil.java

Trong video này, chúng ta sẽ tiếp tục xây dựng lớp **Database Utility** với tên gọi `StudentDbUtil`. Lớp này sẽ thực hiện các chức năng tương tác với cơ sở dữ liệu như kết nối, truy vấn và quản lý dữ liệu của sinh viên. Lớp `StudentDbUtil` sẽ chứa các phương thức sử dụng JDBC để tương tác với cơ sở dữ liệu, và chúng ta sẽ gọi nó từ các Servlets để lấy danh sách sinh viên cũng như thực hiện các thao tác khác. 

Dưới đây là chi tiết từng bước thực hiện và mã nguồn cho lớp `StudentDbUtil`:

### 1. Tạo Lớp StudentDbUtil.java
Đầu tiên, bạn cần tạo một lớp mới có tên `StudentDbUtil.java` trong dự án của mình. Lớp này sẽ đóng vai trò là **Data Access Object (DAO)**, giúp thực hiện các thao tác CRUD (Create, Read, Update, Delete) với cơ sở dữ liệu.

**Mã nguồn của lớp `StudentDbUtil`:**

```java
package com.luv2code.studentapp.util;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

import javax.sql.DataSource;

import com.luv2code.studentapp.model.Student;

public class StudentDbUtil {

    // Tạo một thuộc tính để lưu trữ nguồn kết nối (DataSource)
    private DataSource dataSource;

    // Constructor nhận vào nguồn kết nối và gán cho thuộc tính của lớp
    public StudentDbUtil(DataSource theDataSource) {
        dataSource = theDataSource;
    }

    // Phương thức lấy danh sách sinh viên từ cơ sở dữ liệu
    public List<Student> getStudents() throws Exception {

        List<Student> students = new ArrayList<>(); // Tạo danh sách sinh viên để lưu kết quả

        Connection myConn = null;
        Statement myStmt = null;
        ResultSet myRs = null;

        try {
            // Bước 1: Lấy kết nối tới cơ sở dữ liệu
            myConn = dataSource.getConnection();

            // Bước 2: Tạo câu lệnh SQL để truy vấn danh sách sinh viên
            String sql = "SELECT * FROM student ORDER BY last_name";

            // Bước 3: Tạo đối tượng statement để thực hiện câu lệnh SQL
            myStmt = myConn.createStatement();

            // Bước 4: Thực thi câu lệnh SQL và lấy kết quả
            myRs = myStmt.executeQuery(sql);

            // Bước 5: Duyệt qua các kết quả và tạo đối tượng Student
            while (myRs.next()) {
                // Lấy dữ liệu từ từng dòng kết quả
                int id = myRs.getInt("id");
                String firstName = myRs.getString("first_name");
                String lastName = myRs.getString("last_name");
                String email = myRs.getString("email");

                // Tạo đối tượng Student từ dữ liệu lấy được
                Student tempStudent = new Student(id, firstName, lastName, email);

                // Thêm sinh viên vào danh sách
                students.add(tempStudent);
            }

            // Trả về danh sách sinh viên
            return students;
        }
        finally {
            // Đảm bảo đóng các đối tượng JDBC để giải phóng tài nguyên
            close(myConn, myStmt, myRs);
        }
    }

    // Phương thức đóng các đối tượng JDBC sau khi sử dụng
    private void close(Connection myConn, Statement myStmt, ResultSet myRs) {
        try {
            if (myRs != null) {
                myRs.close(); // Đóng ResultSet
            }

            if (myStmt != null) {
                myStmt.close(); // Đóng Statement
            }

            if (myConn != null) {
                myConn.close(); // Đóng Connection (trả lại vào Connection Pool)
            }
        }
        catch (SQLException exc) {
            exc.printStackTrace();
        }
    }
}
```

### 2. Chi Tiết Các Thành Phần Trong Lớp StudentDbUtil
Lớp `StudentDbUtil` sẽ đảm nhận vai trò giao tiếp với cơ sở dữ liệu, cụ thể là lấy dữ liệu từ bảng `student` trong cơ sở dữ liệu của chúng ta. Dưới đây là chi tiết từng thành phần trong lớp:

- **`DataSource dataSource`**: Đây là đối tượng `DataSource` dùng để quản lý kết nối đến cơ sở dữ liệu. Đối tượng này sẽ giúp quản lý Connection Pool, giúp cải thiện hiệu suất của ứng dụng.

- **Constructor `StudentDbUtil(DataSource theDataSource)`**: 
  - Constructor này nhận vào một `DataSource` và gán nó cho thuộc tính `dataSource` của lớp.
  - Mục đích là để mỗi lần cần kết nối tới cơ sở dữ liệu, chúng ta sẽ sử dụng `dataSource` để lấy ra một `Connection`.

- **Phương thức `getStudents()`**:
  - Phương thức này sẽ thực hiện tất cả các bước cần thiết để truy vấn danh sách sinh viên từ cơ sở dữ liệu.
  - Các bước bao gồm:
    - Lấy một `Connection` từ `dataSource`.
    - Tạo một câu lệnh SQL để truy vấn dữ liệu.
    - Thực thi câu lệnh và lấy kết quả vào `ResultSet`.
    - Duyệt qua `ResultSet` để tạo ra các đối tượng `Student` và thêm vào danh sách `students`.
  - Cuối cùng, phương thức sẽ trả về danh sách `students`.

- **Phương thức `close()`**:
  - Phương thức này sẽ đóng các đối tượng `Connection`, `Statement` và `ResultSet` sau khi chúng ta đã hoàn thành việc sử dụng chúng.
  - Điều này rất quan trọng để tránh rò rỉ tài nguyên (resource leaks) và làm tràn bộ nhớ trong ứng dụng.

### 3. Lưu Ý Khi Sử Dụng Lớp StudentDbUtil
- Lớp `StudentDbUtil` không chỉ phục vụ cho việc lấy danh sách sinh viên mà còn có thể mở rộng để thực hiện các chức năng khác như thêm mới, cập nhật hoặc xóa sinh viên từ cơ sở dữ liệu.
- Việc sử dụng `DataSource` giúp quản lý các kết nối tới cơ sở dữ liệu một cách hiệu quả hơn so với việc tạo mới `Connection` mỗi lần cần kết nối.

### 4. Kết Luận
Lớp `StudentDbUtil.java` đã được xây dựng xong và hoàn thiện với phương thức `getStudents()` để lấy danh sách sinh viên từ cơ sở dữ liệu. Lớp này là một phần quan trọng trong ứng dụng của chúng ta, giúp tách biệt các thao tác liên quan đến cơ sở dữ liệu ra khỏi `Servlet`, giữ cho mã nguồn rõ ràng và dễ bảo trì hơn.

Ở video tiếp theo, chúng ta sẽ xây dựng `Servlet` để gọi `StudentDbUtil` và hiển thị dữ liệu lên giao diện JSP. Tiếp tục theo dõi để hoàn thành ứng dụng quản lý sinh viên của chúng ta nhé! 😄
