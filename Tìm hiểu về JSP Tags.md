# Sử dụng JSP Tags: Hướng dẫn chi tiết

Trong bài viết này, chúng ta sẽ tìm hiểu về JSP Tags, cách sử dụng chúng để tối ưu hóa mã nguồn JSP và cải thiện hiệu suất của ứng dụng web. Chúng ta sẽ khám phá các loại JSP Tags, lợi ích của việc sử dụng chúng, và cung cấp các ví dụ cụ thể để minh họa.

## Mục lục

1. [Giới thiệu về JSP Tags](#gioi-thieu-ve-jsp-tags)
2. [Phân loại JSP Tags](#phan-loai-jsp-tags)
   - [JSP Custom Tags](#jsp-custom-tags)
   - [JSP Standard Tag Library (JSTL)](#jsp-standard-tag-library-jstl)
3. [Vấn đề thực tế và giải pháp](#van-de-thuc-te-va-giai-phap)
   - [Sử dụng Scriptlets](#su-dung-scriptlets)
   - [Sử dụng JSP Custom Tags](#su-dung-jsp-custom-tags)
4. [Lợi ích của việc sử dụng JSP Custom Tags](#loi-ich-cua-viec-su-dung-jsp-custom-tags)
5. [Giới thiệu về JSTL](#gioi-thieu-ve-jstl)
   - [Core Tags](#core-tags)
   - [Message Tags](#message-tags)
   - [Function Tags](#function-tags)
   - [XML Tags](#xml-tags)
   - [SQL Tags](#sql-tags)
6. [Ví dụ thực tế: Tạo Custom Tag hiển thị ngày hiện tại](#vi-du-thuc-te-tao-custom-tag-hien-thi-ngay-hien-tai)
7. [Kết luận](#ket-luan)

## 1. Giới thiệu về JSP Tags

JSP Tags là các thẻ đặc biệt trong JSP cho phép chúng ta thực hiện các chức năng phức tạp một cách đơn giản và hiệu quả. Thay vì viết mã Java trực tiếp trong trang JSP, chúng ta có thể sử dụng các thẻ này để tách biệt **business logic** khỏi mã hiển thị, giúp mã nguồn sạch sẽ và dễ bảo trì hơn.

## 2. Phân loại JSP Tags

Có hai loại JSP Tags chính:

### JSP Custom Tags

- **Định nghĩa**: Là các thẻ tùy chỉnh do lập trình viên tạo ra để thực hiện các chức năng cụ thể.
- **Cách sử dụng**: Bạn có thể viết mã Java riêng, đóng gói nó thành một thẻ và sử dụng thẻ đó trong các trang JSP.
- **Ví dụ**: Thẻ `<weatherReport>` để hiển thị báo cáo thời tiết.

### JSP Standard Tag Library (JSTL)

- **Định nghĩa**: Là tập hợp các thẻ chuẩn do Oracle phát triển, cung cấp các chức năng thường dùng trong phát triển web.
- **Các nhóm thẻ chính**:
  - **Core Tags**: Xử lý biến, vòng lặp, điều kiện.
  - **Message Tags**: Quốc tế hóa và định dạng.
  - **Function Tags**: Xử lý chuỗi, tập hợp.
  - **XML Tags**: Phân tích và thao tác với dữ liệu XML.
  - **SQL Tags**: Truy cập cơ sở dữ liệu (không khuyến khích sử dụng trong môi trường sản xuất).

## 3. Vấn đề thực tế và giải pháp

### Sử dụng Scriptlets

**Vấn đề**: Sếp yêu cầu thêm chức năng hiển thị báo cáo thời tiết vào các trang JSP, và cần hoàn thành ngay trong ngày.

- **Cách thực hiện**:
  - Viết mã Java trực tiếp trong trang JSP bằng scriptlets (`<% ... %>`).
  - Mã sẽ kết nối tới dịch vụ thời tiết, gửi yêu cầu, nhận kết quả, phân tích dữ liệu và hiển thị.

- **Hạn chế**:
  - Mã nguồn JSP sẽ chứa nhiều mã Java, làm trang trở nên khó đọc và khó bảo trì.
  - Trộn lẫn giữa **business logic** và mã hiển thị.
  - Không thể tái sử dụng mã trong các trang khác.

### Sử dụng JSP Custom Tags

**Giải pháp tốt hơn**:

- **Cách thực hiện**:
  - Di chuyển toàn bộ **business logic** (kết nối dịch vụ, xử lý dữ liệu) vào một lớp Java hỗ trợ.
  - Tạo một thẻ tùy chỉnh, ví dụ `<weatherReport city="Tên thành phố">`, để sử dụng trong trang JSP.

- **Lợi ích**:
  - Trang JSP trở nên sạch sẽ, tập trung vào mã hiển thị.
  - **Business logic** được tách biệt, dễ bảo trì và tái sử dụng.
  - Thẻ tùy chỉnh có thể được sử dụng trong nhiều trang và dự án khác nhau.

**Ví dụ về sử dụng JSP Custom Tag**:

Trong trang JSP:

```jsp
<weatherReport city="Hanoi" />
```

Trong lớp Java hỗ trợ:

```java
public class WeatherTag extends SimpleTagSupport {
    private String city;

    public void setCity(String city) {
        this.city = city;
    }

    public void doTag() throws JspException, IOException {
        // Logic để lấy dữ liệu thời tiết từ dịch vụ
        String weatherInfo = getWeatherInfo(city);
        JspWriter out = getJspContext().getOut();
        out.println(weatherInfo);
    }

    private String getWeatherInfo(String city) {
        // Kết nối đến API thời tiết và lấy thông tin
        // Trả về chuỗi chứa thông tin thời tiết
    }
}
```

## 4. Lợi ích của việc sử dụng JSP Custom Tags

- **Giảm thiểu mã scriptlet trong JSP**: Giúp trang JSP sạch sẽ và dễ đọc hơn.
- **Tách biệt **business logic** và hiển thị**: Dễ dàng bảo trì và phát triển.
- **Tái sử dụng**: Thẻ tùy chỉnh có thể được sử dụng trong nhiều trang và dự án khác nhau.
- **Tăng tính modular**: Mỗi thẻ đảm nhiệm một chức năng cụ thể, dễ dàng quản lý và mở rộng.

## 5. Giới thiệu về JSTL

JSTL (JSP Standard Tag Library) là một tập hợp các thẻ chuẩn do Oracle phát triển, giúp đơn giản hóa việc viết mã JSP.

### Core Tags

- **Chức năng**: Xử lý biến, vòng lặp, điều kiện, và các thao tác logic cơ bản.
- **Ví dụ**:

  - **Vòng lặp**:

    ```jsp
    <c:forEach var="item" items="${itemList}">
        ${item}
    </c:forEach>
    ```

  - **Điều kiện**:

    ```jsp
    <c:if test="${user != null}">
        Chào, ${user.name}!
    </c:if>
    ```

### Message Tags

- **Chức năng**: Hỗ trợ quốc tế hóa (i18n) và định dạng văn bản.
- **Ví dụ**:

  - **Hiển thị thông điệp quốc tế hóa**:

    ```jsp
    <fmt:message key="welcome.message" />
    ```

### Function Tags

- **Chức năng**: Xử lý chuỗi, tập hợp, và các hàm tiện ích khác.
- **Ví dụ**:

  - **Chuyển chuỗi sang chữ hoa**:

    ```jsp
    ${fn:toUpperCase(string)}
    ```

### XML Tags

- **Chức năng**: Phân tích và thao tác với dữ liệu XML.
- **Ví dụ**:

  - **Đọc và hiển thị dữ liệu XML**:

    ```jsp
    <x:parse var="doc" xml="${xmlData}" />
    <x:out select="$doc/root/element" />
    ```

### SQL Tags

- **Chức năng**: Truy cập cơ sở dữ liệu.
- **Lưu ý**: Không khuyến khích sử dụng trong môi trường sản xuất do vấn đề bảo mật và hiệu suất. Nên sử dụng JDBC hoặc ORM frameworks như Hibernate.

## 6. Ví dụ thực tế: Tạo Custom Tag hiển thị ngày hiện tại

### Bước 1: Tạo lớp xử lý thẻ

```java
package com.example.tags;

import javax.servlet.jsp.tagext.*;
import javax.servlet.jsp.*;
import java.io.*;
import java.text.SimpleDateFormat;
import java.util.Date;

public class CurrentDateTag extends SimpleTagSupport {
    private String format;

    public void setFormat(String format) {
        this.format = format;
    }

    public void doTag() throws JspException, IOException {
        String pattern = (format != null) ? format : "dd/MM/yyyy";
        SimpleDateFormat sdf = new SimpleDateFormat(pattern);
        String currentDate = sdf.format(new Date());
        JspWriter out = getJspContext().getOut();
        out.println(currentDate);
    }
}
```

### Bước 2: Đăng ký thẻ trong TLD (Tag Library Descriptor)

Tạo tệp `example.tld`:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<taglib xmlns="http://java.sun.com/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
                            http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd"
        version="2.1">

    <tlib-version>1.0</tlib-version>
    <short-name>ExampleTags</short-name>
    <uri>http://example.com/tags</uri>

    <tag>
        <name>currentDate</name>
        <tag-class>com.example.tags.CurrentDateTag</tag-class>
        <body-content>empty</body-content>
        <attribute>
            <name>format</name>
            <required>false</required>
        </attribute>
    </tag>
</taglib>
```

### Bước 3: Sử dụng thẻ trong trang JSP

```jsp
<%@ taglib prefix="ex" uri="http://example.com/tags" %>
<!DOCTYPE html>
<html>
<head>
    <title>Sử dụng Custom Tag</title>
</head>
<body>
    <h2>Ngày hiện tại:</h2>
    <p>Định dạng mặc định: <ex:currentDate /></p>
    <p>Định dạng tùy chỉnh: <ex:currentDate format="EEEE, dd MMMM yyyy" /></p>
</body>
</html>
```

### Kết quả

- **Định dạng mặc định**: 30/09/2023
- **Định dạng tùy chỉnh**: Thứ Bảy, 30 Tháng Chín 2023

**Giải thích**:

- **Lớp `CurrentDateTag`**: Xử lý logic để lấy ngày hiện tại với định dạng tùy chọn.
- **Tệp `example.tld`**: Đăng ký thẻ và thuộc tính của nó.
- **Trang JSP**: Sử dụng thẻ với tiền tố `ex` để hiển thị ngày.

**Lưu ý**:

- Đảm bảo cấu trúc thư mục và khai báo đúng để JSP có thể tìm thấy lớp thẻ và tệp TLD.
- Khi triển khai trên máy chủ ứng dụng, cần cấu hình để nhận diện các thẻ tùy chỉnh.

## 7. Kết luận

Việc sử dụng JSP Tags, đặc biệt là Custom Tags và JSTL, giúp cải thiện đáng kể cấu trúc và chất lượng mã nguồn JSP. Nó giúp tách biệt rõ ràng giữa **business logic** và mã hiển thị, tăng tính tái sử dụng và dễ dàng bảo trì ứng dụng web.

**Tóm tắt lợi ích**:

- **Sạch sẽ và dễ đọc**: Mã nguồn JSP không bị lộn xộn với mã Java.
- **Tái sử dụng**: Thẻ tùy chỉnh và JSTL có thể được sử dụng lại trong nhiều trang.
- **Hiệu suất cao**: Giảm tải cho trang JSP bằng cách chuyển logic phức tạp sang các lớp Java.
- **Bảo trì dễ dàng**: **Business logic** được tách biệt, dễ dàng cập nhật và mở rộng.

**Khuyến nghị**:

- **Học cách tạo và sử dụng JSP Custom Tags**: Giúp bạn tùy chỉnh chức năng theo nhu cầu cụ thể.
- **Sử dụng JSTL khi có thể**: Tận dụng các thẻ chuẩn để giảm thiểu mã nguồn và tăng hiệu suất.
- **Tránh sử dụng Scriptlets**: Thay vào đó, sử dụng Expression Language (EL) và JSTL.

**Tài nguyên tham khảo**:

- [Hướng dẫn về JSP Custom Tags](https://docs.oracle.com/javaee/5/tutorial/doc/bnajl.html)
- [Hướng dẫn về JSTL](https://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html)
- [Trang web chính thức của JSTL](https://jcp.org/en/jsr/detail?id=52)

---

**Chúc mừng bạn đã hoàn thành việc tìm hiểu về JSP Tags!**
