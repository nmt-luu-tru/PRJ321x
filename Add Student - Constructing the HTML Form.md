### Tạo Form `add-student-form.jsp` Để Thêm Sinh Viên Mới

#### **Bước 1: Tạo Form `add-student-form.jsp`**

Mục tiêu của bước này là tạo một form HTML để người dùng nhập thông tin cho sinh viên mới. Form sẽ bao gồm các trường nhập liệu cho **First Name**, **Last Name**, và **Email**, cùng với một nút **Save** để gửi dữ liệu lên Servlet. Dưới đây là các bước chi tiết để thực hiện.

**1. Tạo Form HTML Cho Trang `add-student-form.jsp`**
- Đầu tiên, tạo một trang mới với tên `add-student-form.jsp`.
- Thêm cấu trúc HTML cơ bản cho trang này, bao gồm `DOCTYPE`, `head`, `body`, và các `div` để sắp xếp nội dung.

**2. Thêm Các Trường Nhập Liệu**
- Sử dụng `<form>` với `action` trỏ đến `StudentControllerServlet` và `method="get"` để gửi dữ liệu đến servlet.
- Thêm các trường nhập liệu với thẻ `<input>` để người dùng nhập thông tin **First Name**, **Last Name**, và **Email**.
- Sử dụng `<input type="submit">` để tạo nút **Save** và CSS để định dạng nút này.

Dưới đây là mã HTML cho trang `add-student-form.jsp`:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Add Student Form</title>
    <!-- Sử dụng CSS để định dạng giao diện form -->
    <link rel="stylesheet" type="text/css" href="css/style.css">
    <link rel="stylesheet" type="text/css" href="css/add-student-style.css">
</head>
<body>
    <!-- Tiêu đề của form -->
    <div id="header">
        <h1>FooBar University</h1>
    </div>

    <!-- Form nhập liệu sinh viên mới -->
    <div id="content">
        <h3>Add Student</h3>
        <form action="StudentControllerServlet" method="get">
            <!-- Trường ẩn để xác định lệnh gửi lên servlet -->
            <input type="hidden" name="command" value="ADD">

            <!-- Trường nhập liệu cho First Name -->
            <table>
                <tr>
                    <td>First Name:</td>
                    <td><input type="text" name="firstName" required></td>
                </tr>
                <!-- Trường nhập liệu cho Last Name -->
                <tr>
                    <td>Last Name:</td>
                    <td><input type="text" name="lastName" required></td>
                </tr>
                <!-- Trường nhập liệu cho Email -->
                <tr>
                    <td>Email:</td>
                    <td><input type="email" name="email" required></td>
                </tr>
                <!-- Nút Submit -->
                <tr>
                    <td colspan="2" align="right">
                        <input type="submit" value="Save" class="save">
                    </td>
                </tr>
            </table>
        </form>

        <!-- Liên kết quay lại trang danh sách sinh viên -->
        <div>
            <a href="StudentControllerServlet">Back to List</a>
        </div>
    </div>
</body>
</html>
```

**Giải thích:**
1. **CSS References (`<link>`)**:
   - Chúng ta liên kết đến 2 file CSS để định dạng form (`style.css` và `add-student-style.css`).
   
2. **Form (`<form>`)**:
   - Sử dụng `action="StudentControllerServlet"` để chỉ định địa chỉ servlet sẽ xử lý dữ liệu form.
   - Thêm `method="get"` để gửi dữ liệu thông qua phương thức `GET`.
   - **Trường ẩn**: `<input type="hidden" name="command" value="ADD">` được sử dụng để truyền tham số `command` đến servlet, giúp servlet biết được yêu cầu thêm sinh viên.

3. **Các Trường Nhập Liệu**:
   - Sử dụng `<input type="text">` cho **First Name** và **Last Name**.
   - Sử dụng `<input type="email">` cho **Email**, đảm bảo người dùng nhập đúng định dạng email.

4. **Nút Save**:
   - Sử dụng `<input type="submit" value="Save" class="save">` để tạo nút submit và áp dụng CSS với class `save`.

5. **Liên kết quay lại trang danh sách sinh viên**:
   - Tạo một liên kết `<a href="StudentControllerServlet">Back to List</a>` để quay lại trang danh sách sinh viên (`list_students.jsp`).

#### **Bước 2: Thêm CSS Định Dạng Form**
- Tạo file CSS `add-student-style.css` và `style.css` để định dạng trang `add-student-form.jsp`.
- Tạo mới file `add-student-style.css` và dán nội dung CSS từ file starter hoặc tham khảo từ bên ngoài để định dạng nút và bảng.

#### **Bước 3: Kiểm Tra Form Trên Trình Duyệt**
- Truy cập trang `add-student-form.jsp` để kiểm tra.
- Khi nhấn vào nút **Add Student** trên `list_students.jsp`, trang form sẽ xuất hiện với các trường nhập liệu đã định dạng.

#### **Bước 4: Hoàn Thiện Tính Năng Gửi Dữ Liệu**
- Trong video tiếp theo, chúng ta sẽ xử lý việc nhận dữ liệu từ form và lưu vào cơ sở dữ liệu thông qua Servlet `StudentControllerServlet` và `StudentDbUtil`.

### Tổng Kết
- Chúng ta đã tạo thành công form `add-student-form.jsp` với đầy đủ các trường nhập liệu và nút **Save**.
- Sẵn sàng cho việc xử lý dữ liệu trên servlet trong bước tiếp theo.
- Bạn đã làm rất tốt, hẹn gặp bạn trong video tiếp theo để hoàn thiện chức năng thêm sinh viên mới vào cơ sở dữ liệu!
