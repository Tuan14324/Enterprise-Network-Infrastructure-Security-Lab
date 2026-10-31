# Quy hoạch địa chỉ IP và đấu nối

## VLAN người dùng và VLAN máy chủ

| VLAN | Chức năng | Mạng | Gateway ảo/CORE | DIS1 SVI | DIS2 SVI |
|---|---|---|---|---|---|
| 10 | IT | `10.10.10.0/24` | `10.10.10.1` | `10.10.10.2` | `10.10.10.3` |
| 20 | SALE | `10.10.20.0/24` | `10.10.20.1` | `10.10.20.2` | `10.10.20.3` |
| 30 | MKT | `10.10.30.0/24` | `10.10.30.1` | `10.10.30.2` | `10.10.30.3` |
| 40 | USER | `10.10.40.0/24` | `10.10.40.1` | `10.10.40.2` | `10.10.40.3` |
| 50 | SERVER | `10.10.50.0/24` | CORE `10.10.50.1` | — | — |

- DIS1 dự kiến Active HSRP VLAN 10, 30; DIS2 dự kiến Active HSRP VLAN 20, 40.
- DHCP-SERVER: `10.10.50.10/24`; FILE-SERVER: `10.10.50.11/24`, gateway `10.10.50.1`.
- DHCP cấp DNS `10.30.0.11` thuộc DMZ; cần cho phép lưu lượng DNS LAN→DMZ.

## Đường truyền Layer 3

| Kết nối | Thiết bị A | Thiết bị B |
|---|---|---|
| `10.0.0.0/30` | FortiGate port2 `10.0.0.1` | CORE e0/0 `10.0.0.2` |
| `10.0.0.4/30` | CORE e0/1 `10.0.0.5` | DIS1 e0/0 `10.0.0.6` |
| `10.0.0.8/30` | CORE e0/2 `10.0.0.9` | DIS2 e0/0 `10.0.0.10` |

## Vùng DMZ và WAN mô phỏng

| Thiết bị/dịch vụ | Địa chỉ |
|---|---|
| FortiGate port3 | `10.30.0.9/29` |
| WEB-SERVER | `10.30.0.10/29`, gateway `.9` |
| DNS-SERVER | `10.30.0.11/29`, gateway `.9` |
| FortiGate port1 | DHCP VMnet8; quan sát `192.168.16.134/24` |
| VMware NAT | `192.168.16.2` |
| Windows adapter VMnet8 | `192.168.16.1` trong bài kiểm thử |
| VPCS phía WAN | `192.168.16.147/24` trong bài kiểm thử |
| VIP HTTP | `192.168.16.134:80` → `10.30.0.10:80` TCP |
| VIP DNS | `192.168.16.134:53` → `10.30.0.11:53` UDP |
| Bản ghi A tên miền lab | `www.enterprise.test` → `192.168.16.134` |

**Chú ý:** Địa chỉ FortiGate WAN được DHCP cấp, có thể đổi; khi đổi cần kiểm tra VIP và bản ghi DNS. Đây là IP riêng, không định tuyến trực tiếp trên Internet.

## Bảng cổng tham khảo (phải đối chiếu topology EVE-NG)

- CORE `e0/0`→FortiGate port2; `e0/1`→DIS1 e0/0; `e0/2`→DIS2 e0/0; `e0/3`→SW5 e0/0.
- DIS1/DIS2 dùng `e0/1` và `e0/2` làm EtherChannel `Port-channel1`.
- DIS1 và DIS2: `e0/3`→SW1, `e1/0`→SW2, `e1/1`→SW3, `e1/2`→SW4.
- Mỗi SW1–SW4: `e0/0`→DIS1, `e0/1`→DIS2, `e0/2`→VPCS.
- SW5: `e0/1`→DHCP-SERVER, `e0/2`→FILE-SERVER.
- SW6: `e0/0`→FortiGate port3, `e0/1`→WEB-SERVER, `e0/2`→DNS-SERVER.

VLAN quản trị `10.99.0.0/24` từng xuất hiện ở thiết kế cũ nhưng **chưa xác nhận tồn tại trong topology một CORE hiện tại**, nên không đưa vào cấu hình mẫu.
