# Trên điện thoại
- Tải app Google Authenticator

# Trên server

- Cài đặt Google Authenticator `sudo apt-get install libpam-google-authenticator`
- Chạy Google Authenticator: `google-authenticator`
- Sau khi chạy chương trình sẽ hiện lên QR, lấy điện thoại quét mã hoặc nhập thủ công trong ứng dụng Google Authenticator trong điện thoại

- Trong folder đó sẽ chứa mã key để nhập thủ công và các code dự phòng
- Sửa file `/etc/pam.d/sshd`
  - Thêm giống như hình
  
    <img src="https://i.imgur.com/xjXreon.png">

- Sau đó mở file `/etc/ssh/sshd_config`,tìm và sửa những dòng sau để dùng public key và OTP:
  - ChallengeResponseAuthentication yes
  - UsePAM yes
  - AuthenticationMethods publickey,keyboard-interactive
  - PasswordAuthentication no
- Restart lại sshd `systemctl restart sshd.service`
- SSH lên server và test thử

  <img src="https://i.imgur.com/B7Lxuto.png">
  
- Mở app Google Authenticator trên điện thoại và nhập vào





