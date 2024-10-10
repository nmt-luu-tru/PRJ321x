### Tại Sao Lại Sử Dụng Phương Thức GET Thay Vì POST?

Trong tình huống thực tế của việc tạo form để thêm sinh viên mới vào cơ sở dữ liệu, thông thường chúng ta sẽ sử dụng phương thức **POST** thay vì **GET** để đảm bảo an toàn và bảo mật dữ liệu đầu vào. Tuy nhiên, ở ví dụ trong video, lý do sử dụng phương thức **GET** có thể được giải thích như sau:

#### **1. Tính Đơn Giản Cho Mục Đích Học Tập**
- Ví dụ trong video chủ yếu là để minh họa cách xây dựng ứng dụng từ đầu, giúp người học hiểu về quy trình của một ứng dụng cơ bản.
- Phương thức **GET** đơn giản hơn để hiển thị và kiểm tra dữ liệu ngay trên URL, điều này giúp việc debug (gỡ lỗi) và theo dõi dữ liệu dễ dàng hơn.

#### **2. Sử Dụng GET Để Dễ Dàng Xem Tham Số Truyền Qua URL**
- Khi sử dụng **GET**, các tham số được truyền lên server sẽ hiển thị ngay trên thanh địa chỉ URL (ví dụ: `...?command=ADD&firstName=John&lastName=Doe&email=john.doe@example.com`).
- Điều này giúp dễ dàng kiểm tra và nhìn thấy giá trị của các tham số đang được gửi đi, thuận lợi cho việc học tập và kiểm tra dữ liệu trực tiếp.

#### **3. GET Phù Hợp Với Một Số Chức Năng Đọc Dữ Liệu**
- Phương thức **GET** thường được dùng khi cần **đọc dữ liệu** hoặc **yêu cầu lấy thông tin** từ server mà **không thay đổi trạng thái của ứng dụng** (trong các trường hợp như tìm kiếm, lọc dữ liệu, hoặc tải các trang thông tin).
- Ở ví dụ trên, mặc dù đang thêm sinh viên mới, nhưng tác giả sử dụng GET để minh họa cơ chế truyền tải dữ liệu cơ bản từ form lên servlet và xem kết quả ngay lập tức trên URL.

#### **4. GET Phù Hợp Với Các Thao Tác Đơn Giản**
- GET không yêu cầu thiết lập đặc biệt trên client hoặc server, và thường dễ triển khai hơn khi chỉ cần gửi một lượng dữ liệu nhỏ.
- Với ví dụ trên, do chỉ có ba trường (First Name, Last Name, Email) nên phương thức GET vẫn có thể hoạt động ổn định mà không gặp phải các hạn chế như kích thước dữ liệu truyền tải.

#### **Tuy Nhiên, Tại Sao Thực Tế Nên Sử Dụng POST Cho Form Thêm Dữ Liệu?**

1. **An Toàn Và Bảo Mật Hơn**:
   - Dữ liệu truyền bằng **GET** được hiển thị công khai trên URL, điều này làm tăng nguy cơ bị lộ thông tin nhạy cảm (ví dụ: mật khẩu, thông tin cá nhân, dữ liệu bí mật).
   - **POST** truyền dữ liệu trong phần thân của HTTP request (body), không hiển thị trên URL, giúp bảo mật thông tin tốt hơn.

2. **Không Bị Giới Hạn Kích Thước Dữ Liệu**:
   - **GET** bị giới hạn bởi độ dài của URL (thường khoảng 2048 ký tự tùy trình duyệt và máy chủ).
   - **POST** không bị giới hạn kích thước truyền tải, thích hợp cho việc gửi lượng lớn dữ liệu như form có nhiều trường, tệp tin, hoặc nội dung văn bản dài.

3. **Tránh Được Các Tác Vụ Lặp Lại Không Mong Muốn**:
   - Khi sử dụng **GET**, nếu người dùng nhấn F5 (refresh) hoặc sao chép URL, sẽ dẫn đến việc thực hiện lại hành động thêm dữ liệu (thêm sinh viên).
   - **POST** giúp tránh lặp lại hành động không mong muốn khi refresh, vì khi refresh lại trang, trình duyệt sẽ cảnh báo người dùng trước khi gửi lại request POST.

4. **Phù Hợp Với Các Thao Tác Gây Thay Đổi Dữ Liệu**:
   - **POST** được sử dụng khi cần thay đổi trạng thái của ứng dụng (thêm, sửa, xóa dữ liệu).
   - Đây là tiêu chuẩn chung theo RESTful API: GET để lấy dữ liệu, POST để thêm dữ liệu, PUT để cập nhật và DELETE để xóa dữ liệu.

### Kết Luận
- Trong ví dụ này, việc sử dụng phương thức **GET** chủ yếu nhằm đơn giản hóa quá trình học tập và minh họa cách thức gửi dữ liệu từ form lên servlet.
- Tuy nhiên, **POST** vẫn là phương thức phù hợp và an toàn hơn khi thao tác thêm dữ liệu vào cơ sở dữ liệu, đặc biệt là trong các ứng dụng thực tế hoặc khi triển khai sản phẩm ra môi trường sản xuất.
