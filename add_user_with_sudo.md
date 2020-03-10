# 1. Yêu cầu: Viết một bash script để add user vào ubuntu, các tham số là username, password. Cho phép sudo không cần password.
# 2. Bash Script
- tạo file `vi create_user_sudo.sh`, nhập nội dung sau:

        #!/bin/bash

        echo "Nhap username: "

        read USR

        getent passwd $USR > /dev/null

        if [ $? -ne 0 ]; then #Kiem tra user, echo $? la trang thai cua lenh cuoi cung, khac 0 la error => khac 0 la chua co user

        useradd -m -s /bin/bash $USR
        
        passwd $USR
        
        check_pass=$(getent shadow | grep "$USR" |cut -d":" -f"2") #Check pass trong file shadow
        
                if [ $check_pass != "!" ]; then #Neu truong vua cat khac "!" => nhap 2 lan pass

                        cp /etc/sudoers /tmp/sudoers
                        
                        echo "$USR      ALL=(ALL) NOPASSWD:ALL" >> /tmp/sudoers
                        
                        visudo -cf /tmp/sudoers #Kiem tra file sudo dung syntax khong
                        
                        if [ $? -eq 0 ]; then
                        
                                mv /tmp/sudoers /etc/sudoers

                        fi
                        
                        echo "Da tao thanh cong $USR co quyen sudo"
                        
                else
                
                        userdel -r $USR
                        
                        echo "Password khong giong nhau, tao lai user"
                        
                fi
                
        else



- Lưu lại và phân quyền để sử dụng: `chmod u+x create_user_sudo.sh`
- Để sử dụng, ta chạy file bằng lệnh `./create_user_sudo.sh`

