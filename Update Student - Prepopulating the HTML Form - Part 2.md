### Tạo Form `update-student-form.jsp` Và Tiền Điền Dữ Liệu Sinh Viên

Trong bước này, chúng ta sẽ tạo trang `update-student-form.jsp` để hiển thị form cập nhật thông tin sinh viên. Mục tiêu là tiền điền (pre-populate) form với dữ liệu của sinh viên mà người dùng muốn cập nhật. Điều này giúp người dùng dễ dàng chỉnh sửa thông tin hiện có mà không cần nhập lại từ đầu.

#### **Bước 1: Tạo Trang `update-student-form.jsp` Bằng Cách Sao Chép `add-student-form.jsp`**

1. **Sao chép file `add-student-form.jsp`**:
   - Trong Eclipse (hoặc IDE bạn đang sử dụng), tìm đến file `add-student-form.jsp` trong dự án của bạn.
   - Nhấp chuột phải vào file này và chọn **Copy**.

2. **Dán và đổi tên thành `update-student-form.jsp`**:
   - Nhấp chuột phải vào thư mục chứa file gốc và chọn **Paste**.
   - Khi được yêu cầu đặt tên, nhập **`update-student-form.jsp`** và xác nhận.

#### **Bước 2: Chỉnh Sửa `update-student-form.jsp` Để Phù Hợp Với Chức Năng Cập Nhật**

1. **Thay đổi tiêu đề và tiêu đề trang**:
   - Mở file `update-student-form.jsp`.
   - Thay đổi nội dung trong thẻ `<title>` thành **`Update Student`**.
     ```html
     <title>Update Student</title>
     ```
   - Thay đổi tiêu đề trên trang bằng cách chỉnh sửa thẻ `<h3>`:
     ```html
     <h3>Update Student</h3>
     ```

2. **Thay đổi giá trị của trường ẩn `command` thành `"UPDATE"`**:
   - Tìm thẻ `<input>` có `name="command"`.
   - Thay đổi `value` từ `"ADD"` thành `"UPDATE"`:
     ```html
     <input type="hidden" name="command" value="UPDATE">
     ```

3. **Tiền điền dữ liệu vào các trường nhập liệu**:
   - Đối với mỗi trường nhập liệu (`First Name`, `Last Name`, `Email`), thêm thuộc tính `value` sử dụng Expression Language (EL) để hiển thị dữ liệu hiện có của sinh viên.
   - **Ví dụ cho trường `First Name`**:
     ```html
     <input type="text" name="firstName" value="${THE_STUDENT.firstName}" required>
     ```
   - Thực hiện tương tự cho `Last Name` và `Email`:
     ```html
     <input type="text" name="lastName" value="${THE_STUDENT.lastName}" required>
     <input type="email" name="email" value="${THE_STUDENT.email}" required>
     ```

4. **Thêm trường ẩn `studentId` để giữ ID của sinh viên**:
   - Chúng ta cần biết sinh viên nào đang được cập nhật, vì vậy cần truyền `studentId` về servlet khi form được submit.
   - Thêm trường ẩn sau thẻ `<input type="hidden" name="command" value="UPDATE">`:
     ```html
     <input type="hidden" name="studentId" value="${THE_STUDENT.id}">
     ```

   **Giải thích:**
   - **`${THE_STUDENT}`**: Đây là thuộc tính request mà chúng ta đã đặt trong servlet. Nó chứa đối tượng `Student` cần cập nhật.
   - **`${THE_STUDENT.id}`**, **`${THE_STUDENT.firstName}`**, **`${THE_STUDENT.lastName}`**, **`${THE_STUDENT.email}`**: Sử dụng EL để truy cập các thuộc tính của đối tượng `Student`.

#### **Bước 3: Hiểu Cách `THE_STUDENT` Được Đặt Trong Request Scope**

- Trong servlet `StudentControllerServlet`, khi xử lý lệnh `"LOAD"`, chúng ta đã đặt đối tượng `Student` vào request scope với tên `"THE_STUDENT"`:
  ```java
  // Trong phương thức loadStudent
  request.setAttribute("THE_STUDENT", theStudent);
  ```
- Điều này cho phép chúng ta truy cập dữ liệu của sinh viên trong trang JSP bằng cách sử dụng EL.

#### **Bước 4: Kiểm Tra Form Đã Tiền Điền Dữ Liệu**

1. **Chạy ứng dụng và truy cập trang danh sách sinh viên**:
   - Khởi động máy chủ (Tomcat, v.v.) và truy cập trang `list-students.jsp`.

2. **Nhấp vào liên kết "Update" cho một sinh viên bất kỳ**:
   - Ví dụ, chọn sinh viên **Maxwell Dixon** và nhấp vào "Update".

3. **Kiểm tra xem form có được tiền điền dữ liệu không**:
   - Trang `update-student-form.jsp` sẽ hiển thị với các trường nhập liệu đã được điền sẵn thông tin của **Maxwell Dixon**.

4. **Thử với sinh viên khác để đảm bảo tính nhất quán**:
   - Quay lại danh sách sinh viên.
   - Chọn sinh viên khác, ví dụ **Mary Public**, và nhấp vào "Update".
   - Kiểm tra xem form có hiển thị đúng thông tin của **Mary Public** không.

**Lưu ý:** Việc tiền điền dữ liệu giúp người dùng dễ dàng chỉnh sửa thông tin hiện có mà không cần nhập lại từ đầu. Đây là một phần quan trọng trong trải nghiệm người dùng.

#### **Bước 5: Xác Nhận Hoàn Thành Bước Này**

- Sau khi thực hiện các bước trên, bạn đã tạo thành công trang `update-student-form.jsp` và thiết lập để form được tiền điền dữ liệu dựa trên sinh viên được chọn.
- Form này hiện đã sẵn sàng để người dùng chỉnh sửa thông tin và gửi yêu cầu cập nhật.

### **Kết Luận**

- **Bạn đã hoàn thành việc tạo form cập nhật sinh viên với dữ liệu được tiền điền.**
- **Điều này cải thiện trải nghiệm người dùng và cho phép họ dễ dàng chỉnh sửa thông tin sinh viên.**
- **Trong bước tiếp theo, bạn sẽ cần xử lý việc cập nhật thông tin trong cơ sở dữ liệu khi người dùng gửi form này.**

### **Tiếp Theo**

- **Cập nhật `StudentControllerServlet` để xử lý lệnh `"UPDATE"` trong phương thức `doPost`.**
- **Thêm phương thức `updateStudent` trong servlet để lấy dữ liệu từ form và cập nhật sinh viên trong cơ sở dữ liệu.**
- **Cập nhật `StudentDbUtil` với phương thức `updateStudent` để thực hiện câu lệnh SQL `UPDATE`.**

---
