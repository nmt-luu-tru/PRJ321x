### Cập Nhật `StudentControllerServlet` Để Xử Lý Yêu Cầu Cập Nhật Sinh Viên

Trong bước này, chúng ta sẽ cập nhật `StudentControllerServlet` để xử lý yêu cầu cập nhật thông tin sinh viên khi người dùng gửi form từ `update-student-form.jsp`. Mục tiêu là khi người dùng nhấn nút **Save** sau khi chỉnh sửa thông tin sinh viên, servlet sẽ nhận dữ liệu, cập nhật cơ sở dữ liệu và chuyển hướng người dùng trở lại trang danh sách sinh viên.

**Lưu ý:** Vì form của chúng ta vẫn sử dụng phương thức `GET` như trong các phần trước, chúng ta sẽ xử lý yêu cầu trong phương thức `doGet` của servlet.

#### **1. Mục Tiêu**

- **Xử lý lệnh `UPDATE` trong servlet**: Thêm logic để đọc tham số `command` và xử lý khi `command="UPDATE"` trong phương thức `doGet`.
- **Đọc dữ liệu từ form**: Lấy thông tin sinh viên từ các trường nhập liệu.
- **Cập nhật sinh viên trong cơ sở dữ liệu**: Sử dụng `StudentDbUtil` để thực hiện cập nhật thông tin sinh viên.
- **Chuyển hướng người dùng về trang danh sách sinh viên**: Hiển thị danh sách sinh viên với thông tin đã được cập nhật.

#### **2. Cập Nhật `StudentControllerServlet`**

**Bước 1: Thêm Xử Lý Cho Lệnh `UPDATE` Trong Phương Thức `doGet`**

**Cập nhật phương thức `doGet` như sau:**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    try {
        // Đọc tham số command
        String theCommand = request.getParameter("command");

        // Nếu command không tồn tại, mặc định là "LIST"
        if (theCommand == null) {
            theCommand = "LIST";
        }

        // Chuyển hướng đến phương thức tương ứng
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

            // Các case khác...
            default:
                listStudents(request, response);
        }

    } catch (Exception exc) {
        throw new ServletException(exc);
    }
}
```

**Giải thích:**

- **Đọc tham số `command`**: Giống như trước, chúng ta đọc tham số `command` từ request.
- **Thêm case `"UPDATE"`**: Khi `command="UPDATE"`, chúng ta gọi phương thức `updateStudent`.

**Bước 2: Thêm Phương Thức `updateStudent` Để Xử Lý Việc Cập Nhật Sinh Viên**

**Thêm phương thức `updateStudent` vào servlet:**

```java
private void updateStudent(HttpServletRequest request, HttpServletResponse response) throws Exception {

    // Đọc dữ liệu từ form
    int id = Integer.parseInt(request.getParameter("studentId"));
    String firstName = request.getParameter("firstName");
    String lastName = request.getParameter("lastName");
    String email = request.getParameter("email");

    // Tạo đối tượng Student với thông tin mới
    Student theStudent = new Student(id, firstName, lastName, email);

    // Cập nhật sinh viên trong cơ sở dữ liệu
    studentDbUtil.updateStudent(theStudent);

    // Chuyển hướng về danh sách sinh viên
    listStudents(request, response);
}
```

**Giải thích:**

1. **Đọc dữ liệu từ form:**

   - **`studentId`**: Chuyển đổi từ `String` sang `int` bằng `Integer.parseInt`.
   - **`firstName`, `lastName`, `email`**: Đọc trực tiếp từ request thông qua `getParameter`.

2. **Tạo đối tượng `Student` mới:**

   - Sử dụng constructor có tham số `id`, `firstName`, `lastName`, `email` để tạo đối tượng `Student` mới với thông tin đã cập nhật.

3. **Cập nhật sinh viên trong cơ sở dữ liệu:**

   - Gọi phương thức `updateStudent` của `StudentDbUtil` để cập nhật thông tin sinh viên trong cơ sở dữ liệu.

4. **Chuyển hướng về danh sách sinh viên:**

   - Sau khi cập nhật thành công, gọi lại phương thức `listStudents` để hiển thị danh sách sinh viên mới nhất.

**Bước 3: Xử Lý Ngoại Lệ**

- Thêm `throws Exception` vào khai báo phương thức để xử lý các ngoại lệ có thể xảy ra trong quá trình cập nhật.
