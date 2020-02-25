# 1. Iptables
- Iptables là một hệ thống tường lửa (Firewall) tiêu chuẩn được cấu hình, tích hợp mặc định trong hầu hết các bản phân phối của hệ điều hành Linux (CentOS, Ubuntu…). Iptables hoạt động dựa trên việc phân loại và thực thi các package ra/vào theo các quy tắc được thiết lập từ trước.
- Iptables thường được cài đặt mặc định trong hệ thống. Nếu chưa được cài đặt:
  - Centos: `#yum install iptables`
  - Ubuntu: `#apt-get install iptables`
- Tắt `Firewalld` trên Centos và `ufw` trên Ubuntu
  - Centos: `#systemctl stop firewalld`
            `#systemctl disable firewalld`
  - Ubuntu: `#ufw disable`

- Cơ chế hoạt động của iptables gồm 3 phần: `tables`,`chain`,`targets`

# 2. Table, Chain, Targets
## 2.1. Filter Table
- Table filter là một trong những table được dùng nhiều nhất trong iptables. Table quyết định có cho phép gói tin được chuyển đến địa chỉ đích hay không. Filter gồm có 3 chain:
  - INPUT: Các gói tin đến firewall.
  - OUTPUT: Các gói tin đi ra khỏi firewall
  - FORWARD: Các gói tin được định tuyến đi qua máy chủ
## 2.2. NAT table
- Cho phép route các gói tin đến các host khác nhau trong mạng bằng cách thay đổi IP nguồn và IP đích của gói tin. Table này quy định và cho phép các kết nối có thể truy cập tới các dịch vụ không được truy cập trực tiếp. NAT table gồm 3 chain:
  - PREROUTING: Thay đổi gói tin trước khi định tuyến, điều này có nghĩa là việc dịch gói tin sẽ xảy ra ngay lập tức sau khi gói tin đến hệ thống. Điều này thực hiện thay đổi địa chỉ IP đích thành một địa chỉ nào đó sao cho phù hợp với việc định tuyến trên máy chủ cục bộ - DNAT.
  - POSTROUTING: Thay đổi gói tin sau khi định tuyến, điều này có nghĩa là dịch gói tin khi gói tin ra khỏi hệ thống. Điều này thực hiện thay đổi địa chỉ IP nguồn của gói tin thành một địa chỉ nào đó phù hợp với việc định tuyến trên máy chủ đích - SNAT.
  - OUTPUT: thực hiện NAT cho các gói tin được thực hiện cục bộ trên firewall.
  
## 2.3. MANGLE Table
- Table này liên quan đến việc sửa header của gói tin, ví dụ giá trị TTL
## 2.4. RAW Table
- Iptables là một stateful firewall, điều đó có nghĩa là các gói được kiểm tra liên quan đến trạng thái(state) của nó (Ví dụ: gói có thể là một phần của kết nối mới hoặc có thể là một phần của kết nối hiện có.). Table raw cho phép bạn làm việc với các gói trước khi kernel bắt đầu theo dõi trạng thái của nó, nó cũng giúp loại trừ một số gói tin ra khỏi việc tracking vì ví đề hiệu năng hệ thống.
## 2.5 Security Table
-  Nó được dùng bởi SELinux để thiết lập các chính sách bảo mật.

# 3. Các quy tắc áp dụng Iptables
- Liệt kê toàn bộ quy tắc `#iptables -nvL`

    <img src="https://i.imgur.com/VGjMXHv.png">
    
- Target: hành động áp dụng cho mỗi quy tắc
  - ACCEPT: gói dữ liệu được chuyển tiếp
  - DROP: gói dữ liệu bị chặn
  - REJECT: gói dữ liệu bị chặn, đồng thời gửi thông báo tới người gửi
- Prot: quy định các giao thức sẽ được áp dụng để thực thi quy tắc, bao gồm all, TCP hay UDP
- IN: chỉ ra rule sẽ áp dụng cho các gói tin đi vào từ interface nào, chẳng hạn như lo, eth0, eth1 hoặc any là áp dụng cho tất cả interface.
- OUT: chỉ ra rule sẽ áp dụng cho các gói tin đi ra từ interface nào.
- DESTINATION: Địa chỉ của lượt truy cập được phép áp dụng quy tắc.

# 4. Sử dụng iptables để chặn/mở port hoặc IP
- Mở port: `#iptables -A INPUT -p tcp/udp --dport xxx -j ACCEPT`
- Chặn port: `#iptables -A INPUT -p tcp/udp --dport xxx -j DROP`
- Chặn IP đến : `iptables -A INPUT -p tcp -s xxx.xxx.xxx.xxx -j DROP`
- Chặn đi tới IP: `iptables -A OUTPUT -p tcp -d xxx.xxx.xxx.xxx -j DROP`  
- Cho phép 1 IP cụ thể đi qua port : `iptables -A INPUT -p tcp -s xxx.xxx.xxx.xxx --dport xx -j ACCEPT`
- Chặn IP cụ thể đi qua port: `iptables -A INPUT -p tcp -s xxx.xxx.xxx.xxx --dport xx -j DROP`


# 5. Lưu cấu hình Iptables
- Sau khi cấu hình xong Iptables, ta cần lưu lại
  - Thực hiện những quy tắc trong trường hợp khởi động lại `#iptables-save >/etc/sysconfig/iptables`
  - export file `iptables-save > tenfile`
  - import file `iptables-save < tenfile`
  - Lưu file `service iptables save`
  
  
