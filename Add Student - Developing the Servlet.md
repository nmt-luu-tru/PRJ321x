

Trong bước này, chúng ta sẽ tập trung cập nhật Servlet của mình là `StudentControllerServlet` để xử lý yêu cầu thêm sinh viên từ form mà chúng ta vừa tạo ra ở bước trước. Dưới đây là giải thích chi tiết từng phần mã nguồn mà bạn đã đề cập đến:

#### **1. Mục Đích Của Việc Cập Nhật `StudentControllerServlet`**
`StudentControllerServlet` chịu trách nhiệm xử lý các yêu cầu đến từ người dùng (client). Khi người dùng gửi thông tin từ form `add-student-form.jsp`, Servlet sẽ:
- Nhận thông tin từ form.
- Thêm thông tin sinh viên vào cơ sở dữ liệu.
- Chuyển hướng lại người dùng về trang danh sách sinh viên (`list-students.jsp`).

#### **2. Xử Lý Yêu Cầu Trong `doGet` Của `StudentControllerServlet`**
Trong phương thức `doGet`, ta sẽ thực hiện những việc sau:

- **Đọc tham số `command` từ request**:
  ```java
  String theCommand = request.getParameter("command");
  ```
  - `theCommand` là một tham số mà form sẽ truyền lên, cho biết người dùng đang yêu cầu thực hiện hành động gì (ví dụ: `list` để hiển thị danh sách, `add` để thêm sinh viên mới, ...).

- **Thiết lập giá trị mặc định nếu `command` không tồn tại**:
  ```java
  if (theCommand == null) {
      theCommand = "LIST";
  }
  ```
  - Trong trường hợp không có tham số `command` được gửi lên (ví dụ: người dùng truy cập vào Servlet mà không thông qua bất kỳ hành động cụ thể nào), ta sẽ thiết lập mặc định cho `command` là `"LIST"`.

- **Điều hướng xử lý yêu cầu dựa trên `command`**:
  ```java
  switch (theCommand) {
      case "LIST":
          listStudents(request, response);
          break;
      case "ADD":
          addStudent(request, response);
          break;
      default:
          listStudents(request, response);
  }
  ```
  - **`LIST`**: Gọi phương thức `listStudents` để hiển thị danh sách sinh viên.
  - **`ADD`**: Gọi phương thức `addStudent` để thêm một sinh viên mới.
  - **Mặc định**: Nếu `command` không khớp với bất kỳ giá trị nào đã định trước, ta sẽ hiển thị danh sách sinh viên như một hành động mặc định.

#### **3. Cập Nhật Phương Thức `addStudent` Để Xử Lý Việc Thêm Sinh Viên**
Phương thức `addStudent` sẽ chịu trách nhiệm:
1. **Đọc dữ liệu từ form**:
   - Ta sẽ sử dụng phương thức `request.getParameter` để lấy dữ liệu từ các trường của form:
   ```java
   String firstName = request.getParameter("firstName");
   String lastName = request.getParameter("lastName");
   String email = request.getParameter("email");
   ```

2. **Tạo đối tượng `Student`**:
   - Sử dụng dữ liệu đã lấy từ form để tạo đối tượng `Student` mới:
   ```java
   Student theStudent = new Student(firstName, lastName, email);
   ```

3. **Gọi `studentDbUtil.addStudent` để thêm sinh viên vào cơ sở dữ liệu**:
   - Sử dụng đối tượng `studentDbUtil` để thêm sinh viên mới:
   ```java
   studentDbUtil.addStudent(theStudent);
   ```
   - Đây là phương thức sẽ thực hiện việc thêm sinh viên vào cơ sở dữ liệu thông qua các thao tác JDBC. Tuy nhiên, phương thức này hiện chưa được triển khai đầy đủ.

4. **Chuyển hướng về trang danh sách sinh viên**:
   - Sau khi thêm sinh viên thành công, ta sẽ gọi lại `listStudents` để hiển thị danh sách sinh viên đã được cập nhật:
   ```java
   listStudents(request, response);
   ```

#### **4. Phương Thức `addStudent` Trong `StudentDbUtil`**
- **Tạo Phương Thức `addStudent` Trong `StudentDbUtil`**:
   - Eclipse sẽ tự động tạo một phương thức trống `addStudent` trong lớp `StudentDbUtil` khi chúng ta gọi phương thức `addStudent` từ `StudentControllerServlet`. Điều này giúp ta tránh lỗi biên dịch khi gọi một phương thức chưa tồn tại.
   - Phương thức `addStudent` sau này sẽ được triển khai để thực hiện việc kết nối đến cơ sở dữ liệu và thêm sinh viên mới thông qua JDBC.

#### **5. Kiểm Tra Và Chạy Ứng Dụng**
- Khi nhấn vào nút **Save** trên form `add-student-form.jsp`, bạn sẽ thấy không có sinh viên mới được thêm vào danh sách, vì hiện tại phương thức `addStudent` chưa thực hiện việc thêm dữ liệu vào cơ sở dữ liệu.
- Đây là kết quả mong đợi ở giai đoạn này, vì việc xử lý thêm sinh viên mới chỉ được triển khai trong phần tiếp theo, khi ta cập nhật lại phương thức `addStudent` trong lớp `StudentDbUtil`.

#### **Kết Luận**
Việc cập nhật `StudentControllerServlet` trong bước này giúp chúng ta tạo ra luồng xử lý cơ bản cho yêu cầu thêm sinh viên, từ việc đọc dữ liệu từ form, tạo đối tượng sinh viên, thêm vào cơ sở dữ liệu (ở bước sau), và cuối cùng hiển thị danh sách sinh viên đã được cập nhật. Việc phân tách mã nguồn thành các phương thức và lớp khác nhau giúp chúng ta dễ dàng mở rộng và bảo trì ứng dụng sau này.
