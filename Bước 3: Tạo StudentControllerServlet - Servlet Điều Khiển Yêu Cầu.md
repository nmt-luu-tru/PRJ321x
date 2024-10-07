### Bước 3: Tạo `StudentControllerServlet` - Servlet Điều Khiển Yêu Cầu

Sau khi đã tạo lớp `Student.java` và lớp `StudentDbUtil.java`, bước tiếp theo là tạo một `Servlet` để điều khiển yêu cầu từ phía người dùng. `Servlet` này sẽ chịu trách nhiệm nhận yêu cầu từ trình duyệt, tương tác với `StudentDbUtil` để lấy danh sách sinh viên từ cơ sở dữ liệu, và cuối cùng chuyển tiếp dữ liệu đến trang JSP để hiển thị. Chúng ta sẽ gọi `Servlet` này là `StudentControllerServlet`.

**Bắt đầu nào!**

1. **Tạo `Servlet` mới**

- Mở `Eclipse IDE` và chọn vào package chứa mã nguồn của bạn.
- Nhấp chuột phải vào package, chọn `New` -> `Servlet`.
- Tại ô `Class Name`, đặt tên cho `Servlet` là **`StudentControllerServlet`**.
- Nhấn `Next` để chuyển sang bước tiếp theo, bỏ chọn các mục `Constructors` và `doPost()`.
- Nhấn `Finish` để hoàn thành.

> Khi nhấn `Finish`, Eclipse sẽ tự động tạo ra một `Servlet` cơ bản với các phương thức như `doGet()` và các cấu trúc cần thiết để khởi động `Servlet`.

2. **Thiết lập `StudentDbUtil` trong `Servlet`**

- Trước tiên, tạo một đối tượng `StudentDbUtil` trong `StudentControllerServlet` để tương tác với cơ sở dữ liệu.
- Khai báo `@Resource` để tiêm vào (`inject`) `DataSource` (nguồn dữ liệu).

```java
@Resource(name="jdbc/web_student_tracker")
private DataSource dataSource;

private StudentDbUtil studentDbUtil;
```

- Tiếp theo, chúng ta cần khởi tạo `StudentDbUtil` với `DataSource`. Để làm điều này, sử dụng phương thức `init()`. Phương thức này sẽ được gọi tự động khi `Servlet` được khởi tạo bởi Tomcat.

```java
@Override
public void init() throws ServletException {
    super.init();
    try {
        // Khởi tạo StudentDbUtil với DataSource đã được inject vào
        studentDbUtil = new StudentDbUtil(dataSource);
    } catch (Exception exc) {
        throw new ServletException(exc);
    }
}
```

> Phương thức `init()` sẽ khởi tạo đối tượng `studentDbUtil` và thiết lập `DataSource` cho nó. Điều này cho phép `Servlet` có thể tương tác với cơ sở dữ liệu một cách hiệu quả mà không cần phải tự tạo kết nối mỗi khi có yêu cầu mới.

3. **Xử lý yêu cầu trong `doGet()`**

- Trong phương thức `doGet()`, chúng ta sẽ gọi đến một phương thức hỗ trợ khác là `listStudents()` để xử lý yêu cầu và trả về danh sách sinh viên.
- Phương thức `listStudents()` sẽ tương tác với `studentDbUtil` để lấy danh sách sinh viên từ cơ sở dữ liệu, thêm dữ liệu này vào `request` và chuyển tiếp (`forward`) dữ liệu đến trang JSP `list-students.jsp` để hiển thị.

```java
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    try {
        // Gọi phương thức listStudents để lấy danh sách sinh viên và hiển thị trên trang JSP
        listStudents(request, response);
    } catch (Exception exc) {
        throw new ServletException(exc);
    }
}
```

4. **Tạo phương thức `listStudents()`**

- `listStudents()` sẽ là nơi thực hiện toàn bộ quá trình từ việc lấy dữ liệu từ `StudentDbUtil` đến việc chuyển tiếp dữ liệu đến JSP.
- Các bước cụ thể trong `listStudents()` bao gồm:
  - Lấy danh sách sinh viên từ `StudentDbUtil`.
  - Đặt danh sách sinh viên vào `request` với tên thuộc tính `"STUDENT_LIST"`.
  - Sử dụng `RequestDispatcher` để chuyển tiếp dữ liệu đến trang `list-students.jsp`.

```java
private void listStudents(HttpServletRequest request, HttpServletResponse response) throws Exception {
    // Bước 1: Lấy danh sách sinh viên từ cơ sở dữ liệu thông qua StudentDbUtil
    List<Student> students = studentDbUtil.getStudents();

    // Bước 2: Thêm danh sách sinh viên vào request với tên thuộc tính "STUDENT_LIST"
    request.setAttribute("STUDENT_LIST", students);

    // Bước 3: Chuyển tiếp dữ liệu đến trang JSP để hiển thị
    RequestDispatcher dispatcher = request.getRequestDispatcher("/list-students.jsp");
    dispatcher.forward(request, response);
}
```

5. **Kết luận**

- `StudentControllerServlet` đảm nhận vai trò điều khiển chính trong mô hình MVC.
- `Servlet` này giúp tổ chức các luồng yêu cầu, tương tác với `StudentDbUtil` để lấy dữ liệu, và chuyển tiếp đến `View` (JSP) để hiển thị.
- Việc sử dụng các phương thức như `init()`, `doGet()`, và `RequestDispatcher` giúp đảm bảo rằng yêu cầu được xử lý một cách trơn tru, dữ liệu được lấy ra từ cơ sở dữ liệu và được hiển thị một cách hiệu quả trên trang JSP.

**Tiếp theo**: Chúng ta sẽ tạo trang JSP `list-students.jsp` để hiển thị danh sách sinh viên mà chúng ta đã lấy được từ cơ sở dữ liệu trong `StudentControllerServlet`.
