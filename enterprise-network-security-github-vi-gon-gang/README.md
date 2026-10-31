# Mô phỏng mạng doanh nghiệp với Cisco và FortiGate

Mô hình mạng doanh nghiệp triển khai trên **EVE-NG**, sử dụng Cisco IOS/IOL và FortiGate VM 7.6.8 Evaluation. Dự án tập trung vào phân chia VLAN, định tuyến nội bộ, dự phòng tại Distribution, bảo vệ vùng DMZ và kiểm tra các dịch vụ mạng.

![Sơ đồ logic](assets/topology-logical.svg)

## Kiến trúc

- **CORE:** định tuyến tới FortiGate, hai Distribution và VLAN 50.
- **DIS1/DIS2:** OSPF, HSRP, Rapid-PVST, EtherChannel và DHCP Relay.
- **SW1–SW4:** switch truy cập cho VLAN 10–40, kết nối dự phòng tới DIS1/DIS2.
- **SW5:** máy chủ nội bộ DHCP và TFTP tại VLAN 50.
- **SW6:** WEB và DNS giả lập trong DMZ.
- **FortiGate:** ba vùng WAN (VMnet8), LAN và DMZ; chính sách truy cập, NAT và VIP.

## Công nghệ

`VLAN` · `802.1Q` · `OSPF` · `HSRP` · `Rapid-PVST` · `EtherChannel` · `DHCP Relay` · `FortiGate Firewall` · `NAT/VIP` · `HTTP` · `DNS` · `TFTP`

## Kết quả kiểm thử

| Hạng mục | Kết quả |
|---|---|
| HTTP qua VIP WAN mô phỏng → DMZ | Đạt: nhận HTTP 401, trang đăng nhập IOS hiển thị |
| DNS qua VIP UDP/53 | Đạt: tên `www.enterprise.test` phân giải thành `192.168.16.134` |
| DNS/HTTP theo tên miền (5 lần) | Đạt: 5/5 truy vấn DNS và 5/5 phản hồi HTTP 401 |
| TFTP từ FILE-SERVER đến CORE | Đạt: hai file cùng dung lượng 1.212 byte; chưa đối chiếu hash |
| WAN ping server VLAN 50 | Timeout; chưa có log deny xác nhận nguyên nhân |
| DHCP trên PC, Internet từ PC, failover HSRP/STP | Chưa xác minh đầy đủ |

Chi tiết xem [báo cáo kết quả](tests/bao-cao-ket-qua-kiem-thu.md) và [ma trận kiểm thử](tests/test-matrix.md).

## Tài liệu dự án

| File | Chức năng |
|---|---|
| [Yêu cầu](docs/design-requirements.md) | Mục tiêu và tiêu chí thiết kế |
| [Quy hoạch IP](docs/ip-plan.md) | VLAN, subnet, gateway và thiết bị |
| [Cổng kết nối](docs/so-do-va-cong-ket-noi.md) | Bảng kết nối các thiết bị |
| [Triển khai](docs/deployment.md) | Trình tự cấu hình và xác minh |
| [Xử lý sự cố](docs/troubleshooting.md) | Lỗi đã gặp và cách khắc phục |
| [Giới hạn](docs/limitations.md) | Những gì mô hình chưa đáp ứng |
| [Cấu hình](configs/) | Cấu hình **tham khảo**, không phải running-config đã xuất |
| [Lệnh kiểm thử](tests/test-commands.md) | Các lệnh thu thập kết quả |
| [Bằng chứng](evidence/) | Nơi bổ sung ảnh và log thực tế |


