**Bắt đầu video: Giới thiệu về JSTL Internationalization**

### JSTL Internationalization là gì?

**Internationalization (I18N)** là quá trình thiết kế một ứng dụng sao cho có thể thích ứng với nhiều ngôn ngữ khác nhau mà không cần thay đổi mã nguồn gốc. Đây là một khái niệm phổ biến mà bạn có thể đã thấy khi truy cập các trang web du lịch hoặc khách sạn. Thường thì trên các trang web này, bạn sẽ có tùy chọn chuyển đổi ngôn ngữ bằng cách chọn cờ hoặc biểu tượng đại diện cho ngôn ngữ của các quốc gia khác nhau.

### Ví dụ minh họa

Hãy lấy một ví dụ từ khách sạn JW Marriott tại Bangalore, Ấn Độ. Ở góc trên bên phải, bạn sẽ thấy các biểu tượng cờ đại diện cho các ngôn ngữ khác nhau. Khi bạn chọn ngôn ngữ tiếng Đức, toàn bộ trang web sẽ hiển thị bằng tiếng Đức, thay thế các nhãn và mô tả ban đầu bằng ngôn ngữ tương ứng. Tương tự, bạn có thể chọn tiếng Tây Ban Nha nếu bạn đến từ Tây Ban Nha và muốn xem nội dung bằng tiếng Tây Ban Nha.

### Lợi ích của Internationalization

Điều này giúp việc chuyển đổi ngôn ngữ trên cùng một trang web trở nên dễ dàng mà không cần thay đổi mã nguồn. Khi các lập trình viên xây dựng trang web này, họ đã thiết lập khả năng I18N ngay từ đầu và cho phép trang web tự động thay đổi ngôn ngữ hiển thị chỉ bằng cách thay thế các nhãn hoặc giá trị văn bản theo ngôn ngữ mà người dùng lựa chọn.

### Viết tắt I18N

I18N là cách viết tắt của từ Internationalization. Tại sao lại có cách viết này? Lý do là vì có 18 ký tự giữa chữ cái đầu tiên "I" và chữ cái cuối cùng "N" trong từ "Internationalization". Đây là một cách viết ngắn gọn mà bạn sẽ thường thấy trong tài liệu kỹ thuật hoặc các bài viết liên quan đến phát triển phần mềm đa ngôn ngữ.

### Cách làm Internationalization

Thay vì hard-code (gán cứng) các đoạn văn bản trực tiếp vào mã nguồn, bạn sẽ sử dụng các nhãn (labels) hoặc placeholder cho các thông điệp và văn bản trên trang web. Ví dụ:

- Để chào người dùng, bạn sẽ có một nhãn `greeting`.
- Để hiển thị họ và tên, bạn sẽ có nhãn `firstName` và `lastName`.
- Để đưa ra một thông điệp chào mừng, bạn sẽ có nhãn `welcomeMessage`.

### File ngôn ngữ (Message Bundle)

Để hỗ trợ nhiều ngôn ngữ, bạn cần tạo ra các phiên bản dịch cho từng nhãn tương ứng với mỗi ngôn ngữ mà trang web hỗ trợ. Đây là các file `message bundle` chứa các bản dịch tương ứng với từng ngôn ngữ. Ví dụ: `messages_en_US.properties` cho tiếng Anh, `messages_vi_VN.properties` cho tiếng Việt.

Bạn sẽ cần tạo file này cho mỗi ngôn ngữ mà ứng dụng hỗ trợ. Trong các video tiếp theo, tôi sẽ hướng dẫn bạn cách tạo các file `message bundle` này và sử dụng chúng trong JSP.

### Khái niệm Locale

**Locale** là sự kết hợp giữa ngôn ngữ và khu vực (ngôn ngữ + quốc gia).

Ví dụ:

- `en_US` là tiếng Anh cho khu vực Mỹ.
- `en_GB` là tiếng Anh cho khu vực Anh.

Ngay cả khi cùng sử dụng tiếng Anh, nhưng các khu vực khác nhau vẫn có cách hiển thị và định dạng khác nhau như:

- **Định dạng ngày tháng**: Ở Mỹ (US), định dạng sẽ là Tháng/Ngày/Năm. Trong khi đó ở Anh (GB), định dạng sẽ là Ngày/Tháng/Năm.
- **Định dạng số**: Các dấu ngăn cách phần ngàn và phần thập phân có thể khác nhau.
- **Định dạng tiền tệ**: Dấu hiệu tiền tệ (USD, EUR, GBP,...) sẽ khác nhau.

Lưu ý: Các thẻ định dạng số và tiền tệ chỉ giúp hiển thị đúng ký hiệu, nhưng **không** tự động chuyển đổi tỷ giá. Bạn sẽ cần viết mã Java để chuyển đổi tỷ giá tiền tệ theo yêu cầu.

### Các thẻ JSTL hỗ trợ Internationalization

JSTL cung cấp nhiều thẻ hỗ trợ Internationalization, bao gồm:

1. **Thẻ thiết lập locale**: `fmt:setLocale`, giúp xác định ngôn ngữ và vùng miền mà trang web sẽ sử dụng.
2. **Thẻ bundle thông điệp**: `fmt:setBundle`, sử dụng để chỉ định file chứa các thông điệp đa ngôn ngữ.
3. **Thẻ định dạng số và ngày**: `fmt:formatNumber`, `fmt:formatDate` để hiển thị số và ngày theo đúng định dạng của từng locale.

### Kết luận

Trong các video tiếp theo, tôi sẽ hướng dẫn bạn từng bước để thiết lập một ứng dụng hỗ trợ đa ngôn ngữ bằng cách sử dụng JSP và JSTL. Chúng ta sẽ xây dựng một ứng dụng mẫu và thêm vào đó các định dạng số, ngày tháng, và chuyển đổi ngôn ngữ theo lựa chọn của người dùng.
