### Ứng Dụng Được Khởi Chạy Khi Nào?

Ứng dụng web được khởi chạy khi máy chủ web (web server) hoặc máy chủ ứng dụng (application server) thực hiện việc tải (load) và khởi tạo (initialize) ứng dụng. Thời điểm khởi chạy cụ thể có thể khác nhau tùy thuộc vào cấu hình và cách triển khai ứng dụng. Dưới đây là một số tình huống phổ biến mô tả khi nào một ứng dụng web được khởi chạy:

#### 1. **Khi Máy Chủ Được Khởi Động (Server Startup)**
- Khi máy chủ web như Apache Tomcat, JBoss, WebSphere... được khởi động, nó sẽ quét tất cả các ứng dụng web được triển khai trong thư mục ứng dụng (`webapps` của Tomcat chẳng hạn) hoặc trong tập tin cấu hình (`server.xml`, `application.xml`).
- Máy chủ sẽ lần lượt khởi chạy (deploy) từng ứng dụng theo thứ tự và thực hiện việc khởi tạo các `Servlet`, `DataSource`, và các tài nguyên khác được khai báo.
- Đây là thời điểm đầu tiên mà ứng dụng web được khởi chạy và sẵn sàng để nhận yêu cầu từ người dùng.

#### 2. **Khi Ứng Dụng Được Triển Khai Thủ Công (Manual Deployment)**
- Nếu bạn triển khai một ứng dụng mới lên máy chủ đang chạy thông qua việc tải lên tập tin `.war` hoặc `.ear` (các gói ứng dụng Java), máy chủ sẽ tự động giải nén và khởi chạy ứng dụng đó.
- Quá trình này bao gồm:
  - **Giải nén** và **đọc cấu hình** từ tệp `web.xml` hoặc các tệp cấu hình khác.
  - **Tạo các đối tượng cần thiết** như `Servlet`, `Listener`, `Filter`.
  - **Khởi tạo các thành phần** như `DataSource` và connection pool (nếu có).
  - Sau khi các thành phần được khởi tạo xong, ứng dụng mới chính thức được khởi chạy và sẵn sàng phục vụ các yêu cầu.

#### 3. **Khi Ứng Dụng Được Khởi Chạy Lại (Reload / Redeploy)**
- Trong quá trình phát triển, bạn có thể thay đổi mã nguồn hoặc cấu hình của ứng dụng. Khi bạn thực hiện **tải lại (reload)** hoặc **triển khai lại (redeploy)** ứng dụng, máy chủ sẽ thực hiện khởi chạy lại ứng dụng.
- Thao tác này tương tự như khi bạn khởi chạy ứng dụng lần đầu tiên. Máy chủ sẽ xóa bỏ (undeploy) phiên bản cũ của ứng dụng, sau đó tải và khởi chạy lại phiên bản mới.

#### 4. **Khi Có Yêu Cầu Đầu Tiên Từ Người Dùng (Lazy Initialization)**
- Đôi khi, ứng dụng chỉ được khởi chạy khi nhận được **yêu cầu đầu tiên từ người dùng** (đối với những `Servlet` hoặc `Filter` có cấu hình `lazy-init = true` trong `web.xml`).
- Máy chủ sẽ **trì hoãn việc khởi tạo** (lazy initialization) cho đến khi có yêu cầu đến các tài nguyên này. Khi đó, `Servlet` hoặc tài nguyên liên quan mới được khởi tạo lần đầu tiên và sẵn sàng phục vụ yêu cầu.

#### 5. **Khi Có Sự Thay Đổi Trong Cấu Hình Máy Chủ (Server Configuration Change)**
- Nếu bạn thay đổi một số cấu hình của máy chủ liên quan đến ứng dụng (như thay đổi port, context path, hoặc security settings), máy chủ sẽ khởi động lại ứng dụng để áp dụng những thay đổi đó.
- Việc khởi động lại này thường đi kèm với việc dừng các phiên làm việc hiện tại và giải phóng các tài nguyên đang được sử dụng.

### Quy Trình Khởi Chạy Của Ứng Dụng

Khi một ứng dụng web được khởi chạy, nó sẽ trải qua các bước sau:

1. **Tải Ứng Dụng (Loading Application)**
   - Máy chủ đọc cấu hình ứng dụng từ các tệp như `web.xml`, `context.xml`, `application.properties` hoặc các tệp cấu hình khác.
   - Giải nén (nếu ứng dụng được đóng gói dưới dạng `.war` hoặc `.ear`) và đưa vào bộ nhớ.

2. **Khởi Tạo Các Thành Phần (Initialization)**
   - Tạo các đối tượng `Servlet`, `Listener`, `Filter`, và các tài nguyên khác.
   - Khởi tạo các `Servlet` có `load-on-startup` và các `Filter` đã được chỉ định khởi tạo sớm.
   - Nếu có `DataSource`, máy chủ sẽ tạo connection pool và kết nối đến cơ sở dữ liệu.

3. **Sẵn Sàng (Ready)**
   - Sau khi các thành phần được khởi tạo thành công, ứng dụng sẽ chuyển sang trạng thái "Ready" và bắt đầu lắng nghe các yêu cầu từ phía người dùng.
   - Ứng dụng chính thức sẵn sàng xử lý các yêu cầu (request) và trả về phản hồi (response).

4. **Xử Lý Yêu Cầu (Handling Requests)**
   - Khi người dùng gửi yêu cầu đến, máy chủ sẽ chuyển tiếp yêu cầu này đến `Servlet` hoặc thành phần tương ứng để xử lý.
   - `Servlet` sẽ lấy dữ liệu, xử lý logic, và trả về phản hồi.

Như vậy, ứng dụng web sẽ được khởi chạy tại thời điểm máy chủ khởi động, ứng dụng được triển khai hoặc khi nhận được yêu cầu đầu tiên từ người dùng nếu sử dụng `lazy initialization`.
