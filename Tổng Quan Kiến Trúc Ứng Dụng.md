### Tổng Quan Kiến Trúc Ứng Dụng Quản Lý Sinh Viên Sử Dụng Servlets, JSP và JDBC

Trong dự án này, chúng ta sẽ cùng nhau xây dựng một ứng dụng quản lý sinh viên hoàn chỉnh cho một trường đại học giả lập, **FooBar University**. Ứng dụng sẽ cho phép thực hiện các chức năng cơ bản như liệt kê danh sách sinh viên, thêm mới, cập nhật và xóa thông tin sinh viên. Để đảm bảo mã nguồn được tổ chức một cách rõ ràng, dễ bảo trì và mở rộng, chúng ta sẽ sử dụng kiến trúc **MVC (Model-View-Controller)** kết hợp với **DAO (Data Access Object)** để tương tác với cơ sở dữ liệu.

#### 1. Tính Năng Của Ứng Dụng
Ứng dụng này sẽ hỗ trợ các tính năng chính như:
- **Liệt kê sinh viên**: Hiển thị danh sách toàn bộ sinh viên trong cơ sở dữ liệu.
- **Thêm mới sinh viên**: Nhập và lưu trữ thông tin của sinh viên mới vào cơ sở dữ liệu.
- **Cập nhật thông tin sinh viên**: Chỉnh sửa thông tin của sinh viên hiện có.
- **Xóa sinh viên**: Xóa thông tin của một sinh viên khỏi cơ sở dữ liệu.

#### 2. Kiến Trúc Tổng Quan Của Ứng Dụng
Ứng dụng sẽ được xây dựng theo mô hình **MVC**, trong đó:
- **Model (Mô Hình)**: Là lớp đối tượng đại diện cho dữ liệu và logic xử lý nghiệp vụ. Model sẽ tương tác với cơ sở dữ liệu thông qua một lớp DAO (Data Access Object) để thực hiện các thao tác như truy vấn, thêm mới, cập nhật và xóa dữ liệu.
- **View (Giao Diện)**: Là các trang JSP (Java Server Pages) hiển thị dữ liệu cho người dùng. View chỉ chịu trách nhiệm hiển thị dữ liệu mà không chứa các logic xử lý nghiệp vụ.
- **Controller (Bộ Điều Khiển)**: Là các Servlet chịu trách nhiệm nhận yêu cầu từ người dùng, điều hướng xử lý thông qua Model và cập nhật lại giao diện thông qua View.

#### 3. Sơ Đồ Hoạt Động Của Ứng Dụng
Dưới đây là luồng hoạt động của ứng dụng theo mô hình MVC:
1. **Người dùng** gửi yêu cầu (request) từ trình duyệt, ví dụ: yêu cầu liệt kê danh sách sinh viên.
2. **Controller Servlet** (StudentController) sẽ nhận yêu cầu này, sau đó gọi lớp DAO (StudentDAO) để tương tác với cơ sở dữ liệu.
3. Lớp **StudentDAO** sẽ thực hiện các thao tác JDBC để kết nối với cơ sở dữ liệu MySQL, truy vấn dữ liệu từ bảng `student` và trả kết quả về cho Controller Servlet.
4. **Controller Servlet** nhận kết quả, chuyển dữ liệu sang đối tượng `request` và chuyển tiếp yêu cầu tới trang **JSP** (View) tương ứng.
5. **View (JSP)** hiển thị danh sách sinh viên cho người dùng dưới dạng bảng HTML.
6. Trình duyệt của người dùng sẽ hiển thị thông tin kết quả.

#### 4. Giới Thiệu Về Student Database Utility (StudentDAO)
Lớp **StudentDAO** sẽ chịu trách nhiệm chính trong việc tương tác với cơ sở dữ liệu MySQL. Đây là một phần của mô hình **DAO (Data Access Object)**. DAO là một **design pattern (mẫu thiết kế)** phổ biến trong các dự án phát triển Java, giúp cô lập toàn bộ mã nguồn liên quan đến truy vấn cơ sở dữ liệu vào một hoặc một nhóm lớp cụ thể, giúp mã nguồn dễ bảo trì và mở rộng hơn.

Các lợi ích của DAO trong ứng dụng:
- **Tách biệt logic truy vấn cơ sở dữ liệu**: DAO giúp tách biệt hoàn toàn các câu lệnh SQL và các phương thức truy vấn ra khỏi lớp Controller, giúp Controller chỉ tập trung vào việc điều phối xử lý.
- **Tái sử dụng mã nguồn**: Một lớp DAO có thể được sử dụng lại bởi nhiều Controller khác nhau, giúp giảm bớt sự lặp lại trong mã nguồn.
- **Dễ dàng bảo trì và mở rộng**: Khi cần thay đổi cấu trúc cơ sở dữ liệu hoặc cải thiện hiệu suất truy vấn, bạn chỉ cần chỉnh sửa ở lớp DAO mà không ảnh hưởng đến các phần khác của ứng dụng.
  Trong lập trình, thuật ngữ **"Utility"** thường được dùng để mô tả các lớp hoặc phương thức cung cấp các chức năng hoặc tiện ích chung, hỗ trợ cho các tác vụ cụ thể nhưng không trực tiếp liên quan đến logic nghiệp vụ chính của ứng dụng. Chúng là những "trợ lý" giúp thực hiện các nhiệm vụ chung lặp đi lặp lại hoặc các thao tác phụ trợ trong một ứng dụng.

### Lý do gọi là **"Utility"**:
1. **Cung cấp các hàm chung**: Lớp có từ "Utility" trong tên thường bao gồm các phương thức, công cụ dùng chung trong nhiều phần của ứng dụng. Trong trường hợp này, `Student Database Utility (StudentDAO)` cung cấp các phương thức tiện ích để thực hiện các thao tác cơ sở dữ liệu như kết nối, truy vấn, đóng kết nối,... Những chức năng này được sử dụng lặp đi lặp lại trong các phần khác nhau của ứng dụng.

2. **Tách biệt logic xử lý**: Lớp "Utility" giúp tách biệt logic xử lý cơ sở dữ liệu khỏi phần chính của ứng dụng (chẳng hạn như `Servlet` trong trường hợp này). Điều này giúp mã nguồn trở nên rõ ràng hơn, dễ bảo trì và giảm thiểu sự lặp lại.

3. **Không chứa logic nghiệp vụ phức tạp**: Các lớp "Utility" không phải là phần cốt lõi giải quyết bài toán nghiệp vụ mà chỉ là công cụ hỗ trợ. Ví dụ, `Student Database Utility (StudentDAO)` chỉ thực hiện việc truy vấn và quản lý kết nối với cơ sở dữ liệu mà không chứa logic quản lý sinh viên (như tính toán điểm số hoặc xếp hạng của sinh viên).

### Ví dụ:
Trong `Student Database Utility (StudentDAO)`, chúng ta có các phương thức như `getStudents()` để lấy danh sách sinh viên từ cơ sở dữ liệu. Những phương thức này được sử dụng nhiều lần trong nhiều nơi của ứng dụng, nhưng chúng không phải là logic nghiệp vụ chính. Chúng chỉ cung cấp các tiện ích giúp lấy dữ liệu từ cơ sở dữ liệu một cách dễ dàng và nhất quán.

### Tóm lại:
Các lớp **Utility** như `Student Database Utility (StudentDAO)` được gọi như vậy vì chúng cung cấp các chức năng tiện ích, giúp tối ưu hóa và tái sử dụng code, đồng thời tách biệt các tác vụ phụ trợ khỏi logic chính của ứng dụng, giúp mã nguồn dễ hiểu, dễ bảo trì hơn.
  
