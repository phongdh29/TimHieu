# 1. File system
- File system được dùng để quản lý cách dữ liệu được đọc và lưu trên thiết bị. File system cho phép người dùng truy cập nhanh chóng và an toàn khi cần thiết.
- Filesystem của hệ điều hành Linux được tổ chức theo tiêu chuẩn cấp bậc của hệ thống tập tin Filesystem Hierarchy Standard ( FHS ). Tiêu chuẩn này định nghĩa mục đích của mỗi thư mục.
   <img src="https://i.imgur.com/KK5NcDS.png">
  - `/` - Root: Đây là nơi bắt đầu của tất cả các file và thư mục. Chỉ có root user mới có quyền ghi trong thư mục này. Thư mục home của root user là `/root`
  - `/bin`: Thư mục này chứa các chương trình thực thi. Các chương trình chung của Linux được sử dụng bởi tất cả người dùng được lưu ở đây. Ví dụ như lệnh: ps, ls, ping...
  - `/sbin`: Cũng giống như /bin, /sbin cũng chứa các chương trình thực thi, nhưng chúng là những chương trình của admin, dành cho việc bảo trì hệ thống. Ví dụ như: reboot, fdisk, iptables...
  - `/etc`: Thư mục này chứa các file cấu hình của các chương trình
  - `/dev`: Các phân vùng ổ cứng, thiết bị ngoại vi như USB, ổ đĩa cắm ngoài, hay bất cứ thiết bị nào gắn kèm vào hệ thống đều được lưu ở đây.
  - `/tmp`: Thư mục này chứa các file tạm thời được tạo bởi hệ thống và các người dùng. Các file lưu trong thư mục này sẽ bị xóa khi hệ thống khởi động lại.
  - `/proc`: Thông tin về các tiến trình đang chạy sẽ được lưu trong /proc dưới dạng một hệ thống file thư mục mô phỏng.
  - `/var`: Thông tin về các biến của hệ thống được lưu trong thư mục này, vd: thông tin về log file: /var/log
  - `/home`: Thư mục này chứa tất cả các file cá nhân của từng người dùng. Ví dụ: /home/phongdh

  - `/boot`: Tất cả các file yêu cầu khi khởi động như initrd, vmlinux. grub được lưu tại đây
  - `/lib`:Chứa cá thư viện hỗ trợ cho các file thực thi trong /bin và /sbin.
  - `/opt`: Tên thư mục này nghĩa là optional (tùy chọn), nó chứa các ứng dụng thêm vào từ các nhà cung cấp độc lập khác. Các ứng dụng này có thể được cài ở /opt hoặc một thư mục con của /opt
  - `/mnt`: Thư mục tạm để mount các file hệ thống
  - `/srv:`Chứa dữ liệu liên quan đến các dịch vụ khacs
- Linux dùng ký tự `/` để tách các đường dẫn


# 2. Mount và Umount
- Tất cả các phân vùng được gắn vào hệ thống thông qua một Mount Point. Nó sẽ xác định vị trí của một tập dữ liệu trong hệ thống tệp.
- Lệnh `mount` được sử dụng để đính kèm một file system trong cây hệ thống tập tin.
  - VD: `mount /dev/sdb2 /mnt`
- Lệnh `umount` sử dụng để ngắt kết nối khỏi hệ thống, nếu rút thiết bị khỏi máy tính mà không umount thì dữ liệu có thể bị lỗi
  - VD: `umount /dev/sdb2`

- Khi reboot lại máy thì phân vùng đã mount sẽ bị mất, ta phải khai báo cấu hình mount tự động trong file cấu hình `/etc/fstab`

    <img src="https://i.imgur.com/mQCfHQP.png">

- VD: mount thiết bị /dev/sdb2 vào thư mục /data, ta sẽ thêm câu lệnh sau vào file `etc/fstab`:
  - `/dev/sdb2 /data ext4  defaults  0 0`
    - `dev/sdb2`: Thiết bị cần mount
    - `/data`: Nơi mặc định để mount
    - `ext4`: Loại file system (ext3,ext4)
    - `defaults`: Các tuỳ chọn mount, mount tự động lúc khởi động, user có quyền mount, có cho phép thực thi các file trên đó không.
    - `0`: Thông số tuỳ chọn cho dump. Dump sẽ dựa vào con số cấu hình để biết phải làm gì, nếu nó là 0 thì dump sẽ bỏ qua, hầu hết các trường hợp thông số này đều bằng 0
    - `0`: Thông số tuỳ chọn cho lệnh fsck. Nó quy định thứ tự để kiểm tra file system, nếu thông số này bằng 0 đồng nghĩa với việc fsck sẽ không kiểm tra
    
# 3. File permission
- Sử dụng lệnh `ls -l` để kiểm tra quyền file
    <img src="https://i.imgur.com/VuyPSjL.png">

- Ký tự đầu tiên trong dãy 10 ký tự thể hiện file types.
  - `–` : dấu gạch này thể hiện đó là một file bình thường. Có thể là file chứa dữ liệu, text, hoặc file thực thi binary.
  - `d` : Chữ d thể hiện đó là một thư mục (Directory).
  - `l` : Chữ L thể hiện đó là một Link (tưởng tượng nó như cái shortcut trong Windows vậy).
  - `c` : Chữ c thể hiện Character devices (kiểu thiết bị như bàn phím, chuột, card màn hình, card âm thanh…).
  - `b` : Chữ b thể hiện Block devices (dạng thiết bị lưu dữ liệu như ổ cứng, usb…).
  - `s` : Chữ s thể hiện Socket – một loại file đặc biệt dùng cho việc trao đổi thông tin giữa các process.

- Ký tự thứ 2,3,4 mô tả quyền của user sở hữu file đó
- Ký tự thứ 5,6,7 mô tả quyền của group sở hữu file đó
- Ký tự thứ 8,9,10 mô tả quyền của user khác trên file đó
## 3.1 Quyền trên file
- Dấu `-` là không có quyền
- r: quyền đọc (giá trị 4)
- w: quyền ghi (giá trị 2)
- x: quyền thực thi (giá trị 1)
- Để thay đổi quyền sở hữu ta dùng lệnh `chown`. VD: `chown user:group /tenfile`
- Để thay đổi quyền file ta dùng lệnh `chmod`
  -VD: `chmod 760 /tenfile`: user có giá 7(đọc, ghi, thực thi), group có giá 6(đọc, ghi), others có giá trị 0(không có quyền gì)
  - `chmod u=rwx,g=wr,o= /tenfile`: user có full quyền (= giá trị 7), group có quyền đọc và ghi (tương đương giá trị 6), others không có quyền gì ( ương đương giá trị 0)
  - `chmod a+x /tenfile`: `a` là all(user,group,others), dấu `+` là thêm quyền, dấu `-` là bớt quyền, `x`: là quyền thực thi
 
## 3.2 Umask
- Dùng Umask để thay đổi quyền mặc định của file/folder
  - Quyền mặc định của file khi được tạo ra là: 644 (rw-r--r--)
  - Quyền mặc định của folder khi được tạo ra là: 755 (rwxr-xr-x)
- Umask mặc định là 022, để sửa umask ta dùng lệnh `umask xxx`

