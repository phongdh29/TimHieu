# 1. Giới thiệu system logging
- Syslog là một giao thức dùng để xử lý các file log Linux. Các file log có thể được lưu tại máy đó hoặc trên Syslog server.
- Dùng lệnh `tail -f` để đọc log liên tục
- Đặc điểm:
  - Syslog dùng port 514
  - Dữ liệu được gửi dưới dạng text không mã hóa.
  
# 2. Cấu hình System logging-
- File cấu hình :
  - Centos: /etc/rsyslog.conf
  - Ubuntu: /etc/rsyslog.d/50-default.conf
  
   <img src="https://i.imgur.com/h4se6v4.png">

- File cấu hình cho thấy nơi lưu log của các service trong hệ thống, bên trái là nguồn tạo log - bên phải là nơi lưu log
   
   <img src="https://i.imgur.com/5chBLWw.png">
   
  |Nguồn tạo log| Ý Nghĩa|
  |-|-|
  |kernel|	Những log mà do kernel sinh ra|
  |user|	Log ghi lại cấp độ người dùng|
  |mail|	Log của hệ thống mail|
  |daemon|	Log của các tiến trình trên hệ thống|
  |auth|	Log từ quá trình đăng nhập hệ hoặc xác thực hệ thống|
  |syslog|	Log từ chương trình syslogd|
  |lpr|	Log từ quá trình in ấn|
  |news|	Thông tin từ hệ thống|
  |cron|	Clock deamon|
  |authpriv|	Quá trình đăng nhập hoặc xác thực hệ thống|
  |security|	Kiểm tra đăng nhập|
  |console|	Log cảnh báo hệ thống|
  |local 0 -local 7|	Log dự trữ cho sử dụng nội bộ|

  |Mức cảnh báo| Ý nghĩa|
  |-|-|
  |emerg|	Thông báo tình trạng khẩn cấp|
  |alert|	Hệ thống cần can thiệp ngay|
  |crit|	Tình trạng nguy kịch|
  |error|	Thông báo lỗi đối với hệ thống|
  |warn|	Mức cảnh báo đối với hệ thống|
  |notice|	Chú ý đối với hệ thống|
  |info|	Thông tin của hệ thống|
  |debug|	Quá trình kiểm tra hệ thống|

- Các bản tin log trong hệ thống là vô cùng nhiều. Vì vậy để thuận tiện cho việc phân tích mức độ quan trọng của log, mỗi dòng log đều được gắn một mã cảnh báo, tương ứng mức độ quan trọng của dòng log đó.

- Tùy chỉnh lưu log với các mức độ: 
  - Lưu log với mức độ cảnh báo là INFO: `mail.=info     /var/log/mail`
  - Lưu log từ mức INFO trở lên: `mail.info							/var/log/mail`
  - Lưu log với tất cả mức độ: `mail.*                  /var/log/mail`
  - Lưu tất cả log ngoài mức INFO `mail.!info           /var/log/mail`
  
# 3. Rotating log
- Để ngăn cản những files này ngày càng trở nên nặng và khó kiểm soát, ta nên sử dụng rotate log. Hệ thống đưa ra các lệnh để thiết lập những log files mới, những file log cũ sẽ thêm một số ở cuối tên file
- File cấu hình `/etc/logrotate.d`
   <img src="https://i.imgur.com/nFsjUjS.png">
   
   - Hệ thống sẽ quay vòng log files hàng tuần
   - Lưu lại những thông tin logs trong 4 tuần
   
# 4. Syslog server
- Để chuyển một máy thành máy chủ log thì phải mở cổng 514
  - `iptables -A INPUT -p udp --dport 514 -j ACCEPT`
  - `iptables -A OUTPUT -p udp --sport 514 -j ACCEPT`
  
- Sửa file cấu hình `/etc/rsyslog.conf`

   <img src="https://i.imgur.com/478zHOH.png">
   
- Với client thì sửa file cấu hình log như sau:
   `mail.*      @IP của syslogserver`
   
  
  

 


