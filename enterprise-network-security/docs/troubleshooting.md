# Nhật ký xử lý sự cố (theo kết quả đã quan sát)

## 1. PC không nhận DHCP (`DDD`, `Can't find dhcp server`)
- DHCP-SERVER `10.10.50.10` có pool, route qua `10.10.50.1`, ping gateway 5/5.
- DIS1 ping được DHCP-SERVER qua OSPF; có `ip helper-address 10.10.50.10`.
- Tuy nhiên SVI VLAN10–40 trên DIS1 ở trạng thái **down/down**; `show interfaces trunk` rỗng và STP VLAN10 chưa có instance.
- Nguyên nhân phát hiện: các uplink/Po1 chưa hoạt động đúng trunk 802.1Q. Hướng sửa: `switchport trunk encapsulation dot1q` trước `switchport mode trunk`, cho phép VLAN tương ứng và cấu hình hai đầu.
- **Chưa có kết quả lease của VPCS sau khi sửa**, nên không đánh dấu DHCP đã hoàn tất.

## 2. Ping từ WAN vào DMZ bị timeout
- VPCS WAN `192.168.16.147` ban đầu dùng gateway VMware NAT `192.168.16.2`, không phải FortiGate `192.168.16.134`.
- Khi thử ICMP định tuyến, đã tạm chuyển gateway sang FortiGate và dùng policy PING; đó **không phải** phép thử HTTP VIP.
- WEB-SERVER ping FortiGate DMZ `10.30.0.9` đạt 5/5.

## 3. HTTP VIP trả 401
- VIP HTTP chuyển `192.168.16.134:80` đến `10.30.0.10:80`.
- WEB-SERVER bật `ip http server` và `ip http authentication local`.
- Windows `curl` nhận HTTP 401 và header `Server: cisco-IOS`, chứng minh request đến ứng dụng và đang yêu cầu xác thực; không phải NAT bị hỏng.
- Sau điều chỉnh quyền tài khoản lab, trình duyệt hiển thị trang Cisco WEB-SERVER.
- HTTP này là giao diện quản trị, không xuất bản trên Internet thật.

## 4. DNS VIP không phản hồi
- DNS-SERVER `10.30.0.11` up/up, bật `ip dns server`, có bản ghi A, ping được gateway.
- VIP DNS UDP/53 có sẵn nhưng policy 3 lúc đầu chỉ cho `HTTP`; sniffer thấy gói DNS đến port1 nhưng chưa vào DMZ.
- Sửa policy 3 thêm dịch vụ `DNS` bên cạnh `HTTP` (không tạo policy thứ tư vì giới hạn Evaluation).
- Sau đó `nslookup -type=A www.enterprise.test 192.168.16.134` trả kết quả `192.168.16.134`.

## 5. Windows phân giải tên lúc được lúc không
- Windows ưu tiên DNS của Wi-Fi `8.8.8.8`, không biết tên miền `.test` nội bộ.
- Tạm cấu hình DNS VMnet8 và Wi-Fi là `192.168.16.134`; chạy PowerShell với quyền quản trị để tránh lỗi `PermissionDenied`.
- Xóa DNS cache; `Resolve-DnsName` thành công 5/5, HTTP theo tên miền trả 401 5/5.
- Sau khi chụp bằng chứng, nên phục hồi DNS Wi-Fi ban đầu hoặc cân nhắc NRPT chỉ áp dụng cho `.enterprise.test`.

## 6. TFTP trên Cisco IOL không có `flash:`
- `dir flash:` không hợp lệ; `disk0:` và `disk1:` báo `No such device`; `unix:` có quyền đọc/ghi.
- `tftp-server ?` hỗ trợ `unix:`; cấu hình `tftp-server unix:tftp-test.cfg 10` cùng ACL 10.
- FILE-SERVER có `tftp-test.cfg` **1212 byte** và CORE có `download-test.cfg` **1212 byte**.
- Khi gõ nhầm `dir unix:` trong lời nhắc tên file, IOS tạo file `dir` 1246 byte; file này không liên quan TFTP.
- Không công bố file running-config gốc vì có thể chứa bí mật; chỉ công khai cấu hình đã làm sạch.
