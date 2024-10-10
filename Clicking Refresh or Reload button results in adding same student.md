### Xử lý vấn đề: Click Refresh/Reload dẫn đến thêm sinh viên trùng lặp

**Vấn đề:** 

Khi người dùng nhấp vào nút refresh/reload trên trình duyệt sau khi thêm một sinh viên, sinh viên đó sẽ được thêm lại vào danh sách.

**Giải pháp:**

Để ngăn chặn vấn đề này, chúng ta cần áp dụng **mô hình Post/Redirect/Get (PRG)**. Mô hình này được thiết kế nhằm ngăn chặn việc gửi lại form trùng lặp sau khi người dùng thực hiện thao tác refresh hoặc back trên trình duyệt.

Mô hình PRG hoạt động như sau:
- Khi người dùng nhấn nút submit, dữ liệu sẽ được gửi bằng phương thức `POST`.
- Sau đó, servlet sẽ xử lý dữ liệu, thêm vào cơ sở dữ liệu và cuối cùng chuyển hướng (`redirect`) về trang hiển thị danh sách, sử dụng phương thức `GET`.

**Các bước thực hiện chi tiết:**

### 1. Cập nhật form để sử dụng phương thức `POST`

- **Tệp cần chỉnh sửa:** `add-student-form.jsp`
- Thay đổi phương thức `method` của thẻ `<form>` từ `GET` thành `POST`.

Trước khi thay đổi:

```html
<form action="StudentControllerServlet" method="GET">
```

Sau khi thay đổi:

```html
<form action="StudentControllerServlet" method="POST">
```

### 2. Sửa đổi Servlet để xử lý dữ liệu POST và chuyển hướng

- **Tệp cần chỉnh sửa:** `StudentControllerServlet.java`

#### 2.1 Thay đổi `doGet` để bỏ xử lý `ADD`:

- Xóa đoạn mã sau trong phương thức `doGet`:

```java
case "ADD":
    addStudent(request, response);
    break;
```

#### 2.2 Thêm phương thức `doPost` để xử lý `ADD`:

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    try {
        // đọc tham số "command" từ form
        String theCommand = request.getParameter("command");
                
        // điều hướng đến phương thức phù hợp
        switch (theCommand) {
                        
        case "ADD":
            addStudent(request, response);
            break;
                            
        default:
            listStudents(request, response);
        }
            
    }
    catch (Exception exc) {
        throw new ServletException(exc);
    }
    
}
```

**Giải thích:**
- Phương thức `doPost` sẽ nhận dữ liệu từ form được gửi qua `POST`.
- Phân tích tham số `command`, và nếu giá trị là `"ADD"`, sẽ gọi phương thức `addStudent()` để xử lý.

### 3. Thay đổi phương thức `addStudent` để chuyển hướng sau khi thêm dữ liệu

Thay đổi phương thức `addStudent` trong `StudentControllerServlet` để sử dụng `sendRedirect` sau khi thêm sinh viên vào cơ sở dữ liệu:

```java
private void addStudent(HttpServletRequest request, HttpServletResponse response) throws Exception {

    // đọc thông tin sinh viên từ form
    String firstName = request.getParameter("firstName");
    String lastName = request.getParameter("lastName");
    String email = request.getParameter("email");        
    
    // tạo đối tượng sinh viên mới
    Student theStudent = new Student(firstName, lastName, email);
    
    // thêm sinh viên vào cơ sở dữ liệu
    studentDbUtil.addStudent(theStudent);
            
    // chuyển hướng về trang danh sách sinh viên (tránh thêm trùng lặp khi refresh)
    response.sendRedirect(request.getContextPath() + "/StudentControllerServlet?command=LIST");
}
```

**Giải thích:**
- Thay vì gọi phương thức `listStudents(request, response);` để hiển thị danh sách ngay lập tức, chúng ta sử dụng `response.sendRedirect()` để chuyển hướng trình duyệt đến một URL khác.
- Khi trình duyệt nhận được lệnh `redirect`, nó sẽ thực hiện một yêu cầu `GET` đến URL chỉ định, giúp ngăn chặn việc thêm dữ liệu trùng lặp khi người dùng nhấn `refresh` trên trình duyệt.

### 4. Kiểm tra và xác nhận

- Khởi động lại Tomcat và thử nghiệm với ứng dụng.
- Thực hiện thêm một sinh viên mới và sau đó nhấn nút `refresh/reload` nhiều lần.
- Quan sát danh sách sinh viên. Chỉ có một sinh viên được thêm vào, không có hiện tượng trùng lặp.

### 5. Kết luận

Với mô hình Post/Redirect/Get, chúng ta đã khắc phục được vấn đề thêm trùng lặp dữ liệu khi người dùng nhấn `refresh/reload` trên trình duyệt. Điều này giúp đảm bảo dữ liệu trong cơ sở dữ liệu không bị trùng lặp do các thao tác không mong muốn của người dùng.
