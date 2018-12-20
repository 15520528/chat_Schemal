### SQL

#### 1. Định nghĩa
SQL (Structured Query Language) ngôn ngữ truy vấn mang tính cấu trúc, là một loại ngôn ngữ máy tính phổ biến để tạo, sửa, và lấy dữ liệu từ một hệ quản trị cơ sở dữ liệu quan hệ. 

Năm 1974 : Sql Structured Query Language  ra đời
#### 2. Chức năng của Sql là gì ?
Sql có thể truy vấn Database theo nhiều cách khác nhau, bởi sử dụng các lệnh.
Sql người dùng có thể truy cập dữ liệu từ RDBMS.
Sql cho phép người dùng miêu tả dữ liệu.
Sql cho phép người dùng định nghĩa dữ liệu trong một Database và thao tác nó khi cần thiết.
Cho phép người dùng tạo, xóa Database và bảng.
Cho phép người dùng tạo view, Procedure, hàm trong một Database.
Cho phép người dùng thiết lập quyền truy cập vào bảng, thủ tục và view.

<b>DDL (Data Definition Language) – Ngôn ngữ định nghĩa dữ liệu</b>
Lệnh CREATE: Tạo một bảng, một View của bảng, hoặc đối tượng khác trong Database.

Lệnh ALTER: Sửa đổi một đối tượng Database đang tồn tại, ví dụ như một bảng.

Lệnh: Xóa toàn bộ một bảng, một View của bảng hoặc đối tượng khác trong một Database.

<b>DML (Data Manipulation Language) – Ngôn ngữ thao tác dữ liệu</b>
Lệnh SELECT: Lấy các bản ghi cụ thể từ một hoặc nhiều bảng.

Lệnh INSERT: Tạo một bản ghi.

Lệnh UPDATE: Sửa đổi các bản ghi.

Lệnh DELETE: Xóa các bản ghi.

#### 3. Toàn vẹn dữ liệu trong SQL
Toàn vẹn dữ liệu (Data Integrity) là việc đặt ra các quy tắc trong một cơ sở dữ liệu nhằm kiểm tra các giá trị của dữ liệu trước khi được lưu trữ phải đảm bảo tính chính xác và hợp lý bên trong một cơ sở dữ liệu. Nếu các giá trị dữ liệu nào vi phạm các quy tắc đặt ra thì các dữ liệu đó sẽ không được lưu vào table.