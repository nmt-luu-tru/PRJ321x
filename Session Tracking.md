# Session Tracking

## Giới Thiệu

Trong video này, chúng ta sẽ học cách sử dụng **Session Tracking** với **JSP** để theo dõi và quản lý các hành động của người dùng trong suốt phiên làm việc của họ. Session Tracking là một khái niệm quan trọng trong phát triển ứng dụng web, giúp duy trì trạng thái giữa các yêu cầu HTTP không trạng thái (stateless) từ người dùng.

### Nội Dung Bài Học

- **Demo Session Tracking**: Xem Session Tracking hoạt động như thế nào.
- **Các Bước Lập Trình Session**: Hướng dẫn cách sử dụng các phương thức của đối tượng session trong JSP.
- **Ví Dụ JSP Hoàn Chỉnh**: Kết hợp Session Tracking với một ví dụ thực tế để quản lý danh sách việc cần làm (to-do list) cho từng người dùng.

## 1. Session Tracking Là Gì?

**Session Tracking** trong JSP cho phép bạn theo dõi các hoạt động của người dùng trong suốt phiên làm việc của họ. Một **session** (phiên làm việc) là duy nhất cho mỗi người dùng và thường được sử dụng để lưu trữ các thông tin tạm thời như giỏ hàng trong ứng dụng thương mại điện tử hoặc danh sách việc cần làm trong ứng dụng ghi chú.

### Ví Dụ Ứng Dụng Session

Giả sử bạn có một trang web cho phép người dùng tạo danh sách việc cần làm (to-do list). Mỗi người dùng sẽ có một danh sách riêng biệt mà họ có thể thêm, sửa, hoặc xóa các mục. Dữ liệu này sẽ được lưu trữ trong **session object** và tồn tại cho đến khi người dùng kết thúc phiên làm việc hoặc đăng xuất.

- **Người dùng A** có danh sách việc cần làm: `Mua sắm`, `Tập thể dục`.
- **Người dùng B** có danh sách việc cần làm: `Đi dạo`, `Sửa xe đạp`.

Mỗi người dùng đều có một phiên làm việc độc lập và dữ liệu của họ sẽ không bị ảnh hưởng lẫn nhau.

## 2. Cách Thức Hoạt Động Của Session Object

Khi một người dùng truy cập vào ứng dụng web:

1. **Trình duyệt**: Gửi yêu cầu HTTP tới máy chủ.
2. **Máy chủ (Tomcat)**: Tạo một **session object** cho người dùng nếu đây là lần đầu họ truy cập hoặc sử dụng lại **session object** hiện có.
3. **Session ID**: Mỗi session có một **session ID** riêng biệt để nhận diện người dùng.
4. **Quản lý phiên làm việc**: Dữ liệu của mỗi người dùng được lưu trữ trong bộ nhớ máy chủ (Tomcat server memory) và được truy xuất bằng **session ID**.

### Lưu Ý

- **Session Data** chỉ tồn tại trong bộ nhớ của máy chủ (RAM), không được lưu trên cơ sở dữ liệu hay hệ thống file.
- **Session** sẽ tự động hết hạn nếu không có hoạt động sau một khoảng thời gian nhất định (mặc định là 30 phút trên Tomcat).

## 3. Cách Sử Dụng Session Object Trong JSP

### 3.1. Đặt Dữ Liệu Vào Session

Để lưu trữ thông tin vào session, bạn sử dụng phương thức `session.setAttribute()`.

**Cú Pháp:**
```java
session.setAttribute("tên_biến", giá_trị);
```

**Ví Dụ:**
```java
// Tạo một danh sách các mục việc cần làm
List<String> items = new ArrayList<>();
items.add("Mua sắm");
items.add("Đi tập gym");

// Lưu danh sách vào session với tên biến là "myToDoList"
session.setAttribute("myToDoList", items);
```
Trong ví dụ trên, danh sách `items` sẽ được lưu vào session với tên biến là `myToDoList`.

### 3.2. Lấy Dữ Liệu Từ Session

Để lấy thông tin từ session, bạn sử dụng phương thức `session.getAttribute()`.

**Cú Pháp:**
```java
Object obj = session.getAttribute("tên_biến");
```

**Ví Dụ:**
```java
// Lấy danh sách việc cần làm từ session
List<String> myList = (List<String>) session.getAttribute("myToDoList");
```
Vì phương thức `getAttribute` trả về một đối tượng `Object`, bạn cần phải **down-cast** nó về kiểu dữ liệu tương ứng.

## 4. Các Phương Thức Hữu Ích Khác Của Session Object

Dưới đây là một số phương thức hữu ích của **session object**:

- **`session.getId()`**: Lấy ID của session.
- **`session.isNew()`**: Kiểm tra xem đây có phải là session mới được tạo hay không.
- **`session.invalidate()`**: Hủy session hiện tại (thường dùng khi người dùng đăng xuất).
- **`session.setMaxInactiveInterval(int interval)`**: Đặt khoảng thời gian session sẽ hết hạn nếu không có hoạt động (tính bằng giây).

### Ví Dụ Sử Dụng Các Phương Thức

```jsp
<%
    // Lấy ID của session
    String sessionId = session.getId();
    out.println("Session ID: " + sessionId + "<br>");

    // Kiểm tra xem session có mới không
    boolean isNew = session.isNew();
    out.println("Is session new? " + isNew + "<br>");

    // Đặt khoảng thời gian session hết hạn là 15 phút (900 giây)
    session.setMaxInactiveInterval(900);
%>
```

## 5. Ví Dụ Thực Tế Sử Dụng Session Tracking

Chúng ta sẽ tạo một ví dụ về việc sử dụng session để quản lý danh sách việc cần làm (to-do list) cho từng người dùng.

### 5.1. Tạo Trang `todo-form.html`

Trang HTML này sẽ cho phép người dùng nhập các mục việc cần làm và gửi chúng tới một trang JSP để lưu trữ vào session.

**Nội Dung `todo-form.html`:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>To-Do List</title>
</head>
<body>
    <h2>Enter a new to-do item:</h2>
    <form action="todo-list.jsp" method="post">
        <input type="text" name="newItem">
        <input type="submit" value="Add Item">
    </form>
</body>
</html>
```

### 5.2. Tạo Trang `todo-list.jsp`

Trang JSP này sẽ xử lý các mục việc cần làm từ `todo-form.html`, lưu trữ chúng vào session và hiển thị lại danh sách việc cần làm hiện tại.

**Nội Dung `todo-list.jsp`:**
```jsp
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>To-Do List</title>
</head>
<body>
    <h2>Your To-Do List</h2>

    <%
        // Lấy danh sách từ session, nếu chưa có thì tạo mới
        List<String> toDoList = (List<String>) session.getAttribute("myToDoList");
        if (toDoList == null) {
            toDoList = new ArrayList<>();
        }

        // Lấy dữ liệu từ form và thêm vào danh sách
        String newItem = request.getParameter("newItem");
        if (newItem != null && !newItem.trim().isEmpty()) {
            toDoList.add(newItem);
        }

        // Lưu danh sách vào session
        session.setAttribute("myToDoList", toDoList);
    %>

    <!-- Hiển thị danh sách việc cần làm -->
    <ul>
        <% for (String item : toDoList) { %>
            <li><%= item %></li>
        <% } %>
    </ul>
</body>
</html>
```

### 5.3. Chạy Ứng Dụng Và Kiểm Tra Kết Quả

1. **Mở Trang Form**:
   - Nhấp chuột phải vào tệp `todo-form.html`.
   - Chọn **Run As > Run on Server**.
   - Chọn máy chủ Tomcat đã được cấu hình và nhấn **Finish**.

2. **Điền Thông Tin Và Thêm Mục Việc**:
   - Nhập mục việc cần làm: Ví dụ `Mua sắm`.
   - Nhấn nút **Add Item**.
   - Trang sẽ hiển thị danh sách việc cần làm đã được thêm vào.

3. **Thử Với Nhiều Trình Duyệt**:
   - Mở một trình duyệt khác (ví dụ: Chrome và Firefox).
   - Truy cập vào cùng một URL và thêm các mục việc khác nhau.
   - Mỗi trình duyệt sẽ có một **session object** riêng biệt, do đó danh sách việc cần làm sẽ không bị lẫn lộn giữa các người dùng.

**Kết Quả:**
- **Trình duyệt A (Chrome)**:
  ```
  Your To-Do List
  - Mua sắm
  - Đi tập gym
  ```

- **Trình duyệt B (Firefox)**:
  ```
  Your To-Do List
  - Đi dạo
  - Sửa xe đạp
  ```

## 6. Kết Luận

Trong video này, chúng ta đã học cách sử dụng **Session Tracking** với **JSP** để quản lý dữ liệu người dùng trong suốt phiên làm việc của họ. Cụ thể, chúng ta đã:

1. **Hiểu Rõ Về Session Tracking**:
   - Session Tracking giúp duy trì trạng thái giữa các yêu cầu HTTP không trạng thái.
   - Mỗi người dùng sẽ có một **session object** độc lập để lưu trữ dữ liệu tạm thời.

2. **Sử Dụng Các Phương Thức Cơ Bản của Session Object**:
   - `session.setAttribute("tên_biến", giá_trị)`: Lưu trữ dữ liệu vào session.
   - `session.getAttribute("tên_biến")`: Lấy dữ liệu từ session.
   - `session.invalidate()`: Hủy session hiện tại.
   - `session.getId()`: Lấy ID của session.
   - `session.isNew()`: Kiểm tra xem session có mới không.
   - `session.setMaxInactiveInterval(int interval)`: Đặt khoảng thời gian session hết hạn.

3. **Xây Dựng Ví Dụ Thực Tế**:
   - Tạo trang HTML để nhập dữ liệu.
   - Tạo trang JSP để xử lý và hiển thị dữ liệu đã nhập.
   - Thử nghiệm với nhiều trình duyệt để đảm bảo rằng mỗi người dùng có một **session object** riêng biệt.

### Những Điều Bạn Có Thể Làm Tiếp Theo

- **Tạo Các Trang Đăng Nhập và Đăng Xuất**: Sử dụng session để quản lý trạng thái đăng nhập của người dùng.
- **Tích Hợp Với Cơ Sở Dữ Liệu**: Lưu trữ và truy xuất dữ liệu từ cơ sở dữ liệu thay vì session.
- **Xử Lý Các Phần Tức Thời**: Sử dụng AJAX kết hợp với session để cập nhật dữ liệu mà không cần tải lại trang.
- **Bảo Mật Session**: Áp dụng các biện pháp bảo mật để bảo vệ session khỏi các cuộc tấn công như session hijacking.

Hãy tiếp tục thực hành và khám phá thêm các tính năng của **JSP** để xây dựng các ứng dụng web mạnh mẽ và linh hoạt hơn!
