# Hướng Dẫn Sử Dụng Thẻ JSTL `split` và `join`: Chi Tiết và Ví Dụ

Trong bài viết này, chúng ta sẽ tìm hiểu về các thẻ chức năng của JSTL (`fn`) dùng để tách (`split`) và nối (`join`) chuỗi trong JSP. Những thẻ này giúp bạn thao tác dễ dàng với chuỗi bằng cách tách một chuỗi thành mảng các chuỗi con hoặc ghép các chuỗi con thành một chuỗi lớn.

## Mục Lục

1. [Giới Thiệu Về Thẻ `split` và `join` trong JSTL](#gioi-thieu)
2. [Cú Pháp Và Cấu Trúc](#cu-phap)
3. [Ví Dụ Sử Dụng Thẻ `split` và `join`](#vi-du)
   - [Ví Dụ 1: Tách Chuỗi Bằng `fn:split`](#vi-du-1)
   - [Ví Dụ 2: Nối Chuỗi Bằng `fn:join`](#vi-du-2)
4. [Lợi Ích Của Việc Sử Dụng Thẻ `split` và `join`](#loi-ich)
5. [Kết Luận](#ket-luan)

---

## 1. Giới Thiệu Về Thẻ `split` và `join` trong JSTL

Trong thư viện JSTL Function (JSTL `fn`), các hàm `split` và `join` được sử dụng để xử lý chuỗi một cách linh hoạt và nhanh chóng. Những hàm này giúp bạn chia một chuỗi thành các chuỗi con hoặc hợp nhất một mảng chuỗi thành một chuỗi duy nhất, giúp mã nguồn trở nên ngắn gọn và dễ đọc hơn.

- **`fn:split()`**: Chia tách một chuỗi thành mảng các chuỗi con dựa trên ký tự phân tách.
- **`fn:join()`**: Nối các chuỗi con từ một mảng thành một chuỗi lớn, với một ký tự phân tách cụ thể.

---

## 2. Cú Pháp Và Cấu Trúc

### `fn:split`

**Cú pháp:**

```jsp
${fn:split(string, delimiter)}
```

- **`string`**: Chuỗi bạn muốn tách.
- **`delimiter`**: Ký tự phân tách để chia chuỗi thành các chuỗi con.

**Trả về**: Một mảng các chuỗi con.

### `fn:join`

**Cú pháp:**

```jsp
${fn:join(array, delimiter)}
```

- **`array`**: Mảng chuỗi bạn muốn nối thành một chuỗi lớn.
- **`delimiter`**: Ký tự phân tách được sử dụng giữa các chuỗi con trong chuỗi đã nối.

**Trả về**: Một chuỗi lớn được nối từ các chuỗi con.

---

## 3. Ví Dụ Sử Dụng Thẻ `split` và `join`

### Ví Dụ 1: Tách Chuỗi Bằng `fn:split`

Trong ví dụ này, chúng ta sẽ tách một chuỗi có chứa các thành phố được phân tách bằng dấu phẩy thành một mảng các chuỗi con.

**Mã nguồn JSP:**

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
    <title>Ví Dụ Tách Chuỗi</title>
</head>
<body>
    <h2>Ví Dụ JSTL Function: Tách Chuỗi</h2>
    <%
        // Khai báo một biến chuỗi chứa tên các thành phố
        String data = "Singapore,Tokyo,Mumbai,London";
        pageContext.setAttribute("data", data);
    %>

    <p>Chuỗi gốc: ${data}</p>

    <%
        // Tách chuỗi thành mảng các chuỗi con
        String[] citiesArray = (String[]) pageContext.getAttribute("data").split(",");
        pageContext.setAttribute("citiesArray", citiesArray);
    %>

    <h3>Các Thành Phố Sau Khi Tách:</h3>
    <c:forEach var="city" items="${citiesArray}">
        <p>${city}</p>
    </c:forEach>
</body>
</html>
```

**Giải thích:**

- **`data = "Singapore,Tokyo,Mumbai,London"`**: Chuỗi chứa danh sách các thành phố, ngăn cách nhau bằng dấu phẩy.
- **`fn:split(data, ',')`**: Tách chuỗi `data` thành một mảng các thành phố bằng dấu phẩy (`","`).
- **`<c:forEach>`**: Lặp qua mảng `citiesArray` và hiển thị từng thành phố trong danh sách.

**Kết quả hiển thị:**

```
Chuỗi gốc: Singapore,Tokyo,Mumbai,London

Các Thành Phố Sau Khi Tách:
- Singapore
- Tokyo
- Mumbai
- London
```

### Ví Dụ 2: Nối Chuỗi Bằng `fn:join`

Trong ví dụ này, chúng ta sẽ nối lại mảng các thành phố đã tách ở ví dụ trên thành một chuỗi lớn sử dụng dấu `*` làm ký tự phân tách.

**Mã nguồn JSP:**

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
    <title>Ví Dụ Nối Chuỗi</title>
</head>
<body>
    <h2>Ví Dụ JSTL Function: Nối Chuỗi</h2>

    <h3>Các Thành Phố Được Tách (Ban Đầu):</h3>
    <c:forEach var="city" items="${citiesArray}">
        <p>${city}</p>
    </c:forEach>

    <%
        // Nối mảng các chuỗi con thành một chuỗi lớn với ký tự phân tách là '*'
        String joinedCities = String.join("*", citiesArray);
        pageContext.setAttribute("joinedCities", joinedCities);
    %>

    <h3>Chuỗi Sau Khi Nối:</h3>
    <p>${joinedCities}</p>
</body>
</html>
```

**Giải thích:**

- **`fn:join(citiesArray, '*')`**: Nối các thành phố từ mảng `citiesArray` thành một chuỗi lớn, sử dụng dấu `*` làm ký tự phân tách.
- **Hiển thị kết quả**: Chuỗi đã được nối từ mảng thành một chuỗi duy nhất với các thành phố được phân tách bằng dấu `*`.

**Kết quả hiển thị:**

```
Các Thành Phố Được Tách (Ban Đầu):
- Singapore
- Tokyo
- Mumbai
- London

Chuỗi Sau Khi Nối:
Singapore*Tokyo*Mumbai*London
```

---

## 4. Lợi Ích Của Việc Sử Dụng Thẻ `split` và `join`

- **Đơn Giản Và Dễ Dùng**: Các hàm `split` và `join` rất dễ sử dụng, giúp bạn thực hiện các thao tác với chuỗi mà không cần viết mã Java phức tạp.
- **Giảm Thiểu Mã Java Trong JSP**: Hạn chế việc sử dụng mã Java trực tiếp trong JSP, giúp mã nguồn trở nên sạch sẽ và dễ bảo trì.
- **Tăng Tính Đọc Hiểu**: Sử dụng các thẻ JSTL giúp mã nguồn JSP trở nên dễ đọc và dễ hiểu hơn.
- **Tích Hợp Tốt Với EL (Expression Language)**: Kết hợp các hàm `fn` với Expression Language giúp việc thao tác dữ liệu trở nên linh hoạt và hiệu quả hơn.

---

## 5. Kết Luận

Thẻ JSTL Function `split` và `join` là các công cụ mạnh mẽ giúp bạn dễ dàng thao tác với chuỗi trong JSP. Thẻ `split` giúp bạn tách một chuỗi thành mảng các chuỗi con dựa trên ký tự phân tách, trong khi `fn:join` cho phép bạn nối các chuỗi con từ một mảng thành một chuỗi lớn. Việc sử dụng các thẻ này giúp bạn xây dựng các trang JSP hiệu quả và dễ bảo trì hơn.

**Tài Nguyên Tham Khảo:**

- [Hướng Dẫn JSTL của Oracle](https://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html)
- [Thư Viện Thẻ JSTL Function](https://javaee.github.io/jstl-api/javax/servlet/jsp/jstl/functions/package-summary.html)

**Chúc bạn thành công trong việc phát triển ứng dụng web với JSTL Function!**
