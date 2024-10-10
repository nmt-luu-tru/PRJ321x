### Cập Nhật Trang `list_students.jsp` Để Thêm Nút "Add Student"

#### **Bước 1: Thêm Nút "Add Student" Vào `list_students.jsp`**

Đầu tiên, chúng ta sẽ cập nhật trang `list_students.jsp` để thêm nút **Add Student**. Đây là bước đầu tiên trong việc thêm tính năng cho phép người dùng nhập dữ liệu của sinh viên mới. Chúng ta sẽ thêm nút này dưới phần tiêu đề và nội dung của trang.

**Cách thực hiện:**
1. Mở file `list_students.jsp`.
2. Di chuyển xuống dưới phần `header` của trang, nơi chúng ta sẽ đặt nút này.
3. Thêm một đoạn HTML để tạo nút **Add Student** với một sự kiện `onclick` sẽ dẫn người dùng đến form nhập liệu.

```html
<!-- Button to Add Student -->
<input type="button" class="add-student-button" value="Add Student" onclick="window.location.href='add-student-form.jsp';">
```

**Giải thích:**
- **`<input type="button">`**: Tạo một nút với kiểu là `button`.
- **`class="add-student-button"`**: Áp dụng CSS style `add-student-button` từ file CSS đã được tạo trước đó, giúp định dạng nút với màu sắc và viền bo tròn đẹp mắt.
- **`onclick="window.location.href='add-student-form.jsp';"`**: Khi người dùng nhấn nút, hệ thống sẽ điều hướng đến trang `add-student-form.jsp`, trang này sẽ chứa form nhập liệu cho sinh viên.

#### **Bước 2: Chạy Ứng Dụng Và Kiểm Tra**
- Chạy lại ứng dụng trên Tomcat.
- Truy cập vào trang `list_students.jsp` và kiểm tra nút **Add Student** vừa được thêm.
- Bạn sẽ thấy nút xuất hiện ở dưới phần tiêu đề, và khi nhấn vào, sẽ xuất hiện lỗi 404 (vì trang `add-student-form.jsp` chưa được tạo).

#### **Bước 3: Xử Lý Lỗi 404 Bằng Cách Tạo File `add-student-form.jsp`**

Lý do chúng ta gặp lỗi 404 là vì trang `add-student-form.jsp` chưa tồn tại. Hãy tạo một file mới với tên `add-student-form.jsp` để giải quyết vấn đề này.

1. Tạo một file mới trong thư mục `WebContent` với tên `add-student-form.jsp`.
2. Thêm nội dung placeholder đơn giản vào file này, ví dụ:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Add Student Form</title>
</head>
<body>
    <h2>Add Student Form</h2>
    <p>Placeholder for Add Student form. This page will be updated with a form in the next steps.</p>
</body>
</html>
```

#### **Bước 4: Kiểm Tra Lại**
- Truy cập lại trang `list_students.jsp`.
- Nhấn vào nút **Add Student**.
- Bạn sẽ thấy trang `add-student-form.jsp` được hiển thị với nội dung placeholder, chứng tỏ rằng việc điều hướng từ `list_students.jsp` đến `add-student-form.jsp` đã thành công.

#### **Bước 5: Kế Hoạch Cho Bước Tiếp Theo**
- Trong video tiếp theo, chúng ta sẽ tiến hành xây dựng form nhập liệu cho trang `add-student-form.jsp`.
- Form sẽ bao gồm các trường nhập liệu cho **First Name**, **Last Name**, và **Email**, cùng với một nút **Save** để gửi dữ liệu.

### Tổng Kết
- Chúng ta đã cập nhật thành công trang `list_students.jsp` để thêm một nút **Add Student**.
- Đã xử lý lỗi 404 bằng cách tạo trang `add-student-form.jsp` và thêm nội dung đơn giản để kiểm tra việc điều hướng.
- Trong video tiếp theo, chúng ta sẽ xây dựng form chi tiết và xử lý dữ liệu từ form để thêm sinh viên vào cơ sở dữ liệu.

Hẹn gặp lại bạn ở video tiếp theo để tiếp tục xây dựng tính năng này!
