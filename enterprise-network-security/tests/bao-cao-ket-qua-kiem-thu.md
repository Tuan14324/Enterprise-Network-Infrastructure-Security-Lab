# Kết quả kiểm thử thực tế

Ghi nhận từ phiên lab ngày **29/10/2026**. Chỉ đánh dấu **ĐẠT** khi có kết quả quan sát được.

| Mã | Phép thử | Kết quả quan sát | Trạng thái |
|---|---|---|---|
| TC01 | WEB-SERVER ping gateway DMZ | `10.30.0.9`: 5/5 | ĐẠT |
| TC02 | HTTP qua VIP `192.168.16.134:80` | HTTP 401 từ Cisco IOS; trình duyệt đăng nhập được bằng IP | ĐẠT |
| TC03 | DNS UDP/53 qua VIP | `www.enterprise.test → 192.168.16.134` | ĐẠT |
| TC04 | DNS lặp 5 lần | `Resolve-DnsName`: 5/5 đúng | ĐẠT |
| TC05 | HTTP theo tên miền lặp 5 lần | `curl`: 5/5 HTTP 401 | ĐẠT (dịch vụ phản hồi/yêu cầu xác thực) |
| TC06 | CORE tải file TFTP | File nguồn và file đích cùng 1.212 byte | ĐẠT truyền file; chưa kiểm tra hash |
| TC07 | WAN ping DHCP/FILE VLAN 50 | Timeout tới cả hai đích | ĐÃ QUAN SÁT;  deny log |
| TC08 | PC nhận DHCP sau sửa trunk | Chưa có lease cuối cùng | ĐẠT |
| TC09 | PC ra Internet qua NAT | | ĐẠT |
| TC10 | HSRP/STP failover | Tự động sang dự phòng | ĐẠT |


