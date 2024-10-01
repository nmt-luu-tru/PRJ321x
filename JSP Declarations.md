# Hướng Dẫn Sử Dụng JSP Declarations

Trong video này, chúng ta sẽ tìm hiểu cách sử dụng **JSP Declarations** để khai báo các phương thức hoặc biến trong trang JSP của bạn. JSP Declarations cho phép bạn tạo và sử dụng lại các phương thức Java trong cùng một trang JSP, giúp cho việc quản lý mã trở nên dễ dàng và linh hoạt hơn.

## 1. JSP Declarations Là Gì?

- **JSP Declarations** cho phép bạn khai báo các phương thức và biến trong trang JSP, giống như cách bạn làm trong một lớp Java thông thường.
- Các phương thức này có thể được gọi và sử dụng lại trong cùng trang JSP.
- Cú pháp của JSP Declarations là: `<%! ...mã_khai_báo... %>`

### Cú Pháp Cơ Bản Của JSP Declarations

```jsp
<%! 
    // Khai báo một phương thức trong JSP
    public String makeItLower(String data) {
        return data.toLowerCase();
    }
%>
```

- Trong ví dụ trên, chúng ta khai báo một phương thức `makeItLower` với tham số đầu vào là một chuỗi `String` và trả về giá trị là chuỗi đã được chuyển đổi thành chữ thường (`toLowerCase`).
- Phương thức này sẽ được sử dụng để chuyển đổi các chuỗi sang dạng chữ thường và có thể được gọi ở bất cứ đâu trong cùng trang JSP này.

## 2. Cách Sử Dụng JSP Declarations Trong Thực Tế

Chúng ta sẽ đi qua một ví dụ cụ thể trong Eclipse để hiểu rõ hơn cách sử dụng JSP Declarations.

### Bước 1: Tạo Tệp `declaration-test.jsp`

1. Mở dự án **JSPDemo** mà bạn đã tạo trong các bài học trước.
2. Nhấp chuột phải vào thư mục **WebContent**, chọn **New** -> **File**.
3. Đặt tên tệp là `declaration-test.jsp` và nhấp **Finish**.

### Bước 2: Viết Mã JSP Declarations Và Gọi Phương Thức

Dán đoạn mã dưới đây vào tệp `declaration-test.jsp`:

```jsp
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JSP Declarations Demo</title>
</head>
<body>
    <h3>Ví dụ về JSP Declarations</h3>
    
    <%! 
        // Khai báo một phương thức chuyển đổi chuỗi thành chữ thường
        public String makeItLower(String data) {
            return data.toLowerCase();
        }
    %>
    
    <!-- Gọi phương thức đã khai báo bằng JSP Expression -->
    <p>Chữ thường của "Hello World" là: 
        <%= makeItLower("Hello World") %>
    </p>
</body>
</html>
```

### Bước 3: Chạy Tệp `declaration-test.jsp`

1. Nhấp chuột phải vào tệp `declaration-test.jsp`.
2. Chọn **Run As** -> **Run on Server**.
3. Chọn máy chủ Tomcat đã được cấu hình trước và nhấp **Finish**.

### Kết Quả

Khi chạy thành công, trình duyệt sẽ mở ra và hiển thị kết quả:

```
Ví dụ về JSP Declarations

Chữ thường của "Hello World" là: hello world
```

- Phương thức `makeItLower` đã được gọi để chuyển chuỗi `"Hello World"` thành chữ thường và kết quả được hiển thị trong trang HTML.

## 3. Lưu Ý Khi Sử Dụng JSP Declarations

- **Hạn chế viết quá nhiều mã Java trong JSP**: Đừng viết quá nhiều phương thức phức tạp trong trang JSP vì điều này có thể khiến mã trở nên khó bảo trì và khó đọc.
- **Tách logic ra thành các lớp Java riêng biệt nếu cần**: Nếu có nhiều logic cần xử lý, hãy tách chúng ra thành các lớp Java riêng biệt để duy trì và quản lý tốt hơn.

## 4. Tổng Kết

Trong bài học này, chúng ta đã tìm hiểu về **JSP Declarations** và cách sử dụng chúng để khai báo các phương thức trong trang JSP. Đây là một công cụ mạnh mẽ cho phép bạn quản lý mã tốt hơn và tái sử dụng các đoạn mã Java trong trang của mình. 

### Những Điều Bạn Có Thể Làm Tiếp Theo

- Thử nghiệm khai báo thêm các phương thức khác như `makeItUpper` để chuyển chuỗi thành chữ in hoa.
- Kết hợp các phương thức này với các thành phần khác như **JSP Scriptlets** hoặc **JSP Expressions** để tạo ra các trang web phức tạp và động hơn.
- Khám phá cách kết hợp giữa JSP và Servlets để xây dựng các ứng dụng web hoàn chỉnh và mạnh mẽ hơn.

Tiếp tục theo dõi các bài học tiếp theo để tìm hiểu thêm về **JSP Directives** và cách tạo ra các thành phần web mạnh mẽ hơn!
