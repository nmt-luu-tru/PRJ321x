### Sử Dụng JSTL Để Tối Ưu Trang JSP

Trong bước này, chúng ta sẽ cải tiến mã nguồn trang JSP để tuân theo **best practice** bằng cách sử dụng **JSTL (JavaServer Pages Standard Tag Library)** thay vì sử dụng mã Scriptlet như trước đây. Điều này giúp mã của chúng ta sạch sẽ, dễ đọc và duy trì hơn.

#### 1. Thêm Thư Viện JSTL vào Dự Án
- **Thêm JAR JSTL vào Dự Án**:
  - Mở thư mục `WebContent/WEB-INF/lib` trong Eclipse.
  - Sao chép hai tập tin JAR của JSTL từ thư mục `web_student_tracker_starter_files` → `WebContent/WEB-INF/lib` và dán chúng vào `WebContent/WEB-INF/lib` của dự án.

#### 2. Cập Nhật Trang `list-students.jsp` với JSTL
- Mở tập tin `list-students.jsp` và bắt đầu thay thế mã Scriptlet bằng các thẻ JSTL. 

1. **Thêm JSTL Taglib ở Đầu Trang JSP**:
   ```html
   <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
   ```
   - Đây là thẻ taglib dùng để khai báo thư viện JSTL mà chúng ta sẽ sử dụng. Chúng ta đặt `prefix="c"` để sử dụng các thẻ trong không gian tên `c` của JSTL.

2. **Xóa Phần Import Không Cần Thiết**:
   - Xóa dòng `import` trong Scriptlet:
     ```html
     <%@ page import="java.util.*, com.luv2code.web.jdbc.*" %>
     ```
   - Không cần sử dụng `import` Java theo cách này khi sử dụng JSTL, vì JSTL tự xử lý việc thao tác với các đối tượng mà không cần khai báo import thủ công.

3. **Thay Thế Scriptlet Bằng Thẻ JSTL**:
   - Trước đây, chúng ta sử dụng đoạn mã sau để lấy danh sách sinh viên từ `request`:
     ```java
     <%
         List<Student> theStudents = (List<Student>) request.getAttribute("STUDENT_LIST");
     %>
     ```
   - Với JSTL, chúng ta không cần phải lấy thủ công như vậy. Hãy xóa đoạn mã này.

4. **Sử Dụng Thẻ JSTL `<c:forEach>` Để Hiển Thị Danh Sách Sinh Viên**:
   - Sử dụng `<c:forEach>` để duyệt qua danh sách sinh viên thay vì `for` loop trong Scriptlet:
     ```html
     <c:forEach var="tempStudent" items="${STUDENT_LIST}">
         <tr>
             <td>${tempStudent.firstName}</td>
             <td>${tempStudent.lastName}</td>
             <td>${tempStudent.email}</td>
         </tr>
     </c:forEach>
     ```
   - Ở đây:
     - `var="tempStudent"`: Biến tạm dùng trong vòng lặp để giữ từng sinh viên.
     - `items="${STUDENT_LIST}"`: `STUDENT_LIST` là thuộc tính `request` mà chúng ta đã đặt từ servlet (`request.setAttribute("STUDENT_LIST", students);`).
     - `${tempStudent.firstName}`: Sử dụng Expression Language (EL) để truy cập thuộc tính `firstName` của đối tượng `Student`.

#### 3. Chạy và Kiểm Tra Kết Quả
- Sau khi cập nhật, bạn có thể chạy lại servlet `StudentControllerServlet.java` và kiểm tra kết quả trong trình duyệt.
- Trang JSP sẽ hiển thị danh sách sinh viên tương tự như trước, nhưng giờ mã nguồn JSP của bạn đã được cải tiến với việc sử dụng JSTL, thay vì mã Scriptlet cũ.

### Lợi Ích của JSTL
- **Dễ Duy Trì**: Mã JSP sẽ dễ đọc, dễ bảo trì hơn so với việc sử dụng Scriptlet.
- **Best Practice**: JSTL và Expression Language (EL) là tiêu chuẩn hiện tại cho việc viết mã trong JSP, giúp mã nguồn tuân thủ theo các quy chuẩn tốt nhất.
- **Giảm Thiểu Mã Java Trong JSP**: Giúp tách biệt rõ ràng giữa phần hiển thị (View) và phần xử lý logic (Controller).

### Kết Luận
- Sau bước này, bạn đã cải tiến trang `list-students.jsp` bằng cách thay thế mã Scriptlet với JSTL, giúp mã sạch hơn và dễ duy trì hơn. Ở bước tiếp theo, chúng ta sẽ tiếp tục tối ưu hóa và bổ sung các tính năng khác cho ứng dụng quản lý sinh viên này!
