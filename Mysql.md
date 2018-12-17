### MYSQL

#### 1. Định nghĩa

MySQL là một RDBMS nhanh và dễ dàng để sử dụng. MySQL đang được sử dụng cho nhiều công việc kinh doanh từ lớn tới nhỏ. MySQL được phát triển, được công bố, được hỗ trợ bởi MySQL AB, là một công ty của Thụy Điển. MySQL trở thành khá phổ biến vì nhiều lý do:

- MySQL là mã ngồn mở
- MySQL là một chương trình rất mạnh mẽ.
- MySQL sử dụng một Form chuẩn của ngôn ngữ dữ liệu nổi tiếng là SQL
- MySQL làm việc trên nhiều Hệ điều hành và với nhiều ngôn ngữ như PHP, PERL, C, C++, Java
- MySQL làm việc nhanh và khỏe ngay cả với các tập dữ liệu lớn
- MySQL rất thân thiện với PHP, một ngôn ngữ rất đáng giá để tìm hiểu để phát triển Web

### 1. Storage Engines 
Storage Engines là các thành phần mà xử lý các SQL operations cho các loại table khác nhau. InnoDB là mặc đinh trong MySQL.

Trong MySQL có 2 khái niệm locking khác nhau nhằm phục vụ mục đích transactional read/write: Table Lock & Row Lock
+ Table lock là lock được tạo ra để ngăn các transaction khác can thiệp vào 1 "table" được lock để tránh việc thay đổi dữ liệu trong quá trình 1 câu query đang được thực thi ở 1 table đó.

+ Row lock là lock được tạo ra để ngăn các transaction khác can thiệp vào "row" được lock để tránh việc thay đổi dữ liệu trong quá trình 1 câu query đang diễn ra.

##### 1.1 InnoDB
Đây là Storage Engine mặc định trong MySQL 5.7.
<b>Ưu điểm:</b>
+ (tuân thủ ACID) cho MySQL có các commit, rollback và khả năng khôi phục lỗi để bảo vệ dữ liệu người dùng.
+ Row-level locking của InnoDB và kiểu nonlocking read của Oracle-style làm tăng sự đồng thời và hiệu suất của nhiều người dùng.
+  InnoDB lưu trữ dữ liệu người dùng trong các clustered indexes để giảm I/O cho các truy vấn thông thường dựa trên các primary key. Để duy trì tính toàn vẹn của dữ liệu
+ InnoDB cũng hỗ trợ các ràng buộc toàn vẹn Foreign Key.

<b>Nhược điểm:</b>
+ Hoạt động cần nhiều RAM hơn
##### 1.2 MyISAM
Table-level locking giới hạn hiệu suất read/write dữ liệu, vì vậy nó thường được sử dụng cho các công việc read-only hoặc read-mostly trong các cấu hình Web và lưu trữ dữ liệu.
<b>Ưu điểm:</b>
+ Engine duy nhất hỗ trợ Full Text Search lập chỉ mục toàn văn, cung cấp thuật toán tìm kiếm khá giống Google.
+ Kiến trúc đơn giản nên có tốc độ truy suất (đọc và tìm kiếm) nhanh nhất trong các loại Storage Engine.

<b>Nhược điểm:</b>
+  hoạt động theo cơ chế Table Level Locking, nên khi có hành động thực hiện (thêm/sửa/xóa) 1 bản ghi nào đó trong table thì table đó sẽ bị khóa lại, chờ tới khi hành động này được thực hiện xong thì hành động kia mới tiếp tục được thực hiện. 
+  Kiến trúc đơn giản, không ràng buộc nên loại Storage Engine này rất dễ bị crash

##### 1.3 Memory
Lưu trữ tất cả dữ liệu trong RAM, để truy cập nhanh trong các môi trường đòi hỏi tra cứu nhanh các dữ liệu không quan trọng. Engine này trước đây gọi là HEAP Engine. Storage Engine này đang sử dụng ít dần, do InnoDB với vùng bộ đệm cung cấp một cách mục đích chung và bền để giữ hầu hết hoặc tất cả dữ liệu trong memory, và NDBCLUSTER cung cấp tra cứu giá trị quan trọng nhanh cho các bộ dữ liệu phân tán lớn.

<b>Ưu điểm:</b>
+ tốc độ truy xuất và cập nhật rất nhanh. 
+ Sử dụng làm bảng lưu trữ dữ liệu tạm thời

<b>Nhược điểm:</b>
+ Dễ mất dữ liệu khi mất điện, vấn đề phần cứng, khởi động lại MYSQL.
+ Kích thước bảng phụ thuộc vào cách cấu hình.
+ Chỉ dùng trong mục đích read-only

##### 1.4 CSV
Các bảng của nó thực sự là các tập tin văn bản với các giá trị được phân cách bởi dấu phẩy. Các bảng CSV cho phép bạn nhập hoặc đổ dữ liệu ở định dạng CSV, để trao đổi dữ liệu với các tập lệnh và ứng dụng đọc và ghi cùng một định dạng. Vì bảng CSV không được lập chỉ mục, bạn thường giữ dữ liệu trong các bảng InnoDB trong quá trình hoạt động bình thường và chỉ sử dụng các bảng CSV trong giai đoạn nhập hoặc xuất.

<b>Ưu điểm:</b>
+ Di động cao do dữ liệu lưu trên file

<b>Nhược điểm:</b>
+ Không hỗ trợ lập chỉ mục
+ Không hỗ trợ phân vùng
+ Dữ liệu trên các comlumn không được NOT NULL


##### 1.5 Archive
Các bảng nhỏ gọn, không biểu hiện này được dùng để lưu trữ và truy xuất số lượng lớn các thông tin kiểm tra lịch sử, lưu trữ, hoặc kiểm tra an toàn.

<b>Ưu điểm:</b>

<b>Nhược điểm:</b>

##### 1.6 NDB
Công cụ cơ sở dữ liệu được nhóm lại này đặc biệt phù hợp với các ứng dụng đòi hỏi thời gian hoạt động và tính khả dụng cao nhất có thể.

<b>Ưu điểm:</b>

<b>Nhược điểm:</b>

### 2. Installation

### 3. Data type

### 4. Transaction
<b>Định nghĩa: </b>Transaction là một tiến trình xử lý có xác định điểm đầu và điểm cuối, được chia nhỏ thành các operation (phép thực thi) , tiến trình được thực thi một cách tuần tự và độc lập các operation đó theo nguyên tắc hoặc tất cả đều thành công hoặc một operation thất bại thì toàn bộ tiến trình thất bại. Nếu việc thực thi một operation nào đó bị fail (hỏng) đồng nghĩa với việc dữ liệu phải rollback (trở lại) trạng thái ban đầu.

<b>ACID properties trong transaction</b>

<b>Atomicity :</b> mọi giao dịch chỉ thành công khi tất cả các phần thành công - All or Nothings.
<b>Isolation :</b> các giao dịch thực thi một cách độc lập với nhau.

#### 5. Connector