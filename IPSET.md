B1: Tìm địa chỉ IP thuộc VN
- Tìm trên mạng, hoặc dùng file geolite2

 <img src="http://i.imgur.com/S7Yn7v7.png">
- Tìm địa chỉ IP của VN:

  - Tìm ID VN: 
  
    `grep Vietnam GeoLite2-Country-Locations-en.csv`
  
  - Tìm và lọc list IP thuộc VN và in ra file:
  
    `grep 1562822 GeoLite2-Country-Blocks-IPv4.csv | cut -d"," -f1 > IPv4_VN`

B2: Sử dụng IPTABLES và IPSET
- IPSET là công cụ hữu ích quản lý địa chỉ ip. IP sets có thể lưu trữ IP address, networks, (tcp/udp) port numbers, MAC address, interface name hoặc kết hợp những cái trên.
- `apt-get install ipset`
- Tạo set để lưu trữ network: `ipset create allow_ip hash:net`
- Thêm IP từ file IPv4 vào set allow_ip, tạo 1 file script add_ip.sh

	<img src="https://i.imgur.com/OcFc30K.png">
- Sử dụng lệnh `chmod u+x add_ip.sh` để có thể chạy script
- Chạy script `sh add_ip.sh`
- Kiểm tra IP trong set allow_ip: `ipset list allow_ip`
- Chặn mọi kết nối từ bên ngoài, ngoại trừ IP VN có trong set allow_ip: 

  `iptables -A INPUT -m set ! --match-set allow_ip src -j DROP`
