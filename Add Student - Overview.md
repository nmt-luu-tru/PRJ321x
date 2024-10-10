### Thêm Tính Năng “Add Student” Vào Ứng Dụng Quản Lý Sinh Viên

Trong video này, chúng ta sẽ học cách thêm một sinh viên mới vào cơ sở dữ liệu bằng cách xây dựng một form nhập liệu và xử lý dữ liệu đó thông qua Servlet. Đây là một tính năng quan trọng giúp chúng ta thực hiện thao tác **Create** trong mô hình CRUD (Create, Read, Update, Delete).

#### **Bước 1: Demo Tính Năng Thêm Sinh Viên**
- Tính năng này sẽ cho phép người dùng click vào nút **Add Student** trên trang chính (`list_students.jsp`) để mở form nhập liệu.
- Khi người dùng nhập tên, họ và email của sinh viên vào form và nhấn **Save**, thông tin sẽ được thêm vào cơ sở dữ liệu.
- Sau khi thêm, trang sẽ quay lại danh sách sinh viên và hiển thị thông tin sinh viên mới.

#### **Bước 2: Phân Tích Quy Trình Hoạt Động**
1. **Giao Diện Danh Sách Sinh Viên (`list_students.jsp`)**:
   - Thêm một nút **Add Student** vào trang.
2. **Form Thêm Sinh Viên**:
   - Tạo một trang mới (`add_student_form.jsp`) để hiển thị form nhập liệu cho sinh viên.
3. **Servlet Xử Lý Dữ Liệu**:
   - Khi người dùng nhấn nút **Save** trên form, dữ liệu sẽ được gửi đến `StudentControllerServlet`.
   - Servlet sẽ đọc dữ liệu từ form, và sau đó gọi `StudentDbUtil` để thêm sinh viên vào cơ sở dữ liệu.
4. **Thêm Dữ Liệu Vào Cơ Sở Dữ Liệu**:
   - `StudentDbUtil` sẽ thực hiện lệnh SQL để chèn dữ liệu mới vào bảng `students`.

#### **Bước 3: Sơ Đồ Chuỗi (Sequence Diagram)**
Dưới đây là sơ đồ chuỗi thể hiện luồng dữ liệu của ứng dụng khi thực hiện thao tác thêm sinh viên:

1. Người dùng truy cập vào `list_students.jsp`.
2. Nhấn nút **Add Student** để chuyển đến `add_student_form.jsp`.
3. Nhập thông tin sinh viên vào form và nhấn **Save**.
4. Dữ liệu được gửi đến `StudentControllerServlet`.
5. `StudentControllerServlet` xử lý và chuyển dữ liệu đến `StudentDbUtil`.
6. `StudentDbUtil` chèn dữ liệu vào cơ sở dữ liệu.
7. `StudentControllerServlet` chuyển hướng người dùng trở lại `list_students.jsp` và hiển thị danh sách sinh viên đã được cập nhật.
