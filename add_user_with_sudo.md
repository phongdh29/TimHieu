# 1. Yêu cầu: Viết một bash script để add user vào ubuntu, các tham số là username, password. Cho phép sudo.
# 2. Bash Script
- tạo file `vi create_user_sudo.sh`, nhập nội dung sau:

#!/bin/bash

echo "Nhap username: ";

read USR;

useradd -m -s /bin/bash $USR;

passwd $USR;

echo "Tao xong user $USR";

usermod -aG sudo $USR;

echo "Da them sudo cho $USR";


- Lưu lại và phân quyền để sử dụng: `chmod u+x create_user_sudo.sh`
- Để sử dụng, ta chạy file bằng lệnh `./create_user_sudo.sh`

# 3. Sudo không cần password
- Mở file `/etc/sudoers`
- Tìm và sửa dòng sau: 

    <img src="https://i.imgur.com/5JKBCDu.png">
    
  
