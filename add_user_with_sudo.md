# 1. Yêu cầu: Viết một bash script để add user vào ubuntu, các tham số là username, password. Cho phép sudo không cần password.
# 2. Bash Script
- tạo file `vi create_user_sudo.sh`, nhập nội dung sau:

#!/bin/bash

echo "Nhap username: ";

read USR;

useradd -m -s /bin/bash $USR;

passwd $USR;

echo "$USR ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers




- Lưu lại và phân quyền để sử dụng: `chmod u+x create_user_sudo.sh`
- Để sử dụng, ta chạy file bằng lệnh `./create_user_sudo.sh`

