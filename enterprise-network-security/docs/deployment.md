# Quy trình triển khai từng bước

> Cấu hình trong `configs/` chỉ là **mẫu tham khảo**. Sao lưu trước và đối chiếu cổng trên topology thực tế, không dán hàng loạt vào thiết bị đã hoạt động.

## Bước 1 – Khảo sát hiện trạng
Chạy `show ip interface brief`, `show interfaces status`, `show vlan brief`, `show interfaces trunk`, `show etherchannel summary` và lưu `show running-config` ở nơi **không công khai**.

## Bước 2 – FortiGate và CORE
- Kiểm tra FortiGate: `get system status`, xác nhận license hợp lệ; `port1` DHCP WAN, `port2` đến CORE, `port3` đến DMZ.
- Thiết lập ba kết nối lớp 3 FGT–CORE, CORE–DIS1, CORE–DIS2.
- Từ CORE ping `10.0.0.1`, `10.0.0.6`, `10.0.0.10`.

## Bước 3 – Distribution và Access
- Cấu hình OSPF trên CORE/DIS1/DIS2.
- Tạo VLAN 10/20/30/40, EtherChannel `Po1` giữa DIS1–DIS2.
- Trên image hỗ trợ chọn encapsulation, nhập `switchport trunk encapsulation dot1q` **trước** `switchport mode trunk`.
- Cấu hình uplink trunk hai đầu, SVI, HSRP và Rapid-PVST.
- Xác nhận `show interfaces trunk`, `show spanning-tree vlan 10`, `show standby brief`, `show ip ospf neighbor`.

## Bước 4 – VLAN 50 và DHCP
- CORE dùng SVI VLAN50 `10.10.50.1`; DHCP-SERVER `10.10.50.10`; FILE-SERVER `10.10.50.11`.
- Thêm `ip helper-address 10.10.50.10` tại các SVI VLAN10–40 **trên cả DIS1 và DIS2**.
- Trên PC VPCS chạy `ip dhcp`, kiểm tra lease; kết quả cuối cùng **chưa được cung cấp**.

## Bước 5 – DMZ
- WEB-SERVER `10.30.0.10`, DNS-SERVER `10.30.0.11`; FortiGate port3 `10.30.0.9`.
- Chỉ dùng HTTP quản trị IOS trong lab riêng; không công khai lên Internet thật.

## Bước 6 – NAT, VIP, policy
- FortiGate có route `10.10.0.0/16` qua CORE `10.0.0.2`.
- Policy 1: VLAN 10–40 ra port1 có NAT.
- Policy 2: LAN→DMZ cho PING/DNS/HTTP/HTTPS.
- Policy 3: WAN→DMZ với cả VIP HTTP và VIP DNS, dịch vụ **HTTP và DNS**.
- Kiểm tra giới hạn giấy phép Evaluation trước khi thay đổi số policy/interface.

## Bước 7 – Kiểm thử và lưu minh chứng
- Windows WAN: `nslookup`, `Resolve-DnsName`, `curl.exe`.
- CORE tải TFTP từ FILE-SERVER; so sánh file bằng checksum nếu có thể.
- Xác nhận OSPF, HSRP, DHCP, các luồng bị chặn bằng **log thực tế**.
- Tạo ảnh kiểm thử và đặt vào `tests/evidence/` sau khi che thông tin nhạy cảm.

**Không factory-reset FortiGate đã kích hoạt giấy phép chỉ để làm lại lab.**
