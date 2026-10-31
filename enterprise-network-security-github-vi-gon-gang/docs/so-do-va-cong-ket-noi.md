# Sơ đồ và cổng kết nối cần đối chiếu

> Bảng sau phản ánh kết nối đã thảo luận. Chưa có `show cdp neighbors detail` đầy đủ của từng thiết bị nên **cần đối chiếu lại với EVE-NG trước khi dùng file cấu hình**.

| Thiết bị | Cổng | Nối tới | Ghi chú |
|---|---|---|---|
| FortiGate | port1 | VMnet8 | WAN mô phỏng, địa chỉ DHCP |
| FortiGate | port2 | CORE Ethernet0/0 | 10.0.0.0/30 |
| FortiGate | port3 | SW6 DMZ | 10.30.0.8/29 |
| CORE | Ethernet0/1 | DIS1 Ethernet0/0 | 10.0.0.4/30 |
| CORE | Ethernet0/2 | DIS2 Ethernet0/0 | 10.0.0.8/30 |
| DIS1–DIS2 | Ethernet0/1, Ethernet0/2 | Port-channel1 | EtherChannel L2 |
| DIS1 / DIS2 | Ethernet0/3 | SW1 (VLAN10) | Trunk |
| DIS1 / DIS2 | Ethernet1/0 | SW2 (VLAN20) | Trunk |
| DIS1 / DIS2 | Ethernet1/1 | SW3 (VLAN30) | Trunk |
| DIS1 / DIS2 | Ethernet1/2 | SW4 (VLAN40) | Trunk |
| CORE | Chưa xác nhận cổng tới SW5 | SW5 | VLAN50, kiểm tra trên topology |
| SW5 | Cổng máy chủ chưa xác nhận | DHCP-SERVER, FILE-SERVER | Access VLAN50 |
| SW6 | Cổng máy chủ chưa xác nhận | WEB-SERVER, DNS-SERVER | DMZ |

Ảnh topology thực: đặt vào `evidence/topology/` sau khi kiểm tra đúng cổng.
