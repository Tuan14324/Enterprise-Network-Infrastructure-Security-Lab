# Yêu cầu thiết kế

## Yêu cầu chức năng

1. Phân chia người dùng theo VLAN 10 (IT), 20 (SALE), 30 (MKT), 40 (USER); VLAN 50 dành cho máy chủ nội bộ.
2. Định tuyến nội bộ với OSPF giữa CORE và DIS1/DIS2.
3. HSRP cung cấp gateway dự phòng trên DIS1/DIS2; Rapid-PVST tránh loop; EtherChannel ghép hai đường DIS1–DIS2.
4. DHCP cấp IP cho VLAN người dùng, sử dụng DHCP Relay tại SVI Distribution.
5. FortiGate tách WAN, LAN và DMZ; NAT cho VLAN người dùng; VIP đưa HTTP và DNS vào DMZ.
6. TFTP tại VLAN 50 phục vụ tải file nội bộ, giới hạn bằng ACL.

## Tiêu chí nghiệm thu

- Xác nhận trunk, SVI, OSPF, HSRP, EtherChannel bằng đầu ra lệnh thực tế.
- PC nhận đúng DHCP lease và đi được tới các đích được phép.
- Truy cập HTTP/DNS qua VIP và tải file TFTP thành công.
- Kiểm thử các luồng bị chặn bằng log firewall, không chỉ dựa vào ping timeout.
- Thử failover có kết quả trước/sau khi ngắt link.
