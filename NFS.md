# 1. NFS là gì:
- NFS là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý.
- Sử dụng TCP và UDP để truy cập và phân phối dữ liệu tùy thuộc vào phiên bản được sử dụng. Cơ chế hệ thống tệp cho phép lưu trữ và truy xuất dữ liệu từ nhiều đĩa và thư mục.
- Để truy cập dữ liệu được lưu trữ trên 1 máy chủ, server sẽ triển khai NFS để cung cấp dữ liệu cho khách hàng. Quản trị viên máy chủ xác định những gì cần cung cấp và đảm bảo có thể nhận ra các máy khách được xác nhận. Từ client, yêu cầu quyền truy cập vào dữ liệu đã xuất, bằng cách sử dụng lệnh `mount`
# 2. Ưu , nhược điểm
- Ưu điểm:
  - NFS là 1 giải pháp chi phí thấp để chia sẻ tệp mạng.
  - Dễ cài đặt vì nó sử dụng cơ sở hạ tầng IP hiện có
  - Cho phép quản lý trung tâm, giảm nhu cầu thêm phần mềm cũ và dụng lượng đĩa trên các hệ thống người dùng cá nhân
- Nhược điểm:
  - NFS vốn không an toàn, chỉ nên sử dụng trên 1 mạng đáng tin cậy sau Firewall
  - NFS bị chậm trong khi lưu lượng mạng lớn
  - Client và server tin tưởng lần nhau vô điều kiện
# 3. Cài đặt
## 3.1 Trên server
- Cài đặt `apt install nfs-kernel-server`
- Tạo thư mục để share file: `mkdir /mnt/share`
- Thêm vào cuối file file `/etc/exports`
  - Chỉ dùng cho 1 IP cụ thể: ` /thu_muc_share clientIP(option1,option2,...)`
  - Dùng cho 1 dải IP: ` /thu_muc_share subnetIP/24(option1,option2,...)
  
    <img src="https://i.imgur.com/3pfEZng.png">
    
  - Các option :
   
    <img src="https://i.imgur.com/qME2tAw.png">
    
 
 - Chạy lệnh `exportfs -a` để bắt nfs cập nhật lại nội dung file `/etc/exports`
 
 - Cấu hình iptables:
 
    `iptables -A INPUT -s 192.168.1.0/24 -d 192.168.1.0/24 -p udp -m multiport --dports 111,2049 -m state --state NEW,ESTABLISHED -j ACCEPT`
    
   `iptables -A INPUT -s 192.168.1.0/24 -d 192.168.1.0/24 -p tcp -m multiport --dports 111,2049 -m state --state NEW,ESTABLISHED -j ACCEPT`
   
   `iptables -A OUTPUT -s 192.168.1.0/24 -d 192.168.1.0/24 -p udp -m multiport --sports 111,2049 -m state --state ESTABLISHED -j ACCEPT`
   
   `iptables -A OUTPUT -s 192.168.1.0/24 -d 192.168.1.0/24 -p tcp -m multiport --sports 111,2049 -m state --state ESTABLISHED -j ACCEPT`
   
- Kiểm tra mount point trên server `showmount -e`

- Khởi động lại các dịch vụ liên quan: `systemctl restart rpcbind`
  
 
## 3.2 Trên client
- Cài đặt `apt-get install nfs-common`
- Tạo thư mục để gắn mount: `mkdir /mnt/shared_client`
- Mount thư mục trên NFS server `mount serverIP:/mnt/shared /mnt/shared_client` hoặc sửa file `/etc/fstab` để tự động mount khi reboot:

    <img src="https://i.imgur.com/Q8R8ZiR.png">
    
  
- Sử dụng `df -h` để kiểm tra

    <img src="https://i.imgur.com/hXQNsq5.png">
    
    
