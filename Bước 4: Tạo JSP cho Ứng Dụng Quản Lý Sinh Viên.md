### Bước 4: Tạo JSP cho Ứng Dụng Quản Lý Sinh Viên

Trong video này, chúng ta sẽ tập trung vào việc xây dựng trang JSP để hiển thị danh sách sinh viên trong ứng dụng quản lý sinh viên. 

#### 1. Tạo Tập Tin JSP
- **Tạo tập tin mới**: Trong thư mục `WebContent`, nhấp chuột phải và chọn `New` → `File`, sau đó đặt tên cho tập tin là **`list-students.jsp`**.
- **Mở tập tin**: Sau khi tạo tập tin, chúng ta sẽ mở nó để thêm mã.

#### 2. Nhập Các Gói Cần Thiết
- Đầu tiên, ta cần nhập các gói Java cần thiết để sử dụng trong JSP:
  ```jsp
  <%@ page import="java.util.*" %>
  <%@ page import="com.Luv2code.web.jdbc.*" %>
  ```

#### 3. Thiết Lập Cấu Trúc HTML
- Bây giờ, chúng ta sẽ thêm cấu trúc HTML cơ bản cho trang. Ta sẽ thiết lập tiêu đề trang là **"Student Tracker App"**.
  ```jsp
  <html>
      <head>
          <title>Student Tracker App</title>
      </head>
      <body>
  ```

#### 4. Lấy Danh Sách Sinh Viên
- Ta sẽ sử dụng một scriptlet JSP để lấy danh sách sinh viên từ đối tượng `request` mà Servlet đã thiết lập:
  ```jsp
  <%
      List<Student> theStudents = (List<Student>) request.getAttribute("student_list");
  %>
  ```

#### 5. Xác Nhận Dữ Liệu
- Để xác nhận rằng chúng ta có thể lấy dữ liệu đúng cách, hãy in danh sách sinh viên ra trước khi thiết lập bảng:
  ```jsp
  <%
      for (Student tempStudent : theStudents) {
          out.println(tempStudent); // in thông tin sinh viên
      }
  %>
  ```

#### 6. Tạo Bảng HTML
- Bây giờ, chúng ta sẽ tạo bảng HTML để hiển thị danh sách sinh viên theo định dạng rõ ràng hơn:
  ```jsp
  <table border="1">
      <tr>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Email</th>
      </tr>
      <%
          for (Student tempStudent : theStudents) {
              out.println("<tr>");
              out.println("<td>" + tempStudent.getFirstName() + "</td>");
              out.println("<td>" + tempStudent.getLastName() + "</td>");
              out.println("<td>" + tempStudent.getEmail() + "</td>");
              out.println("</tr>");
          }
      %>
  </table>
  ```

#### 7. Kết Thúc Cấu Trúc HTML
- Đừng quên kết thúc thẻ body và html:
  ```jsp
      </body>
  </html>
  ```

### Kết Quả
- Khi tất cả các bước trên được thực hiện, bạn có thể chạy Servlet `StudentControllerServlet`, và khi trang JSP `list-students.jsp` được tải, nó sẽ hiển thị một bảng danh sách sinh viên với các thông tin như họ tên và địa chỉ email.

### 8. Tiếp Tục
- Trong video tiếp theo, chúng ta sẽ học cách áp dụng Cascading Style Sheets (CSS) để làm cho bảng trông đẹp hơn và hấp dẫn hơn. 

---

Như vậy, bạn đã hoàn thành việc xây dựng trang JSP cho ứng dụng quản lý sinh viên, và sẵn sàng để nâng cấp giao diện trong video tiếp theo!
