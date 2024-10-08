### Bước 3: Tạo và Sử Dụng CSS trong Ứng Dụng Quản Lý Sinh Viên

Sau khi hoàn thành việc tạo trang JSP và hiển thị danh sách sinh viên, trong bước này, chúng ta sẽ cải thiện giao diện của bảng bằng cách sử dụng **Cascading Style Sheets (CSS)** để làm cho bảng trở nên trực quan và thu hút hơn.

#### 1. Tạo Thư Mục CSS
- Để quản lý CSS một cách có tổ chức, đầu tiên ta sẽ tạo một thư mục chứa các tập tin CSS trong ứng dụng.
  - **Tạo thư mục**: Nhấp chuột phải vào thư mục `WebContent` trong Eclipse, chọn `New` → `Folder`.
  - Đặt tên cho thư mục là **`CSS`** và nhấp vào `Finish` để hoàn tất.

#### 2. Thêm Tập Tin CSS
- Chúng ta sẽ thêm tập tin **style.css** vào thư mục `CSS`:
  - **Copy và Paste**: Trong thư mục `CSS`, nhấp chuột phải và chọn `Paste` để dán tập tin `style.css` đã có sẵn trong thư mục `web_student_tracker_starter_files`.

#### 3. Tham Chiếu CSS trong JSP
- Để liên kết tập tin CSS với trang `list-students.jsp`, ta sẽ thêm một dòng mã vào phần `head` của JSP như sau:
  ```html
  <head>
      <title>Student Tracker App</title>
      <link type="text/css" rel="stylesheet" href="CSS/style.css">
  </head>
  ```
- Ở đây, `href="CSS/style.css"` là đường dẫn đến tập tin CSS mà chúng ta đã thêm vào trong bước trước. Điều này sẽ giúp áp dụng các quy tắc CSS cho bảng HTML của chúng ta.

#### 4. Cấu Trúc HTML với Các Quy Tắc CSS
- Trong tập tin `style.css`, chúng ta đã định nghĩa các quy tắc CSS như sau:
  ```css
  body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
  }

  .wrapper {
      width: 80%;
      margin: 0 auto;
  }

  .header {
      text-align: center;
      margin-bottom: 30px;
  }

  table {
      width: 100%;
      border-collapse: collapse;
  }

  th, td {
      padding: 12px;
      border: 1px solid #ddd;
      text-align: left;
  }

  th {
      background-color: #4CAF50;
      color: white;
  }

  tr:nth-child(even) {
      background-color: #f2f2f2;
  }
  ```
- Đây là những quy tắc cơ bản giúp định dạng bảng và các phần tử khác trên trang JSP.

#### 5. Cập Nhật JSP với Các Thẻ HTML Áp Dụng CSS
- Trong `list-students.jsp`, ta sẽ thêm các `div` và cấu trúc bảng HTML để phù hợp với các quy tắc CSS trên:
  ```html
  <body>
      <div class="wrapper">
          <div class="header">
              <h2>FooBar University</h2>
          </div>

          <div class="content">
              <table>
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
          </div>
      </div>
  </body>
  ```

#### 6. Kiểm Tra Kết Quả
- Sau khi hoàn thành việc cập nhật `list-students.jsp` và thêm CSS, bạn có thể kiểm tra giao diện mới bằng cách **refresh** lại trang trong trình duyệt.
- Bảng hiển thị danh sách sinh viên giờ sẽ có kiểu dáng đẹp hơn, với các màu sắc được định nghĩa bởi CSS. Ví dụ:
  - Hàng đầu tiên của bảng (`th`) sẽ có màu nền xanh lá cây.
  - Các hàng dữ liệu (`td`) sẽ có màu xen kẽ để dễ nhìn hơn.

### Kết Luận
- Sau bước này, bạn đã hoàn thiện phần giao diện của bảng danh sách sinh viên bằng cách sử dụng CSS để cải thiện trực quan. Như vậy, trang JSP của bạn giờ đã có một giao diện đẹp mắt và thân thiện hơn so với bảng HTML đơn giản ban đầu. 

### Tiếp Tục
- Trong video tiếp theo, chúng ta sẽ học cách thêm, cập nhật và xóa sinh viên từ danh sách. Bây giờ bạn có thể thư giãn một chút, vì chúng ta đã hoàn thành một phần quan trọng trong việc tạo giao diện của ứng dụng quản lý sinh viên!
