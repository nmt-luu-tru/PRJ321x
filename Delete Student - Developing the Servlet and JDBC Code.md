
### **Cập Nhật `StudentControllerServlet` Để Xử Lý Yêu Cầu Xóa**

Trong servlet `StudentControllerServlet`, chúng ta sẽ thêm logic để xử lý yêu cầu `command=DELETE` được gửi từ liên kết "Delete" trên trang `list-students.jsp`.

#### **Cập nhật phương thức `doGet` trong `StudentControllerServlet`:**

1. Mở file `StudentControllerServlet.java`.
2. Di chuyển đến phần `doGet` để đọc tham số `command`.
3. Thêm trường hợp xử lý `case "DELETE"` để gọi phương thức `deleteStudent`.

**Mã lệnh:**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // Đọc tham số command từ request
    String theCommand = request.getParameter("command");

    // Nếu không có command, mặc định là list
    if (theCommand == null) {
        theCommand = "LIST";
    }

    // Chuyển hướng đến các phương thức xử lý tương ứng
    switch (theCommand) {
        case "LIST":
            listStudents(request, response);
            break;

        case "LOAD":
            loadStudent(request, response);
            break;

        case "UPDATE":
            updateStudent(request, response);
            break;

        case "DELETE": // Thêm case "DELETE"
            deleteStudent(request, response); 
            break;

        default:
            listStudents(request, response);
    }
}
```

#### **Thêm phương thức `deleteStudent` vào `StudentControllerServlet`**

**Mã lệnh:**

```java
private void deleteStudent(HttpServletRequest request, HttpServletResponse response) throws Exception {
    // Đọc student id từ form data
    String theStudentId = request.getParameter("studentId");

    // Xóa sinh viên khỏi cơ sở dữ liệu bằng cách gọi phương thức deleteStudent từ StudentDbUtil
    studentDbUtil.deleteStudent(theStudentId);

    // Chuyển hướng về trang danh sách sinh viên
    listStudents(request, response);
}
```

- `theStudentId`: Lấy `studentId` từ tham số request.
- `studentDbUtil.deleteStudent(theStudentId)`: Gọi phương thức xóa sinh viên từ `StudentDbUtil`.
- `listStudents(request, response)`: Sau khi xóa, chuyển hướng về trang danh sách sinh viên.

### **2. Bước 3: Cập Nhật `StudentDbUtil` Để Thực Hiện Xóa Sinh Viên**

Tiếp theo, chúng ta cần viết mã lệnh JDBC để thực hiện việc xóa sinh viên trong cơ sở dữ liệu.

#### **Thêm phương thức `deleteStudent` vào `StudentDbUtil`:**

1. Mở file `StudentDbUtil.java`.
2. Thêm phương thức `deleteStudent` với mã lệnh JDBC để xóa sinh viên theo `studentId`.

**Mã lệnh:**

```java
public void deleteStudent(String theStudentId) throws Exception {
    Connection myConn = null;
    PreparedStatement myStmt = null;

    try {
        // Chuyển đổi studentId từ String sang int
        int studentId = Integer.parseInt(theStudentId);

        // Lấy kết nối cơ sở dữ liệu
        myConn = dataSource.getConnection();

        // Tạo câu lệnh SQL để xóa sinh viên
        String sql = "DELETE FROM student WHERE id=?";

        // Tạo PreparedStatement
        myStmt = myConn.prepareStatement(sql);

        // Thiết lập giá trị tham số cho câu lệnh SQL
        myStmt.setInt(1, studentId);

        // Thực thi câu lệnh SQL
        myStmt.execute();
    } finally {
        // Đóng các đối tượng JDBC
        close(myConn, myStmt, null);
    }
}
```

- `int studentId = Integer.parseInt(theStudentId)`: Chuyển đổi `studentId` từ String sang kiểu số nguyên để sử dụng trong câu lệnh SQL.
- `myConn = dataSource.getConnection()`: Lấy kết nối cơ sở dữ liệu.
- `String sql = "DELETE FROM student WHERE id=?"`: Tạo câu lệnh SQL để xóa sinh viên với `id` tương ứng.
- `myStmt.setInt(1, studentId)`: Thiết lập giá trị cho tham số `?` trong câu lệnh SQL.
- `myStmt.execute()`: Thực thi câu lệnh SQL.

### **3. Kiểm Tra Chức Năng Xóa Sinh Viên**

Sau khi thêm mã lệnh vào servlet và `StudentDbUtil`, hãy kiểm tra tính năng "Delete" trên giao diện.

1. Khởi động ứng dụng và truy cập trang `list-students.jsp`.
2. Nhấp vào liên kết "Delete" cho một sinh viên cụ thể.
3. Hộp thoại xác nhận sẽ hiện ra.
4. Nhấp "OK" để xóa sinh viên.
5. Kiểm tra lại danh sách sinh viên để đảm bảo rằng sinh viên đã được xóa thành công.
6. Kiểm tra cơ sở dữ liệu để xác nhận rằng sinh viên đã bị xóa khỏi bảng `student`.

### **Tổng Kết**

Trong phần này, chúng ta đã hoàn thành việc tích hợp chức năng xóa sinh viên vào ứng dụng bằng cách:

1. Cập nhật `StudentControllerServlet` để xử lý yêu cầu `DELETE`.
2. Thêm mã lệnh JDBC trong `StudentDbUtil` để thực hiện việc xóa sinh viên khỏi cơ sở dữ liệu.
