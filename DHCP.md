# 1. DHCP là gì
- DHCP (Dynamic Host Configuration Protocol) hay còn gọi là giao thức cấu hình động máy chủ. DHCP giúp cấp phát địa chỉ IP một cách tự động, trong đó bao gồm các thông số Subnet Mask và Gateway. Dịch vụ DHCP giúp máy tính trong mạng được cấu hình địa chỉ IP một cách tự động, giảm thiểu thời gian cấu hình thủ công. Dịch vụ này đặc biệt hữu ích trong các hệ thống máy tính có hàng trăm, hàng ngàn máy con.
- DHCP Server có thể là một thiết bị chuyên dụng như Router hoặc là một máy chủ server (Windows, Linux).
# 2. Cài đặt DHCP server
- Cài đặt: `apt-get install isc-dhcp-server`
- Chỉnh ip tĩnh cho `ens34` là `192.168.1.1` trong `/etc/netplan/50-cloud-init.yaml
    <img src="https://i.imgur.com/8KOMUn8.png">

- Ta cần gán Interface sẽ lắng nghe các DHCP request từ máy con trong mạng, ở đây ta sẽ dùng `ens34`: `vi /etc/default/isc-dhcp-server`
    <img src="https://i.imgur.com/vk5yRl3.png">
- Mở file cấu hình DHCP `/etc/dhcp/dhcpd.conf`, file cấu hình DHCP gồm 2 phần cơ bản:
  - Cấu hình toàn cục: quy định những thông tin mặc định cho các khai báo lớp mạng cấp phát ip động
      <img src="https://i.imgur.com/XTzynu3.png">
  
      - option domain-name: khai báo tên miền lớp mạng chung
      - option domain-name-server: khai báo name server của domain-name ở trên
      - default-lease-time: thời gian mặc định một IP DHCP tồn tại được cấp phát
      - max-lease-time: thời gian tối đa một IP DHCP tồn tại được cấp phát cho người dùng
      - lease-file-name: file chứa thông tin địa chỉ IP đã được cấp thông qua DHCP
      - authoritative: set master DHCP server
  - Khai báo mạng cấp phát DHCP
      <img src="https://i.imgur.com/vQz4Xvc.png">
      
      - range: IP bắt đầu và IP kết thúc được cấp phát
      - option subnet-mask: thông tin subnet-mask
      - option routers: thông tin địa chỉ IP của router gateway
      - option broadcast-address: thông tin địa chỉ broadcast
  - Cấp IP cho 1 PC cụ thể:
    <img src="https://i.imgur.com/PQaHx66.png"
    
      -hardware ethernet: địa chỉ MAC của PC cần gán
      -fix-addres: địa chỉ IP cố định

- Có thể có nhiều khai báo mạng cấp phát DHCP
- Sau khi xong ta restart dịch vụ DHCP: `systemctl restart isc-dhcp-server`
- Cuối cùng, ta cấu hình 
