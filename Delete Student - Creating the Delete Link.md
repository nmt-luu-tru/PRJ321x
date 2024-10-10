### Thêm Liên Kết "Delete" Vào Trang `list-students.jsp`

Trong bước này, chúng ta sẽ thêm một liên kết "Delete" vào trang `list-students.jsp` của chúng ta để cho phép người dùng có thể xóa sinh viên từ danh sách. Mỗi liên kết "Delete" sẽ chứa `studentId` được nhúng trong URL để khi người dùng nhấn "Delete", thông tin về sinh viên đó sẽ được gửi đến servlet của chúng ta để xử lý việc xóa sinh viên khỏi cơ sở dữ liệu.

#### **1. Chỉnh Sửa `list-students.jsp` Để Thêm Liên Kết "Delete"**

Mở file `list-students.jsp` và chỉnh sửa như sau:

1. Thêm cột "Action" vào bảng hiển thị danh sách sinh viên, nếu chưa có.

2. Thêm liên kết "Delete" vào cùng một ô với liên kết "Update". Liên kết này sẽ gọi `StudentControllerServlet` với `command=DELETE` và `studentId` tương ứng của sinh viên.

**Mã lệnh:**

```jsp
<!-- Cập nhật cột Action trong bảng -->
<th>Action</th>

<!-- Trong phần hiển thị từng dòng của bảng, thêm liên kết Delete -->
<td>
    <a href="${tempLink}">Update</a> | 
    <a href="${deleteLink}" onclick="return confirm('Are you sure you want to delete this student?');">Delete</a>
</td>
```

- `tempLink`: Liên kết đến `StudentControllerServlet` với `command=LOAD` và `studentId`.
- `deleteLink`: Liên kết đến `StudentControllerServlet` với `command=DELETE` và `studentId`.

**Ví dụ đầy đủ cho `list-students.jsp`:**

```jsp
<!-- Bảng hiển thị danh sách sinh viên -->
<table border="1">
    <tr>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Email</th>
        <th>Action</th> <!-- Thêm cột Action -->
    </tr>
    
    <!-- Lặp qua danh sách sinh viên và hiển thị từng dòng -->
    <c:forEach var="tempStudent" items="${THE_STUDENT_LIST}">
        <tr>
            <td>${tempStudent.firstName}</td>
            <td>${tempStudent.lastName}</td>
            <td>${tempStudent.email}</td>
            <td>
                <c:url var="tempLink" value="StudentControllerServlet">
                    <c:param name="command" value="LOAD"/>
                    <c:param name="studentId" value="${tempStudent.id}"/>
                </c:url>
                <a href="${tempLink}">Update</a> | 

                <!-- Tạo liên kết Delete tương tự như Update -->
                <c:url var="deleteLink" value="StudentControllerServlet">
                    <c:param name="command" value="DELETE"/>
                    <c:param name="studentId" value="${tempStudent.id}"/>
                </c:url>
                <a href="${deleteLink}" onclick="return confirm('Are you sure you want to delete this student?');">Delete</a>
            </td>
        </tr>
    </c:forEach>
</table>
```

#### **2. Giải Thích Chi Tiết Mã Lệnh**

1. **Định Nghĩa Các Liên Kết `Update` và `Delete`:**

   - **Liên kết `Update`:**
     - Dùng `<c:url>` để tạo liên kết `tempLink` với `command=LOAD` và `studentId`.
     - `tempLink` được sử dụng trong thẻ `<a>` để tạo liên kết cập nhật cho sinh viên.

   - **Liên kết `Delete`:**
     - Dùng `<c:url>` để tạo liên kết `deleteLink` với `command=DELETE` và `studentId`.
     - `deleteLink` được sử dụng trong thẻ `<a>` để tạo liên kết xóa sinh viên.

2. **JavaScript `onclick` Event Handler:**

   - JavaScript `onclick="return confirm('Are you sure you want to delete this student?');"` được thêm vào liên kết "Delete" để hiển thị một hộp thoại xác nhận khi người dùng nhấn vào liên kết.
   - Nếu người dùng nhấn "OK", hàm `confirm()` sẽ trả về `true` và liên kết sẽ tiếp tục điều hướng đến `StudentControllerServlet`.
   - Nếu người dùng nhấn "Cancel", hàm `confirm()` sẽ trả về `false` và việc điều hướng đến `StudentControllerServlet` sẽ bị hủy bỏ.

#### **3. Kiểm Tra Bước 1**

Sau khi thực hiện các thay đổi trên, hãy khởi chạy ứng dụng và kiểm tra:

1. Mở trình duyệt và truy cập vào trang `list-students.jsp`.
2. Kiểm tra xem cột "Action" có hiển thị hai liên kết "Update" và "Delete" cho từng sinh viên hay không.
3. Nhấp vào liên kết "Delete" để xác nhận rằng hộp thoại JavaScript hiển thị thông báo "Are you sure you want to delete this student?".
4. Nhấp "Cancel" để kiểm tra xem liên kết có bị hủy hay không.
5. Nhấp "OK" để kiểm tra liên kết `deleteLink` chứa `studentId` đúng với sinh viên được chọn.

Nếu bạn làm theo đúng các bước trên, bạn sẽ thấy liên kết "Delete" hoạt động chính xác trên trang `list-students.jsp`.

#### **4. Bước Tiếp Theo**

- **Bước 2:** Cập nhật `StudentControllerServlet` để xử lý `command=DELETE` và xóa sinh viên khỏi cơ sở dữ liệu.
- **Bước 3:** Viết mã lệnh JDBC cho phương thức `deleteStudent` trong lớp `StudentDbUtil`.
