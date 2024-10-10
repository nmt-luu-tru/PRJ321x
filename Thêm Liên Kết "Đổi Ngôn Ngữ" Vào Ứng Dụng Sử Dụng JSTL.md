https://(fu.)udemy.com/course/jsp-tutorial/learn/lecture/4200098#notes  

### Thêm Liên Kết "Đổi Ngôn Ngữ" Vào Ứng Dụng Sử Dụng JSTL

Trong video này, chúng ta sẽ tìm hiểu cách thêm liên kết đổi ngôn ngữ vào trang web sử dụng **JSTL Internationalization**.

#### Giới Thiệu

**Internationalization (I18N)** là quá trình thiết kế một ứng dụng có khả năng thích ứng với nhiều ngôn ngữ khác nhau mà không cần chỉnh sửa mã nguồn. Việc này giúp ứng dụng có thể phục vụ nhiều đối tượng người dùng trên khắp thế giới mà không cần thay đổi code quá nhiều.

Ví dụ phổ biến của internationalization mà bạn có thể thấy là khi bạn truy cập các trang web du lịch hoặc khách sạn, bạn sẽ có tùy chọn để thay đổi ngôn ngữ sang tiếng Anh, tiếng Tây Ban Nha, hoặc các ngôn ngữ khác.

**Trong video này**, chúng ta sẽ xây dựng một ứng dụng hỗ trợ đa ngôn ngữ bằng cách sử dụng **JSTL**. Ứng dụng sẽ cho phép người dùng chọn hiển thị nội dung bằng ba ngôn ngữ: tiếng Anh, tiếng Tây Ban Nha, và tiếng Đức.

### Các Bước Thực Hiện

#### Bước 1: Tạo File Dịch (Resource Files)

Để bắt đầu, chúng ta cần tạo ra các file `properties` cho từng ngôn ngữ mà ứng dụng hỗ trợ. Mỗi file sẽ chứa các nhãn (labels) và thông điệp (messages) tương ứng với ngôn ngữ cụ thể.

- **Định dạng tên file**: Tên file phải tuân theo chuẩn `mylabels_<language-code>_<country-code>.properties`.
  
Ví dụ:

1. `mylabels_en_US.properties`: Cho tiếng Anh, khu vực Mỹ.
2. `mylabels_es_ES.properties`: Cho tiếng Tây Ban Nha, khu vực Tây Ban Nha.
3. `mylabels_de_DE.properties`: Cho tiếng Đức, khu vực Đức.

- **Nội dung file**: Mỗi file sẽ chứa các cặp key-value đại diện cho các nhãn hiển thị trên trang web. Ví dụ:

```properties
# mylabels_es_ES.properties
label.greeting=Hola
label.firstname=Nombre
label.lastname=Apellido
label.welcome_message=Buenos días
```

#### Bước 2: Sử Dụng Nhãn Trong JSP

Tiếp theo, chúng ta sẽ sử dụng các nhãn này trong file JSP của ứng dụng. Thay vì viết trực tiếp văn bản vào mã nguồn, ta sẽ thay thế bằng các placeholder JSTL như `fmt:message` để sử dụng giá trị từ file `properties`.

Ví dụ:

```jsp
<fmt:message key="label.greeting"/>
<fmt:message key="label.firstname"/>
<fmt:message key="label.lastname"/>
<fmt:message key="label.welcome_message"/>
```

Các thẻ này sẽ tự động thay thế bằng giá trị tương ứng với ngôn ngữ mà người dùng chọn.

#### Bước 3: Tạo Liên Kết Đổi Ngôn Ngữ Trên Trang JSP

Ta sẽ thêm các liên kết (links) trên trang JSP để cho phép người dùng chọn ngôn ngữ mong muốn. Các liên kết này sẽ chuyển giá trị `locale` đến servlet, từ đó servlet sẽ tải các file `properties` tương ứng.

- **Tạo liên kết ngôn ngữ**:

```jsp
<a href="yourpage.jsp?theLocale=en_US">English</a> |
<a href="yourpage.jsp?theLocale=es_ES">Español</a> |
<a href="yourpage.jsp?theLocale=de_DE">Deutsch</a>
```

- **Xử lý ngôn ngữ trong JSP**:

```jsp
<c:set var="theLocale" value="${param.theLocale}" />
<fmt:setLocale value="${theLocale}" />
<fmt:setBundle basename="mylabels" />
```

Mã trên sẽ lấy giá trị `theLocale` từ tham số trên URL và đặt làm ngôn ngữ hiển thị cho trang JSP. Ví dụ: Nếu `theLocale` là `es_ES`, trang sẽ hiển thị nội dung tiếng Tây Ban Nha.

#### Bước 4: Thêm Xử Lý Chuyển Ngôn Ngữ Trong Servlet

Trong `Servlet`, chúng ta sẽ xử lý việc đổi ngôn ngữ dựa trên tham số `locale` mà người dùng đã chọn. Sau khi nhận được tham số này, ta sẽ thiết lập lại `locale` cho ứng dụng và chuyển hướng (redirect) người dùng đến trang JSP với ngôn ngữ đã chọn.

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String theLocale = request.getParameter("theLocale");
    if (theLocale != null) {
        // Thiết lập ngôn ngữ ứng dụng dựa trên giá trị người dùng chọn
        request.getSession().setAttribute("theLocale", theLocale);
    }
    // Chuyển hướng về trang chính
    request.getRequestDispatcher("/list-students.jsp").forward(request, response);
}
```

#### Kết Quả Cuối Cùng

Ứng dụng sẽ hiển thị các liên kết ngôn ngữ ở đầu trang. Khi người dùng nhấp vào một ngôn ngữ, trang sẽ tải lại và hiển thị nội dung tương ứng với ngôn ngữ đã chọn. Tất cả nội dung sẽ được lấy từ các file `properties` tương ứng, giúp ứng dụng hỗ trợ đa ngôn ngữ mà không cần thay đổi mã nguồn.

### Kết Luận

Với những bước trên, bạn đã xây dựng thành công một ứng dụng hỗ trợ đa ngôn ngữ bằng cách sử dụng JSTL trong JSP. Trong các video tiếp theo, chúng ta sẽ cùng xây dựng và triển khai ứng dụng mẫu này trong IDE như Eclipse. Hãy tiếp tục theo dõi để khám phá thêm chi tiết và cách triển khai thực tế!
