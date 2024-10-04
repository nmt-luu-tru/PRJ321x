# JSTL Core Tags - Looping with forEach

Trong bài viết này, chúng ta sẽ tìm hiểu về **JSTL Core Tags** trong JavaServer Pages (JSP). JSTL (JSP Standard Tag Library) cung cấp một tập hợp các thẻ chuẩn giúp đơn giản hóa việc viết mã JSP và giảm thiểu sự phụ thuộc vào mã Java trong trang JSP. Chúng ta sẽ tập trung vào các thẻ phổ biến trong thư viện Core của JSTL và cung cấp các ví dụ minh họa cụ thể.

## Mục Lục

1. [Giới Thiệu Về JSTL Core Tags](#gioi-thieu)
2. [Các Thẻ Thường Dùng Trong JSTL Core](#cac-the-thuong-dung)
3. [Sử Dụng JSTL Core Tags Trong JSP](#su-dung-jstl)
   - [Khai Báo Thư Viện Thẻ JSTL](#khai-bao)
   - [Ví Dụ: Sử Dụng Thẻ `c:forEach`](#vi-du-foreach)
4. [Tài Nguyên Tham Khảo](#tai-nguyen)
5. [Kết Luận](#ket-luan)

## 1. Giới Thiệu Về JSTL Core Tags

**JSTL** bao gồm **năm thư viện thẻ** chính:

- Core
- Formatting
- SQL
- XML
- Functions

Trong đó, **thư viện Core** là một trong những thư viện quan trọng nhất, cung cấp khoảng **15 thẻ** để thực hiện các tác vụ phổ biến như điều kiện, vòng lặp, xử lý biến, và xuất dữ liệu.

## 2. Các Thẻ Thường Dùng Trong JSTL Core

Dưới đây là một số thẻ phổ biến trong thư viện JSTL Core:

- **`c:catch`**: Xử lý ngoại lệ trong trang JSP.
- **`c:choose`**, **`c:when`**, **`c:otherwise`**: Thực hiện các điều kiện tương tự như cấu trúc `switch-case`.
- **`c:if`**: Thực hiện điều kiện tương tự như câu lệnh `if`.
- **`c:forEach`**: Lặp qua các phần tử của một tập hợp hoặc mảng.
- **`c:forTokens`**: Lặp qua các phần tử trong một chuỗi dựa trên bộ tách (delimiter).
- **`c:import`**: Nhập nội dung từ một URL khác, có thể là tương đối hoặc tuyệt đối.
- **`c:out`**: Xuất dữ liệu ra trang JSP, tương tự như `System.out.println`.
- **`c:set`**: Thiết lập giá trị cho một biến hoặc thuộc tính.
- **`c:remove`**: Xóa một biến khỏi phạm vi (scope) hiện tại.
- **`c:url`**: Tạo URL với việc mã hóa các tham số cần thiết.
- **`c:param`**: Thêm tham số vào URL hoặc thẻ khác.
- **`c:redirect`**: Chuyển hướng đến một URL khác.

Chúng ta sẽ tập trung vào thẻ **`c:forEach`** trong ví dụ dưới đây.

## 3. Sử Dụng JSTL Core Tags Trong JSP

### Khai Báo Thư Viện Thẻ JSTL

Để sử dụng các thẻ JSTL Core trong trang JSP, bạn cần khai báo thư viện thẻ bằng cách thêm dòng sau vào đầu trang JSP:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

**Giải thích:**

- **`uri`**: Đây là một định danh duy nhất cho thư viện thẻ JSTL Core. **Lưu ý** rằng đây không phải là một URL thực tế và không yêu cầu kết nối Internet. Nó chỉ là một chuỗi định danh để JSP nhận biết thư viện thẻ.
- **`prefix`**: Tiền tố để sử dụng các thẻ trong thư viện này. Thông thường, tiền tố cho JSTL Core là **`c`**.

### Ví Dụ: Sử Dụng Thẻ `c:forEach`

**Mục tiêu:** Lặp qua một danh sách các thành phố và hiển thị chúng trên trang JSP.

**Bước 1: Thiết Lập Dữ Liệu Mẫu**

Trong một ứng dụng thực tế, dữ liệu thường được cung cấp bởi mô hình MVC (Model-View-Controller). Tuy nhiên, để đơn giản, chúng ta sẽ sử dụng một scriptlet để thiết lập dữ liệu mẫu.

```jsp
<%
    // Tạo một mảng các thành phố
    String[] cities = {"Mumbai", "Singapore", "Philadelphia"};

    // Đặt mảng vào phạm vi (scope) trang với tên "myCities"
    pageContext.setAttribute("myCities", cities);
%>
```
**Lưu ý:** Mặc dù chúng ta muốn giảm thiểu việc sử dụng scriptlet trong JSP, nhưng trong ví dụ này, chúng ta chỉ sử dụng để thiết lập dữ liệu mẫu.

**Giải thích** `pageContext.setAttribute`: Trong JSP, đối tượng `pageContext` là một đối tượng tích hợp sẵn (implicit object) cung cấp thông tin về ngữ cảnh của trang JSP hiện tại. Nó cho phép bạn truy cập và thao tác với các thuộc tính và đối tượng trong các phạm vi (scope) khác nhau, bao gồm:  

**-Page Scope** (Phạm vi trang)  

**-Request Scope** (Phạm vi yêu cầu)  

**-Session Scope** (Phạm vi phiên làm việc)  

**-Application Scope** (Phạm vi ứng dụng)  

Phương thức `setAttribute(String name, Object value)` của `pageContext` được sử dụng để đặt một thuộc tính với tên `name` và giá trị `value` vào phạm vi trang. Điều này có nghĩa là thuộc tính này chỉ tồn tại và có thể truy cập được trong trang JSP hiện tại.

**Bước 2: Sử Dụng Thẻ `c:forEach` Để Lặp Qua Danh Sách**

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Ví Dụ JSTL Core Tag</title>
</head>
<body>
    <h2>Danh Sách Các Thành Phố:</h2>
    <ul>
        <c:forEach var="tempCity" items="${myCities}">
            <li>${tempCity}</li>
        </c:forEach>
    </ul>
</body>
</html>
```

**Giải thích:**

- **`<c:forEach>`**: Bắt đầu vòng lặp.
  - **`var="tempCity"`**: Biến tạm thời lưu trữ từng phần tử trong mỗi lần lặp.
  - **`items="${myCities}"`**: Tập hợp hoặc mảng để lặp qua. Sử dụng Expression Language (EL) với cú pháp `${...}` để truy cập thuộc tính.
- **`${tempCity}`**: Hiển thị giá trị của biến `tempCity` trong mỗi lần lặp.

**Kết quả mong muốn:**

- Trang web sẽ hiển thị danh sách các thành phố:
  - Mumbai
  - Singapore
  - Philadelphia

**Toàn bộ mã trang JSP:**

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%
    // Tạo một mảng các thành phố
    String[] cities = {"Mumbai", "Singapore", "Philadelphia"};
    // Đặt mảng vào phạm vi trang với tên "myCities"
    pageContext.setAttribute("myCities", cities);
%>
<!DOCTYPE html>
<html>
<head>
    <title>Ví Dụ JSTL Core Tag</title>
</head>
<body>
    <h2>Danh Sách Các Thành Phố:</h2>
    <ul>
        <c:forEach var="tempCity" items="${myCities}">
            <li>${tempCity}</li>
        </c:forEach>
    </ul>
</body>
</html>
```

**Giải thích bổ sung:**

- **Phạm vi (Scope):** Khi sử dụng JSTL, các biến phải được đặt trong một phạm vi như `page`, `request`, `session`, hoặc `application`. Trong ví dụ này, chúng ta sử dụng `pageContext` để đặt biến trong phạm vi trang.
- **Expression Language (EL):** Cho phép truy cập các thuộc tính và biến một cách ngắn gọn và dễ đọc.

<a name="tai-nguyen"></a>
## 4. Tài Nguyên Tham Khảo

Để tìm hiểu chi tiết về từng thẻ trong thư viện JSTL Core, bạn có thể tham khảo các tài nguyên sau:

- **Hướng Dẫn Về JSTL:**
  - [JSTL Tutorial - Oracle](https://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html)
- **JSTL JavaDoc:**
  - [JSTL 1.2 JavaDoc](https://javadoc.io/doc/javax.servlet/jstl/1.2/index.html)
- **Đặc Tả JSTL:**
  - [JSTL Specification PDF](https://jcp.org/aboutJava/communityprocess/final/jsr052/index.html)
- **Tài Nguyên Khác:**
  - [JSP Standard Tag Library Documentation](https://javaee.github.io/jstl-api/)

<a name="ket-luan"></a>
## 5. Kết Luận

Việc sử dụng **JSTL Core Tags** giúp đơn giản hóa việc viết mã trong trang JSP, giảm thiểu sự phụ thuộc vào scriptlet và tách biệt **business logic** khỏi **presentation layer**. Thẻ `c:forEach` là một trong những thẻ mạnh mẽ và thường dùng nhất, cho phép lặp qua các tập hợp dữ liệu một cách dễ dàng và hiệu quả.

**Lợi Ích Khi Sử Dụng JSTL Core Tags:**

- **Đơn giản hóa mã JSP:** Giảm thiểu việc sử dụng mã Java trực tiếp trong trang JSP.
- **Tăng tính đọc hiểu và bảo trì:** Mã nguồn sạch sẽ, dễ đọc và dễ bảo trì.
- **Tái sử dụng:** Các thẻ chuẩn có thể được sử dụng trong nhiều trang và dự án khác nhau.
- **Tương thích với MVC:** Dễ dàng tích hợp với các mô hình MVC, nơi dữ liệu được cung cấp từ controller.

**Khuyến Nghị:**

- **Học cách sử dụng các thẻ khác trong JSTL Core:** Như `c:if`, `c:choose`, `c:set` để tận dụng tối đa sức mạnh của JSTL.
- **Tránh sử dụng scriptlet trong JSP:** Sử dụng JSTL và Expression Language (EL) để thay thế.
- **Tìm hiểu về Expression Language (EL):** Để truy cập dữ liệu và thuộc tính một cách hiệu quả.

---

**Chúc mừng bạn đã hoàn thành việc tìm hiểu về JSTL Core Tags!**

Nếu bạn có bất kỳ câu hỏi hoặc cần hỗ trợ thêm, đừng ngần ngại liên hệ. Chúc bạn thành công trong việc phát triển ứng dụng web với JSP và JSTL.
