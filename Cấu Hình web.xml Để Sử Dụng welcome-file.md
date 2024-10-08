### Thêm File `welcome-file` trong `web.xml` Để Tối Ưu URL Của Ứng Dụng

Khi xây dựng ứng dụng web với Servlets và JSP, việc sử dụng URL có chứa chi tiết như tên servlet (`/studentControllerServlet`) sẽ gây khó khăn và không thân thiện với người dùng. Để khắc phục điều này, chúng ta có thể sử dụng **welcome file** trong `web.xml`. Dưới đây là các bước chi tiết để thêm `welcome-file` vào ứng dụng web của bạn:

### Bước 1: Vấn Đề Của URL Hiện Tại

- Khi bạn truy cập vào ứng dụng, bạn phải nhập URL đầy đủ, ví dụ: `http://localhost:8080/web-student-tracker/studentControllerServlet`.
- Điều này không thuận tiện cho người dùng cuối và không phù hợp cho triển khai thực tế.
- Mục tiêu của chúng ta là có thể truy cập ứng dụng chỉ với `http://localhost:8080/web-student-tracker` mà không cần thêm chi tiết tên servlet vào URL.

### Bước 2: Cấu Hình `web.xml` Để Sử Dụng `welcome-file`

**1. Thêm File `web.xml` vào Thư Mục `WEB-INF`**
- `web.xml` là tệp cấu hình triển khai (deployment descriptor) của ứng dụng web Java.
- Di chuyển file `web.xml` từ thư mục `web_student_tracker_starter_files` vào `WebContent/WEB-INF` trong dự án của bạn.

**2. Mở Tệp `web.xml` Và Cấu Hình `welcome-file`**
- Mở tệp `web.xml` trong Eclipse hoặc trình soạn thảo mã nguồn của bạn.
- Thêm `welcome-file` vào bên trong thẻ `<welcome-file-list>` như sau:
  
  ```xml
  <welcome-file-list>
      <!-- URL Mapping của Servlet -->
      <welcome-file>studentControllerServlet</welcome-file>
      
      <!-- File HTML hoặc JSP mặc định -->
      <welcome-file>index.html</welcome-file>
      <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  ```

- **Giải Thích:**
  - `<welcome-file>`: Đây là các file hoặc URL mà server sẽ tìm kiếm và phục vụ khi bạn truy cập vào ứng dụng mà không chỉ định file cụ thể.
  - Trong ví dụ trên, `studentControllerServlet` được đặt lên đầu danh sách. Server sẽ tìm kiếm và thực thi servlet này trước tiên khi người dùng truy cập vào `http://localhost:8080/web-student-tracker`.

### Bước 3: Tạo File `index.html` Để Kiểm Tra `welcome-file`

**1. Tạo File `index.html` Trong Thư Mục `WebContent`**
- Tạo file `index.html` và thêm nội dung sau:
  
  ```html
  <html>
  <head>
      <title>Welcome Page</title>
  </head>
  <body>
      <h1>Hello Brave New World</h1>
  </body>
  </html>
  ```

**2. Cập Nhật `web.xml` Để Kiểm Tra `index.html`**
- Đặt `index.html` vào `web.xml` như là `welcome-file` để xem ứng dụng xử lý trang HTML này như thế nào khi không có chỉ định URL cụ thể.

### Bước 4: Kiểm Tra Và Triển Khai Ứng Dụng

**1. Restart Tomcat**
- Sau khi cập nhật `web.xml`, bạn cần khởi động lại Tomcat để áp dụng các thay đổi cấu hình.

**2. Truy Cập Ứng Dụng Từ Trình Duyệt**
- Truy cập vào `http://localhost:8080/web-student-tracker`.
- Nếu `web.xml` đã được cấu hình chính xác, bạn sẽ thấy trang `index.html` hoặc servlet `studentControllerServlet` được hiển thị mà không cần nhập URL chi tiết.

### Bước 5: Sắp Xếp Thứ Tự `welcome-file`

- **Thứ tự `welcome-file`**:
  - Nếu có nhiều `welcome-file`, thứ tự của các thẻ `<welcome-file>` trong `web.xml` sẽ quyết định file nào được ưu tiên tìm kiếm trước.
  - Trong ví dụ:
    ```xml
    <welcome-file-list>
        <welcome-file>studentControllerServlet</welcome-file>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
    ```
  - `studentControllerServlet` sẽ được tìm trước. Nếu không tìm thấy servlet này, server sẽ tiếp tục tìm `index.html` và `index.jsp` theo thứ tự từ trên xuống dưới.

### Kết Quả

- Sau khi hoàn thành, bạn chỉ cần nhập `http://localhost:8080/web-student-tracker` và ứng dụng sẽ tự động gọi servlet `studentControllerServlet` hoặc trang HTML/JSP mà bạn đã cấu hình làm `welcome-file`.
- Điều này giúp URL của bạn ngắn gọn và thân thiện hơn, đồng thời ẩn đi các chi tiết nội bộ của ứng dụng khỏi người dùng cuối.

### Tóm Lược
- **`web.xml`** là file cấu hình triển khai của ứng dụng web Java.
- **`welcome-file`** giúp xác định file hoặc servlet nào sẽ được gọi khi không có URL cụ thể.
- Sử dụng `welcome-file` để tối ưu URL và trải nghiệm người dùng trong ứng dụng web của bạn.

Bây giờ bạn đã hiểu cách sử dụng `welcome-file` trong `web.xml` để tạo trải nghiệm URL thân thiện hơn cho người dùng cuối!
