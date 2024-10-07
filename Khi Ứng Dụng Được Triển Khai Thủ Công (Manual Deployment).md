Khi bạn thuê một máy chủ và tải lên tệp `.war` hoặc `.ear` chứa toàn bộ mã nguồn của ứng dụng web, quy trình sẽ diễn ra như sau:

### 1. **Máy chủ tiếp nhận và triển khai (deploy) ứng dụng**
- Khi bạn tải tệp `.war` hoặc `.ear` lên, máy chủ sẽ thực hiện các bước triển khai:
  - **Giải nén tệp .war/.ear** để lấy mã nguồn và tài nguyên cần thiết.
  - **Đọc và phân tích các tệp cấu hình** như `web.xml`, `context.xml`, `application.properties`.
  - **Tạo và khởi tạo** tất cả các `Servlet`, `Filter`, `Listener`, và các đối tượng khác theo thứ tự đã cấu hình trong các tệp này.
  - **Khởi tạo kết nối** đến cơ sở dữ liệu hoặc các tài nguyên khác mà ứng dụng yêu cầu (nếu có).

- Sau khi hoàn tất việc triển khai, máy chủ sẽ đưa ứng dụng vào trạng thái **"Sẵn sàng"**. Nghĩa là ứng dụng đã được khởi động hoàn tất, các thành phần cần thiết đã được tạo và có thể xử lý các yêu cầu từ người dùng.

### 2. **Ứng dụng sẵn sàng phục vụ yêu cầu (Request) của người dùng**
- Sau khi triển khai thành công, ứng dụng sẽ bắt đầu lắng nghe các yêu cầu từ người dùng thông qua **các endpoint** (như URL, URI) được cấu hình trong `web.xml` hoặc `ServletMapping`.
- Từ thời điểm này, **bất kỳ người dùng nào** truy cập vào trang web của bạn đều có thể gửi yêu cầu đến ứng dụng và nhận được phản hồi.

### 3. **Ứng dụng đã sẵn sàng trước khi người dùng truy cập**
- Điều này có nghĩa là **ứng dụng đã chạy sẵn** trên máy chủ trước khi bất kỳ người dùng nào truy cập vào.
- Khi người dùng mở trình duyệt và truy cập vào trang web của bạn, yêu cầu của họ sẽ được chuyển đến ứng dụng web đã sẵn sàng trên máy chủ.
- **Dù người dùng tắt tab trình duyệt, đóng trình duyệt hoặc tạm ngừng truy cập**, ứng dụng của bạn vẫn tiếp tục chạy trên máy chủ.

### 4. **Truy cập lại sau khi tắt tab hoặc đóng trình duyệt**
- Khi người dùng tắt tab trình duyệt hoặc đóng trình duyệt, điều này không ảnh hưởng đến trạng thái hoạt động của ứng dụng trên máy chủ.
- Ứng dụng vẫn chạy **liên tục** trên máy chủ, trừ khi máy chủ bị dừng hoặc bạn thực hiện thao tác dừng (undeploy) ứng dụng thủ công.
- Khi người dùng mở lại trình duyệt và truy cập vào trang web, yêu cầu của họ sẽ tiếp tục được gửi đến ứng dụng đã sẵn sàng và đang chạy trên máy chủ như bình thường.

### 5. **Vòng đời ứng dụng và truy cập của người dùng**
- Ứng dụng trên máy chủ có **vòng đời riêng**:
  - Bắt đầu khi máy chủ khởi động hoặc ứng dụng được triển khai.
  - Tiếp tục lắng nghe và xử lý các yêu cầu của người dùng.
  - Kết thúc khi bạn dừng ứng dụng hoặc máy chủ ngừng hoạt động.
- **Yêu cầu của người dùng** không ảnh hưởng trực tiếp đến việc khởi chạy hoặc dừng ứng dụng. Việc tắt tab trình duyệt hoặc ngừng truy cập không khiến ứng dụng ngừng chạy trên máy chủ.

### Ví dụ minh họa
- **Bạn triển khai một ứng dụng web** trên máy chủ Tomcat, ứng dụng này có URL: `http://mywebsite.com/student`.
- Khi ứng dụng đã được triển khai thành công, máy chủ sẽ giữ cho ứng dụng này **luôn chạy** và **sẵn sàng**.
- Khi người dùng A truy cập `http://mywebsite.com/student`, máy chủ nhận yêu cầu từ người dùng A và trả về dữ liệu tương ứng.
- Người dùng A tắt tab trình duyệt của họ, **ứng dụng vẫn tiếp tục chạy** trên máy chủ.
- Sau một lúc, người dùng B truy cập `http://mywebsite.com/student`, máy chủ lại nhận yêu cầu từ người dùng B và tiếp tục xử lý như bình thường.
- Lúc này, ứng dụng của bạn **không cần khởi động lại** mà đã **chạy sẵn** từ trước đó.

### Kết luận
Như vậy, khi bạn tải lên tệp `.war` hoặc `.ear` và ứng dụng được triển khai thành công trên máy chủ, ứng dụng sẽ **chạy liên tục** và **luôn sẵn sàng** để tiếp nhận các yêu cầu từ người dùng, bất kể người dùng mở, đóng hoặc tạm ngừng truy cập trang web của bạn. Trạng thái này chỉ thay đổi khi bạn dừng ứng dụng hoặc tắt máy chủ.
