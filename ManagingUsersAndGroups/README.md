# 1. Giới thiệu về Users và Groups
- User dùng để định danh cho một người dùng trong hệ thống. Group là tên định danh cho một nhóm người dùng có cùng một đặc điểm nào đó. Một group có thể có nhiều user và một user có thể là thành viên trong nhiều group.
- Mỗi user đều được gán cho 1 ID (UID), user thường bắt đầu với UID > 1000.
- Mối user đều là thành viên của 1 group trùng tên với user.
Người dùng cũng có một hoặc nhiều ID nhóm (gid), bao gồm một ID mặc định giống với ID người dùng. Những ID này được liên kết với tên thông qua các tập tin `/etc/passwd và /etc/group`.

   <img src="https://i.imgur.com/l2OUG7u.png">

- Mỗi thuộc tính cách nhau bởi dấu `:`
   - Username: Tên đăng nhập user, có phân biệt chữ hoa, thường
   - Password: Password của user đã được mã hoá (chữ x)
   - UserID: Đây là 1 chuỗi số duy nhất được gán cho user, hệ thống sử dụng UID hơn là username để nhận dạng user. User root có UID = 0
   - GroupID: Là 1 chuỗi số duy nhất được gán cho Group đầu tiên mà user này tham gia (thông tin Group có trong file /etc/group)
   - User ID Info: Thông tin về user.
   - Home Directory: Là đường dẫn đầy đủ tới thư mục chủ của user
   - Shell: Đường dẫn của shell mặc định khi user login
# 2. Cách tạo, xóa user và group
- Để tạo 1 user ta sử dụng lệnh `useradd user1`
- Để tạo password cho user, ta dùng lệnh `passwd user1`
- Để xóa 1 user ta dùng lệnh `userdel user1`
- Tạo 1 group ta dùng lệnh `groupadd group1`
- Thêm user có sẵn vào group ta dùng lệnh `usermod -a -G group1 user1`
- Tạo user và thêm 1 user vào group `useradd -a -G group1 user1`
- Xóa 1 group ta dùng lệnh `groupdel group1`

# 3. sudo và su
- su: khởi chaỵ shell mới với tư cách người dùng khác `su - user1`
   - `su - ` : câu lệnh này sẽ chuyển sang người dùng root
   - Bắt buộc người dùng chia sẻ root password với user khác
   - Một mối nguy hiểm cho bảo mật và tính ổn định, có thể dẫn dến lỗi hệ thống khi xóa các tệp quan trọng
- sudo: Khi chạy sudo thì phải nhập thông tin về tài khoản, password của mình để có thể chạy câu lệnh như người dùng root.
## 3.1 Cấu hình sudo
- Khai báo trong `/etc/sudoers` với cú pháp
   - `WHO  WHERE=(as whom) what`
- VD: để user1 chạy được lệnh ifconfig `user1 ALL=(root) /usr/sbin/ifconfig`
# 4. Lệnh chage
- Lệnh `chage` thay đổi thông tin hết hạn mất khẩu người dùng.
  - VD1: sử dụng `chage -l root` để xem thông tin
  
   <img src="https://i.imgur.com/OwDwRhI.png">
   
  - VD2: sử dụng lệnh `chage -E year-month-day user` để thay đổi ngày mật khẩu hết hạn.
  - VD3: sử dụng lệnh `chage -W 3 user` để đưa ra cảnh báo trước khi mật khẩu hết hạn.
  - VD4: sử dụng lệnh `chage -M 5 user` để chỉ định số ngày tối đa và tối thiểu giữa các lần thay đổi mật khẩu
