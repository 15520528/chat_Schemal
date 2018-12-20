
### Mục lục
[I. Yêu cầu ứng dụng](#I.-Yêu-cầu-ứng-dụng)
[II. Thiết kế cơ sở dữ liệu cho ứng dụng](#II.-Thiết-kế-cơ-sở-dữ-liệu-cho-ứng-dụng)
[III. Truy vấn dữ liệu](#Truy-vấn-dữ-liệu)

### I. Yêu cầu ứng dụng
Thiết kế schema cho chương trình chat - trò chuyện (với Redis và với MySQL), sử dụng Python để tương tác.

Mô tả chương trình chat: cơ chế tương tự Zalo, Message. 
### 1.1. Cơ bản:

- Tạo tài khoản (username/password, email,...)
- Chọn người trò chuyện (theo username hoặc email)
- Hiển thị lịch sử trò chuyện (nếu có)
- Trò chuyện (chat)

### 1.2. Nâng cao:

- Kết bạn
- Chat trong nhóm
- Hiển thị trạng thái online/offline của người khác
- Hiện thị trạng thái tin nhắn (seen)


### 1.3. Hướng dẫn 

- Thiết kế hệ thống tài khoản 
- Thiết kế cấu trúc lưu trữ lịch sử chat
- Thiết kế cấu trúc lưu trữ nội dung tin nhắn 

### II. Thiết kế cơ sở dữ liệu cho ứng dụng
#### 2.1 Mô hình cơ sở dữ liệu

Bảng Users
Mô tả: chứa thuộc tính của người dùng

| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| user_id    | int(11)          |    Mã user    |
| user_name  | varchar(25)  | Tên đăng nhập |
| password   | varchar(40)  | Mật khẩu      |
| email      | varchar(25)  | Địa chỉ mail  | 
| full_name  | varchar(55)  | Tên đầy đủ    |
| phone      | varchar(11)  | Số điện thoại |
| gender     | varchar(1)   | Giới tính     |
| birthday   | Date         | Ngày sinh     |
| photo_id| varchar(255) | Mã ảnh đại diện  |
| album_id | varchar(255) | Mã album |
| is_online  | boolean      | Trạng thái online |
| is_blocked | boolean      | Trạng thái block |
| created_at | DateTime     | Thời điểm tạo |

Bảng Photos
Mô tả: Chứa thông tin về ảnh đại diện của user
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| photo_id   | int(11)      | Mã photo |
| owner_id   | int(11)      | Mã user sở hữu id |
| link       | varchar(55)  | Đường dẫ đến ảnh |
| cation     | varchar(255) | Phụ đề cho ảnh   |
| created_at | DateTime     | Thời điểm tạo    |
| is_deleted | boolean      | Bị xóa hay chưa  |
| type       | enum         | Loại ảnh         |

Bảng News
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| news_id    | int(11)      | Mã bài viết   |
| writer_id  | int(11)      | Mã người viết |
| react_id   | int(11)      | Mã phản hồi   |
| content    | text         | Nội dung      |
| is_deleted | boolean      | Tình trạng xóa|
| created_at | DateTime     | Thời điểm được đăng |

Bảng EmotionReactions
Mô tả: chứa thông tin về phản hồi về ảnh của người dùng 
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| news_id   | int(11)      | Mã bảng tin    |
| react_user | int(11)      | Mã người phả hồi |
| type       | enum         | Loại phản hồi |
| updated_at | DateTime     | Thời điểm cập nhật |

Bảng CommentsReactions
Mô tả: chứa thông tin về phản hồi về ảnh của người dùng 
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| comments_id    | int(11)     | Mã comment    |
| news_id   | int(11)     | Mã photo       |
| sender_id  | int(11)     | Mã người bình luận   |
| parentComment_id | int(11) | Mã comment cha |
| content    | text  | Nội dung | |
| sticker_id    | int(11)       | Mã sticker được dùng |
| image_link | varchar(55)   | Đường dẫn tới ảnh |
| is_deleted | boolean       | Tình trạng bị xóa |
| created_at | DatimeTime    | Thời điểm được gửi |

Bảng UserStatus
Mô tả: Chứa thông tin trạng thái được thay đổi bởi người dùng
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| status_id  | int(11)      | Mã trạng thái |
| user_id     | int(11)      | Mã user       |
| Update_time | DateTime    | Thời điểm cập nhật|
| Description | varchar(50) | Mô tả cập nhật |

 
Bảng FriendRelationship
Mô tả: chứa thuộc tính của người dùng
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| user1_id   | int(11)      | Mã người dùng 1|
| user2_id   | int(11)      | Mã người dùng 2|
| type       | enum         | Mối quan hệ    |
| areFriends | boolean      | Có phải bạn bè  hay không |

Bảng Conversations
Mô tả: chứa thông về cuộc trò chuyện
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| conversation_id | int(11) | Mã cuộc trò chuyện |
| title           | varchar(55) | Tên cuộc trò chuyện |
| created_at      | DateTime    | Thời gian tạo cuộc trò chuyện |
| type            | enum        | Loại trò chuyện |
| creator_id      | int(11)     | Id user tạo cuộc trò chuyện |
| deleted_at      | DateTime    | Thời điểm xóa cuộc trò chuyện |

Bảng Participants
Mô tả: bảng chứa thông tin chi tiết về những người tham gia cuộc trò chuyện
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| participant_id | int(11) | Mã participant|
| conversation_id | int(11)| Mã cuộc trò chuyện |
| user_id         | int(11)| Mã user tham gia |
| nick_name       | varchar(55) | Biệt danh người tham gia |
| is_leave        | boolean     | Đã rời nhóm hay chưa? |
| lass_accessed   | DateTime    | Lần truy cập cuối |

Bảng Messages
Mô tả: bảng chứa thông tin về nội dung thông điệp mà user gửi trong messages
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| message_id | int(11)      | Mã thông điệp |
| sender_id  | int(11)      | Id người gửi  |
| message    | varchar(255)         | Thông điệp    |
| attached_links | varchar(255) | Đường dẫn được đính kèm |
| attached_images| varchar(255) | Đường dẫn hình ảnh được đính kèm | 
| created_at | DateTime     | Thời điểm tạo |
| type       | enum         | Loại message  |       
| sticker_id | int(11) | id của sticker được thêm vào |
| is_retrived | boolean     | Tình trạng bị thu hồi |
| created_at | DateTime     | Thời điểm được gửi |  
| retrieved_at | DatTime    | Thời điểm thu hồi |

Bảng MessagesMark
Mô tả: Bảng chứa các thông điệp được đánh dấu bởi người dùng
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| user_id    | int(11)      | Mã người dùng |
| message_id | int(11)      | Mã thông điệp |

Bảng stickers
Mô tả: chứa thông tin về  stickers 
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| stickers_id    | int(11)      | Mã icon       |
| content    | varbinary  | Nội dung icon      |
| groupSticker_id   | int(11)      | Mã nhóm icon  |
| created_at | DateTime     | Thời điểm được thêm|

Bảng groupSticker_id
Mô tả: chứa thông tin về  nhóm
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| groupSticker_id   | int(11)      | Mã nhóm icon  |
| name      | varchar()     | Tên nhóm icon |
| created_at | DateTime     | Thời điểm được tạo|

Bảng Deleted_Conversations
Mô tả: chưa thông tin những cuộc trò chuyện đã xóa
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| deleted_conversation_id| int(11) | Mã khóa chính |
| conversation_id | int(11) | Mã cuộc trò chuyện được xóa |

Bảng Sessions
Mô tả: Lưu trữ phiên truy cập của người dùng
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| session_id | int(11) | Mã session |
| user_id    | int(11) | Mã người dùng |
| device_id  | int(11) | Mã thiết bị truy cập |
| country    | varchar(50)| Quốc gia |
| city       | varchar(50)| Thành phố |
| street     | varchar(50)| đường     |
| accessed_at| DateTime | Thời điểm truy cập |

Bảng devices
Mô tả: Lưu trữ thông tin thiết bị
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:|
| device_id  | int(11)      | Mã thiết bị |
| device_name  | varchar(255)| Tên thiết bị |
| origin       | enum       | Nhà sản xuất  |

Bảng ErrorReport
Mô tả: Lưu trữ thông tin phản hồi của người dùng
| **Thuộc tính** | **Kiểu dữ liệu** |   **Diễn giải**   |
|:----------:|--------------|:-------------:| 
| report_id  | int(11)      | Mã phản hồi   |
| type       | enum         | Loại phản hồi |
| sender_id  | int(11)      | Mã người gửi  |
| message    | varchar(500) | Thông điệp    | 

#### 2.2 Mô tả dữ liệu các bảng


### III. Truy vấn dữ liệu