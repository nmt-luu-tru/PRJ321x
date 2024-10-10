
#### **1. Cập Nhật `StudentControllerServlet` Để Xử Lý Yêu Cầu "LOAD"**

**Mục tiêu:**
- Đọc tham số `command` từ request và thêm case `"LOAD"` trong switch-case để xử lý yêu cầu tải thông tin sinh viên.
- Đọc `studentId` từ tham số request.
- Gọi phương thức `getStudent` trong `StudentDbUtil` để lấy thông tin sinh viên từ cơ sở dữ liệu.
- Đặt đối tượng `Student` vào `request` để chuyển sang trang JSP.
- Chuyển hướng đến trang `update-student-form.jsp` để hiển thị form cập nhật.

**Thực hiện:**

1. **Mở file `StudentControllerServlet.java`**:
   - Đi tới phương thức `doGet`.

2. **Thêm case `"LOAD"` trong switch-case của `doGet`**:

   ```java
   switch (theCommand) {
       case "LOAD":
           loadStudent(request, response);
           break;
       // Các case khác...
       default:
           listStudents(request, response);
   }
   ```

3. **Tạo phương thức `loadStudent`**:

   ```java
   private void loadStudent(HttpServletRequest request, HttpServletResponse response) throws Exception {
       // Đọc studentId từ request
       String theStudentId = request.getParameter("studentId");

       // Lấy thông tin sinh viên từ cơ sở dữ liệu (sử dụng StudentDbUtil)
       Student theStudent = studentDbUtil.getStudent(theStudentId);

       // Đặt đối tượng Student vào request attribute
       request.setAttribute("THE_STUDENT", theStudent);

       // Chuyển tiếp đến trang update-student-form.jsp
       RequestDispatcher dispatcher = request.getRequestDispatcher("/update-student-form.jsp");
       dispatcher.forward(request, response);
   }
   ```

**Giải thích:**

- **`getParameter("studentId")`**: Lấy ID của sinh viên từ tham số request (được truyền qua URL khi người dùng nhấp vào liên kết "Update").
- **`studentDbUtil.getStudent(theStudentId)`**: Gọi phương thức trong `StudentDbUtil` để lấy thông tin sinh viên từ cơ sở dữ liệu.
- **`request.setAttribute("THE_STUDENT", theStudent)`**: Đặt đối tượng sinh viên vào request để có thể sử dụng trong JSP.
- **Chuyển tiếp đến `update-student-form.jsp`**: Sử dụng `RequestDispatcher` để chuyển tiếp yêu cầu và dữ liệu đến trang JSP.

#### **2. Cập Nhật `StudentDbUtil` Với Phương Thức `getStudent`**

**Mục tiêu:**
- Viết phương thức `getStudent` để lấy thông tin sinh viên từ cơ sở dữ liệu dựa trên ID.

**Thực hiện:**

1. **Mở file `StudentDbUtil.java`**.

2. **Thêm phương thức `getStudent`**:

   ```java
   public Student getStudent(String theStudentId) throws Exception {
       Student theStudent = null;
       Connection myConn = null;
       PreparedStatement myStmt = null;
       ResultSet myRs = null;

       try {
           // Chuyển đổi theStudentId thành int
           int studentId = Integer.parseInt(theStudentId);

           // Lấy kết nối cơ sở dữ liệu
           myConn = dataSource.getConnection();

           // Tạo câu lệnh SQL để lấy thông tin sinh viên
           String sql = "SELECT * FROM student WHERE id=?";

           // Chuẩn bị câu lệnh
           myStmt = myConn.prepareStatement(sql);

           // Thiết lập tham số cho câu lệnh SQL
           myStmt.setInt(1, studentId);

           // Thực thi câu lệnh SQL
           myRs = myStmt.executeQuery();

           // Xử lý ResultSet
           if (myRs.next()) {
               String firstName = myRs.getString("first_name");
               String lastName = myRs.getString("last_name");
               String email = myRs.getString("email");

               // Tạo đối tượng Student
               theStudent = new Student(studentId, firstName, lastName, email);
           } else {
               throw new Exception("Could not find student id: " + studentId);
           }

           return theStudent;
       } finally {
           // Đóng kết nối và các tài nguyên JDBC
           close(myConn, myStmt, myRs);
       }
   }
   ```

**Giải thích:**

- **Chuyển đổi `theStudentId` thành `int`**: Vì ID trong cơ sở dữ liệu là số nguyên.
- **Kết nối cơ sở dữ liệu**: Sử dụng `dataSource` để lấy kết nối.
- **Chuẩn bị câu lệnh SQL**: Sử dụng `PreparedStatement` để tránh SQL Injection và dễ dàng thiết lập tham số.
- **Thiết lập tham số**: `myStmt.setInt(1, studentId);` thiết lập giá trị cho dấu `?` trong câu lệnh SQL.
- **Thực thi và xử lý kết quả**: Sử dụng `executeQuery()` và `ResultSet` để lấy dữ liệu.
- **Tạo đối tượng `Student`**: Nếu tìm thấy sinh viên, tạo đối tượng `Student` với thông tin lấy được.
- **Xử lý trường hợp không tìm thấy sinh viên**: Ném ra ngoại lệ nếu không tìm thấy sinh viên với ID tương ứng.

