### Cập Nhật Trang `list-students.jsp` Để Thêm Cột "Action" Với Liên Kết "Update"

Trong bước này, chúng ta sẽ cập nhật trang `list-students.jsp` để thêm một cột mới tên là "Action", trong đó chứa liên kết "Update" cho mỗi sinh viên. Liên kết này sẽ giúp chúng ta chuyển đến trang cập nhật thông tin sinh viên, với thông tin được điền sẵn dựa trên ID của sinh viên.

#### **Bước 1: Thêm Cột "Action" Vào Tiêu Đề Bảng**

- Mở file `list-students.jsp` trong dự án của bạn.
- Tìm phần tiêu đề của bảng, nơi bạn định nghĩa các cột bằng thẻ `<th>`.
- Thêm một cột mới cho "Action":

```html
<thead>
    <tr>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Email</th>
        <th>Action</th> <!-- Thêm cột Action -->
    </tr>
</thead>
```

#### **Bước 2: Thêm Liên Kết "Update" Cho Mỗi Sinh Viên**

Trong phần thân bảng, nơi bạn sử dụng vòng lặp để hiển thị danh sách sinh viên, chúng ta sẽ thêm liên kết "Update" cho mỗi sinh viên.

1. **Sử dụng Vòng Lặp JSTL `<c:forEach>`**

   Giả sử bạn đang sử dụng vòng lặp `<c:forEach>` để duyệt qua danh sách sinh viên:

```jsp
<tbody>
    <c:forEach var="tempStudent" items="${STUDENT_LIST}">
        <tr>
            <td>${tempStudent.firstName}</td>
            <td>${tempStudent.lastName}</td>
            <td>${tempStudent.email}</td>
            <!-- Chúng ta sẽ thêm cột Action ở đây -->
        </tr>
    </c:forEach>
</tbody>
```

2. **Thêm Cột "Action" Với Liên Kết "Update"**

   Trong mỗi hàng của bảng, thêm một cột mới chứa liên kết "Update":

```jsp
<td>
    <!-- Sử dụng thẻ <c:url> để tạo URL với tham số -->
    <c:url var="tempLink" value="StudentControllerServlet">
        <!-- Thêm tham số command=LOAD -->
        <c:param name="command" value="LOAD" />
        <!-- Thêm tham số studentId với ID của sinh viên hiện tại -->
        <c:param name="studentId" value="${tempStudent.id}" />
    </c:url>
    <!-- Tạo liên kết đến URL vừa tạo -->
    <a href="${tempLink}">Update</a>
</td>
```

**Giải thích chi tiết:**

- **`<c:url>`**: Thẻ JSTL dùng để xây dựng URL động, bao gồm các tham số truyền qua phương thức GET.
  - **`var="tempLink"`**: Tên biến lưu trữ URL đã tạo.
  - **`value="StudentControllerServlet"`**: Đường dẫn đến servlet xử lý yêu cầu.
- **`<c:param>`**: Thêm tham số vào URL.
  - **`name="command"`**: Tên tham số, ở đây là `"command"`.
  - **`value="LOAD"`**: Giá trị tham số, ở đây là `"LOAD"`, cho biết chúng ta muốn tải thông tin sinh viên để cập nhật.
  - **`value="${tempStudent.id}"`**: Lấy ID của sinh viên hiện tại trong vòng lặp.
- **`<a href="${tempLink}">Update</a>`**: Tạo liên kết đến URL vừa tạo, với văn bản hiển thị là "Update".

#### **Bước 3: Kiểm Tra Kết Quả**

- Lưu file `list-students.jsp`.
- Triển khai lại ứng dụng trên máy chủ (nếu cần).
- Mở trình duyệt và truy cập trang danh sách sinh viên.
- Bạn sẽ thấy cột "Action" mới với liên kết "Update" cho mỗi sinh viên.
- Khi di chuột qua liên kết "Update", bạn sẽ thấy URL chứa tham số `studentId` tương ứng với mỗi sinh viên.

**Ví dụ về URL khi di chuột lên liên kết "Update":**

```
http://localhost:8080/your-app-name/StudentControllerServlet?command=LOAD&studentId=5
```

Trong đó, `studentId` sẽ thay đổi tương ứng với ID của sinh viên trong cơ sở dữ liệu.

#### **Bước 4: Giải Thích Ý Nghĩa**

- **Tại sao sử dụng `command=LOAD`?**
  - Trong servlet `StudentControllerServlet`, chúng ta sẽ xử lý tham số `command` để xác định hành động cần thực hiện.
  - Khi `command=LOAD`, servlet sẽ biết rằng chúng ta muốn tải thông tin sinh viên để cập nhật.
- **Truyền `studentId` qua URL:**
  - Chúng ta cần biết ID của sinh viên để truy vấn thông tin từ cơ sở dữ liệu và điền vào form cập nhật.
  - Việc truyền `studentId` qua tham số URL giúp chúng ta xác định sinh viên cần cập nhật.

#### **Bước 5: Tiếp Theo**

- Trong bước tiếp theo, chúng ta sẽ cập nhật `StudentControllerServlet` để xử lý yêu cầu với `command=LOAD`, lấy thông tin sinh viên từ cơ sở dữ liệu và chuyển tiếp đến trang `update-student-form.jsp`.
- Chúng ta cũng sẽ tạo trang `update-student-form.jsp` để hiển thị form cập nhật thông tin sinh viên, với các trường được điền sẵn dữ liệu.

### Tổng Kết

- Bạn đã cập nhật thành công trang `list-students.jsp` để thêm cột "Action" với liên kết "Update" cho mỗi sinh viên.
- Liên kết "Update" được tạo động, chứa tham số `studentId` của sinh viên tương ứng.
- Điều này cho phép chúng ta xác định sinh viên cần cập nhật khi người dùng nhấp vào liên kết "Update".

---

Nếu bạn có bất kỳ câu hỏi nào hoặc cần thêm thông tin chi tiết, hãy cho tôi biết!
