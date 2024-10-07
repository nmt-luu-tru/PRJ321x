Đối tượng `DataSource` được khởi tạo **một lần** và tồn tại **suốt vòng đời của ứng dụng**. Đây là một phần rất quan trọng trong việc quản lý **connection pool** và tài nguyên kết nối đến cơ sở dữ liệu. Hãy cùng đi sâu hơn vào việc này:

### 1. **Đối tượng `DataSource` trong vòng đời của ứng dụng**
- Khi ứng dụng web được triển khai (deploy) trên máy chủ, máy chủ sẽ khởi tạo và cấu hình `DataSource` theo các thông tin được chỉ định trong tệp cấu hình như `context.xml` hoặc `web.xml`.
- Sau khi được khởi tạo, đối tượng `DataSource` này sẽ tồn tại **duy nhất** và **không thay đổi** cho đến khi ứng dụng được gỡ bỏ (undeploy) hoặc máy chủ bị tắt.
- `DataSource` là một đối tượng toàn cục (global) của ứng dụng, có thể được các thành phần khác nhau như `Servlet`, `JSP`, hoặc các lớp Java trong ứng dụng sử dụng lại mà **không cần khởi tạo mới**.

### 2. **Quản lý Connection Pool bởi `DataSource`**
- Khi `DataSource` được khởi tạo, nó sẽ **tạo và duy trì một connection pool** (tập hợp các kết nối) theo cấu hình được chỉ định.
  - Ví dụ: Bạn có thể cấu hình `maxTotal`, `maxIdle`, `minIdle`, và `maxWait` để điều chỉnh số lượng kết nối tối đa, tối thiểu và thời gian chờ của connection pool.
- Connection pool sẽ tạo sẵn một số kết nối đến cơ sở dữ liệu (theo cấu hình `minIdle`) và giữ chúng trong bộ nhớ.
- Khi một `Servlet` hoặc lớp Java khác trong ứng dụng yêu cầu một kết nối đến cơ sở dữ liệu, `DataSource` sẽ lấy một kết nối sẵn có từ pool và cấp cho đối tượng đó.
- Sau khi `Servlet` hoặc lớp Java hoàn thành tác vụ, kết nối sẽ không bị đóng mà sẽ được **trả lại connection pool** để tiếp tục sẵn sàng phục vụ cho các yêu cầu tiếp theo.

### 3. **Quy trình hoạt động của `DataSource` với connection pool trong trường hợp nhiều người dùng**
Giả sử có 10 người dùng cùng lúc truy cập vào ứng dụng của bạn trên 10 máy tính khác nhau, quy trình hoạt động của `DataSource` sẽ diễn ra như sau:

1. **Khi ứng dụng được khởi tạo trên máy chủ:**
   - `DataSource` sẽ được tạo một lần duy nhất và connection pool sẽ được khởi tạo với số lượng kết nối ban đầu (`minIdle`) theo cấu hình.
   - Ví dụ: Nếu bạn cấu hình `minIdle=5`, thì 5 kết nối sẽ được tạo sẵn đến cơ sở dữ liệu và lưu trữ trong connection pool.

2. **Người dùng đầu tiên gửi yêu cầu:**
   - `DataSource` nhận yêu cầu kết nối đến cơ sở dữ liệu.
   - Connection pool cấp phát một kết nối sẵn có (ví dụ: kết nối 1) cho người dùng đầu tiên.
   - Sau khi người dùng đầu tiên hoàn thành truy vấn, kết nối 1 được trả lại pool và sẵn sàng phục vụ yêu cầu tiếp theo.

3. **Người dùng thứ hai đến người dùng thứ năm gửi yêu cầu:**
   - Mỗi người dùng sẽ được cấp một kết nối khác nhau từ connection pool, ví dụ:
     - Người dùng thứ hai sử dụng kết nối 2.
     - Người dùng thứ ba sử dụng kết nối 3.
     - ...
   - Connection pool sẽ đảm bảo rằng mỗi người dùng nhận được một kết nối sẵn có mà không cần tạo kết nối mới.

4. **Người dùng thứ sáu gửi yêu cầu:**
   - Nếu tất cả 5 kết nối ban đầu đã được cấp phát, connection pool sẽ kiểm tra giới hạn `maxTotal` (tổng số kết nối tối đa).
   - Nếu còn dư số kết nối so với `maxTotal` (ví dụ `maxTotal=10`), connection pool sẽ **tạo thêm kết nối mới** (ví dụ kết nối 6) và cấp cho người dùng thứ sáu.
   - Nếu đạt đến giới hạn `maxTotal`, người dùng thứ sáu sẽ phải **chờ** đến khi có một kết nối được trả lại vào pool (`maxWait`).

5. **Khi người dùng hoàn tất yêu cầu:**
   - Kết nối mà họ sử dụng sẽ không bị đóng mà **trả về connection pool**.
   - Kết nối đó sẽ được tái sử dụng cho yêu cầu khác từ bất kỳ người dùng nào.

### 4. **Tóm tắt vòng đời của `DataSource` và connection pool:**
- Đối tượng `DataSource` và connection pool chỉ được khởi tạo **một lần duy nhất** khi ứng dụng bắt đầu và tồn tại cho đến khi ứng dụng được gỡ bỏ.
- `DataSource` chịu trách nhiệm **cấp phát và quản lý các kết nối** cho các yêu cầu đến cơ sở dữ liệu.
- Mỗi kết nối trong pool sẽ được **tái sử dụng** liên tục cho các yêu cầu khác nhau.
- Việc tắt hoặc đóng trình duyệt của người dùng không ảnh hưởng đến trạng thái `DataSource` và connection pool. Chúng vẫn tiếp tục hoạt động cho các yêu cầu tiếp theo từ những người dùng khác.

### 5. **Kết luận**
- `DataSource` tạo ra **một lần duy nhất** cho toàn bộ ứng dụng và tồn tại trong suốt vòng đời của ứng dụng.
- `DataSource` duy trì connection pool, giúp quản lý kết nối và tăng hiệu suất truy cập cơ sở dữ liệu.
- Việc có nhiều người dùng cùng truy cập vào ứng dụng không ảnh hưởng đến số lượng `DataSource` tạo ra. Chỉ có **một `DataSource`** duy nhất quản lý tất cả các kết nối và chịu trách nhiệm cấp phát, tái sử dụng kết nối từ connection pool cho mọi yêu cầu của người dùng.
