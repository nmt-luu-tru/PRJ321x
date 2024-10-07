### Quy Trình Hoạt Động Của Đối Tượng `DataSource`

`DataSource` là một đối tượng trung gian được sử dụng để quản lý kết nối cơ sở dữ liệu và đóng vai trò như một **connection pool**. Điều này giúp tối ưu hóa hiệu suất khi có nhiều người dùng cùng truy cập vào hệ thống cùng một lúc. Để hiểu rõ hơn về quy trình hoạt động của `DataSource` khi có nhiều người dùng truy cập cùng lúc, chúng ta sẽ xem xét các khía cạnh sau:

1. **Tạo và Quản Lý `DataSource`**
2. **Connection Pool trong `DataSource`**
3. **Luồng Hoạt Động Khi Có Nhiều Người Dùng Truy Cập**
4. **Ví Dụ Mô Tả Quy Trình Hoạt Động**
5. **Kết Luận: `DataSource` Hoạt Động Thế Nào Trong Môi Trường Nhiều Người Dùng**

---

### 1. **Tạo và Quản Lý `DataSource`**

- `DataSource` là một đối tượng **được tạo ra một lần duy nhất** trong suốt vòng đời của ứng dụng. Thông thường, `DataSource` sẽ được cấu hình trong các tệp cấu hình như `context.xml` của Tomcat hoặc qua mã nguồn Java trong lớp khởi tạo của ứng dụng.

- Khi ứng dụng khởi động, `DataSource` sẽ được **khởi tạo và cấu hình** (như cấu hình `maxTotal`, `maxIdle`, `initialSize`), sau đó `DataSource` sẽ tạo ra một tập hợp các kết nối (connection pool) theo các cấu hình này. Những kết nối này sẽ được **lưu trữ** trong `DataSource` và sẵn sàng để phục vụ người dùng.

Ví dụ: 
- Khi `initialSize` được cấu hình là 10, `DataSource` sẽ tạo sẵn 10 kết nối cơ sở dữ liệu khi ứng dụng khởi động.

### 2. **Connection Pool trong `DataSource`**

- **Connection Pool** là một tập hợp các kết nối cơ sở dữ liệu đã được tạo sẵn và quản lý bởi `DataSource`. 

- Khi một người dùng yêu cầu truy cập vào cơ sở dữ liệu (ví dụ như truy vấn thông tin, thêm, sửa, xóa dữ liệu), `DataSource` sẽ **lấy ra một kết nối** từ pool để phục vụ yêu cầu này. Khi yêu cầu kết thúc, kết nối này **sẽ không bị đóng** mà **được trả lại về pool**, để có thể tái sử dụng cho các yêu cầu tiếp theo.

- **Số lượng kết nối** mà `DataSource` tạo ra và quản lý được xác định dựa trên các tham số cấu hình như:
  - **`initialSize`**: Số lượng kết nối được tạo ra ban đầu khi ứng dụng khởi động.
  - **`maxTotal`**: Số lượng kết nối tối đa mà `DataSource` có thể tạo ra khi có nhiều yêu cầu đồng thời.
  - **`maxIdle`**: Số lượng kết nối tối đa mà `DataSource` có thể giữ trong trạng thái nhàn rỗi (không phục vụ yêu cầu nào).

### 3. **Luồng Hoạt Động Khi Có Nhiều Người Dùng Truy Cập**

Giả sử có 10 người dùng cùng truy cập vào một trang web cùng một lúc. Chúng ta sẽ xem xét quy trình hoạt động của `DataSource` trong tình huống này:

1. **Khởi Tạo `DataSource`**:
   - Khi ứng dụng web được triển khai và khởi chạy, `DataSource` sẽ được **khởi tạo một lần duy nhất** và cấu hình kết nối với cơ sở dữ liệu.
   - Nếu `initialSize` là 5, `DataSource` sẽ tạo sẵn **5 kết nối** đến cơ sở dữ liệu và giữ chúng trong connection pool.

2. **Người Dùng Truy Cập**:
   - Mỗi khi một người dùng gửi yêu cầu đến ứng dụng (ví dụ: yêu cầu xem danh sách sinh viên), ứng dụng sẽ **lấy một kết nối** từ connection pool của `DataSource`.
   - Với 10 người dùng cùng truy cập, `DataSource` sẽ lần lượt lấy ra 10 kết nối từ pool. Nếu ban đầu chỉ có 5 kết nối được tạo sẵn (do `initialSize = 5`), `DataSource` sẽ tạo thêm 5 kết nối nữa (vì `maxTotal = 10`).

3. **Sử Dụng Kết Nối**:
   - Khi một kết nối đã được lấy ra từ pool, nó sẽ phục vụ cho yêu cầu của người dùng (truy vấn, cập nhật dữ liệu, xóa dữ liệu,...).
   - Trong suốt quá trình này, kết nối sẽ **không được đóng** mà sẽ **giữ nguyên trạng thái**.

4. **Trả Lại Kết Nối**:
   - Sau khi yêu cầu của người dùng được xử lý xong, kết nối sẽ **không bị đóng lại** mà **được trả về pool**.
   - Khi kết nối được trả lại, `DataSource` sẽ đánh dấu kết nối này là **rảnh rỗi (idle)** và sẵn sàng cho các yêu cầu tiếp theo từ người dùng khác.

5. **Quản Lý Connection Pool**:
   - Nếu sau khi người dùng sử dụng xong và chỉ còn ít người dùng hơn, `DataSource` có thể đóng bớt các kết nối nhàn rỗi dựa trên cấu hình `maxIdle` để giải phóng tài nguyên hệ thống.
   - Ngược lại, nếu có thêm nhiều người dùng truy cập đồng thời vượt quá `maxTotal`, các yêu cầu này sẽ phải **chờ đợi (waiting)** cho đến khi có kết nối được trả về pool.

### 4. **Ví Dụ Mô Tả Quy Trình Hoạt Động**

Giả sử bạn có một ứng dụng web kết nối đến cơ sở dữ liệu và được cấu hình `DataSource` như sau:

```xml
<Resource name="jdbc/web_student_tracker"
          auth="Container"
          type="javax.sql.DataSource"
          maxTotal="10"
          maxIdle="5"
          initialSize="5"
          username="webstudent"
          password="webstudent"
          driverClassName="com.mysql.cj.jdbc.Driver"
          url="jdbc:mysql://localhost:3306/web_student_tracker"/>
```

1. **Khởi Động Ứng Dụng**:
   - `DataSource` được khởi tạo với `initialSize = 5`, nghĩa là sẽ có **5 kết nối** đến cơ sở dữ liệu được tạo sẵn và lưu trong connection pool.

2. **10 Người Dùng Truy Cập**:
   - 10 người dùng cùng gửi yêu cầu đến ứng dụng.
   - `DataSource` sẽ lấy ra 5 kết nối đã được tạo sẵn trong pool và phục vụ cho 5 người dùng đầu tiên.
   - Với 5 người dùng tiếp theo, `DataSource` sẽ tạo thêm **5 kết nối mới** để phục vụ họ.
   - Tại thời điểm này, **tổng số kết nối là 10**.

3. **Người Dùng Kết Thúc Yêu Cầu**:
   - Khi 5 người dùng đầu tiên kết thúc yêu cầu, kết nối sẽ được **trả lại pool** và trạng thái kết nối được đánh dấu là nhàn rỗi.
   - 5 người dùng tiếp theo có thể sử dụng lại những kết nối nhàn rỗi này mà không cần tạo kết nối mới.

4. **Giải Phóng Kết Nối**:
   - Sau một khoảng thời gian nếu số lượng kết nối nhàn rỗi vượt quá `maxIdle = 5`, `DataSource` sẽ **đóng bớt** các kết nối này để duy trì số kết nối nhàn rỗi tối đa là 5.

### 5. **Kết Luận: `DataSource` Hoạt Động Thế Nào Trong Môi Trường Nhiều Người Dùng**

- `DataSource` chỉ **được tạo ra một lần** trong suốt vòng đời của ứng dụng và **không tạo lại** khi có thêm người dùng truy cập.

- `DataSource` quản lý một **connection pool** và tạo sẵn một số kết nối theo cấu hình (`initialSize`).

- Số kết nối tối đa mà `DataSource` có thể quản lý là `maxTotal`. Khi có nhiều người dùng đồng thời truy cập, `DataSource` sẽ phân phối các kết nối từ pool cho các yêu cầu này mà **không tạo lại `DataSource`**.

- Nếu tất cả các kết nối đã được sử dụng và có yêu cầu mới vượt quá `maxTotal`, người dùng sẽ phải **chờ** cho đến khi có kết nối được trả về pool.

Quy trình này giúp `DataSource` quản lý và sử dụng tài nguyên một cách hiệu quả, đồng thời đảm bảo khả năng phục vụ nhiều người dùng cùng một lúc mà không cần phải liên tục tạo mới và đóng kết nối, giúp tăng hiệu suất cho ứng dụng.
