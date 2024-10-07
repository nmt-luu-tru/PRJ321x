### Vòng Đời Của Ứng Dụng (Application Lifecycle)

Trong bối cảnh ứng dụng web (Java Web Application), vòng đời của ứng dụng bao gồm tất cả các trạng thái từ khi ứng dụng được triển khai và khởi chạy, đến khi ngừng hoạt động và bị loại bỏ khỏi máy chủ ứng dụng. Quá trình này được quản lý bởi máy chủ ứng dụng (như Tomcat, JBoss, hoặc WebSphere), bao gồm việc khởi tạo các thành phần như `Servlet`, `DataSource`, và quản lý các kết nối đến cơ sở dữ liệu.

#### 1. **Các Giai Đoạn Chính Trong Vòng Đời Ứng Dụng**
Vòng đời của một ứng dụng web thông thường có thể được chia thành 4 giai đoạn chính:
1. **Khởi tạo (Initialization)**
2. **Vận hành (Running)**
3. **Dừng (Stopping)**
4. **Hủy (Destruction)**

#### 2. **Chi Tiết Các Giai Đoạn Vòng Đời Ứng Dụng**

##### **1. Khởi Tạo Ứng Dụng (Initialization)**
- **Triển Khai (Deployment)**:
  - Quá trình này diễn ra khi bạn triển khai (deploy) ứng dụng lên máy chủ (server). Máy chủ sẽ đọc và phân tích tệp `web.xml` hoặc các cấu hình của ứng dụng (nếu sử dụng framework như Spring Boot thì sẽ là `application.properties` hoặc `application.yml`).
  
- **Khởi Tạo `Servlet` và Các Thành Phần Liên Quan**:
  - Máy chủ sẽ tải và khởi tạo các `Servlet` được khai báo trong `web.xml` hoặc được đánh dấu bằng annotation như `@WebServlet`.
  - Các đối tượng `ServletContext`, `ServletConfig` sẽ được tạo ra để cung cấp thông tin cấu hình của ứng dụng cho `Servlet`.
  - Nếu bạn có khai báo `@Resource` cho `DataSource` hoặc các tài nguyên khác, máy chủ sẽ khởi tạo và cấu hình `DataSource` theo thông tin được cung cấp.

- **Khởi Tạo Kết Nối `DataSource` và Connection Pool**:
  - `DataSource` sẽ được tạo và khởi động, thiết lập các thông số như `initialSize`, `maxTotal` và `maxIdle`.
  - `DataSource` sẽ tạo sẵn một số lượng kết nối đến cơ sở dữ liệu (theo `initialSize`) và lưu trữ chúng trong connection pool.

##### **2. Ứng Dụng Đang Chạy (Running)**
- **Xử Lý Các Yêu Cầu (Request Handling)**:
  - Khi có yêu cầu từ người dùng (thông qua trình duyệt hoặc API), máy chủ sẽ gửi yêu cầu này đến `Servlet` tương ứng để xử lý.
  - `Servlet` sẽ nhận yêu cầu, tương tác với các thành phần khác như `DataSource` để lấy kết nối và truy vấn dữ liệu từ cơ sở dữ liệu.
  - Sau khi xử lý xong, `Servlet` sẽ gửi phản hồi lại cho người dùng.
  
- **Quản Lý Kết Nối `DataSource`**:
  - `DataSource` sẽ cung cấp các kết nối từ connection pool cho các yêu cầu và sau khi xử lý xong, kết nối sẽ được trả về pool để tái sử dụng.
  - Nếu có nhiều người dùng truy cập cùng lúc, `DataSource` sẽ mở thêm các kết nối (tối đa đến `maxTotal`). Ngược lại, nếu không còn yêu cầu nào đến, các kết nối nhàn rỗi sẽ bị giảm xuống theo `maxIdle`.

##### **3. Dừng Ứng Dụng (Stopping)**
- Quá trình dừng (stopping) xảy ra khi bạn **tắt ứng dụng** hoặc **tắt máy chủ**.
- Máy chủ sẽ lần lượt gọi phương thức `destroy()` trên các `Servlet` để thực hiện các công việc dọn dẹp và giải phóng tài nguyên.
- `DataSource` sẽ đóng tất cả các kết nối hiện có trong pool và giải phóng tài nguyên bộ nhớ.

##### **4. Hủy Ứng Dụng (Destruction)**
- Máy chủ sẽ hủy tất cả các đối tượng liên quan đến ứng dụng, bao gồm `ServletContext`, `ServletConfig`, `Servlet`, `DataSource`, và tất cả các đối tượng kết nối (connection).
- Ứng dụng hoàn toàn bị xóa khỏi bộ nhớ và không còn bất kỳ dữ liệu hoặc trạng thái nào còn tồn tại trên máy chủ.

#### 3. **Tóm Tắt Quy Trình Vòng Đời Ứng Dụng**
- Khi ứng dụng được khởi chạy, các thành phần như `Servlet`, `DataSource`, và connection pool sẽ được khởi tạo.
- Trong quá trình hoạt động, `Servlet` sẽ xử lý các yêu cầu từ người dùng và sử dụng các kết nối từ connection pool của `DataSource`.
- Khi dừng ứng dụng, máy chủ sẽ gọi phương thức `destroy()` trên `Servlet` và hủy connection pool, đóng các kết nối đến cơ sở dữ liệu.
- Cuối cùng, khi hủy ứng dụng, tất cả các đối tượng liên quan đến ứng dụng sẽ bị xóa khỏi bộ nhớ của máy chủ.
