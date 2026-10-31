# Ma trận kiểm thử

| Mã | Mục tiêu | Kỳ vọng | Trạng thái hiện tại |
|---|---|---|---|
| TC01 | Kết nối WEB đến gateway DMZ | Ping thành công | ĐẠT |
| TC02 | HTTP qua VIP | Phản hồi HTTP từ WEB-SERVER | ĐẠT |
| TC03 | DNS qua VIP | Trả A record chính xác | ĐẠT |
| TC04 | DNS 5 lần | 5 lần trả đúng IP | ĐẠT |
| TC05 | HTTP theo tên 5 lần | Nhận phản hồi ứng dụng | ĐẠT |
| TC06 | TFTP FILE-SERVER → CORE | Nhận file, kiểm tra kích thước | ĐẠT; chưa hash |
| TC07 | WAN → VLAN 50 | Bị chặn và có log deny | ĐÃ QUAN SÁT timeout;  deny |
| TC08 | DHCP VLAN 10–40 | PC nhận IP/gateway/DNS đúng |  |
| TC09 | Internet từ PC | Kết nối ra ngoài qua NAT |  |
| TC10 | HSRP/STP failover | Khôi phục kết nối sau ngắt link |  |


