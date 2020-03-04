# Mục đích bài lab
  <img src="https://i.imgur.com/lqQ1krV.png">
  
- Cấu hình network cơ bản
- Sử dụng ip route để ping được từ server 1 sang server 2

| Name | Interface |IP address| 
|--------------|-------|------|
| Server router | ens34 | 192.168.1.1/24 |
|  | ens39 | 192.168.2.1/24 | 
| Server Ubuntu 1 | ens34 | 192.168.1.2 | 
| Server Ubuntu 2 | ens38 | 192.168.2.2 |


## 1. Cấu hình IP trên server và cấu hình cho phép forward gói tin
- Để cấu hình IP tĩnh, ta sửa file cấu hình `/etc/netplan/50-cloud-init.yaml`
  <img src="https://i.imgur.com/6bdBmOr.png">
  
- Sửa file cấu hình như trên, xong ta sử dụng lệnh `netplan apply` để restart lại network

## 2. Cấu hình IP và route trên Ubuntu Server 1:
- Mở file `/etc/netplan/50-cloud-init.yaml`
  <img src="https://i.imgur.com/KXsCw6X.png"

- Thực hiện file cấu hình như trên để chỉnh ip tĩnh và ip route
- Sử dụng `netplan apply` để restart lại network

## 3. Cấu hình IP và route trên Ubuntu server 2:
- Mở file `/etc/netplan/50-cloud-init.yaml`
  <img src="https://i.imgur.com/Hd1DgQN.png"

- Thực hiện file cấu hình như trên để chỉnh ip tĩnh và ip route
- Sử dụng `netplan apply` để restart lại network


