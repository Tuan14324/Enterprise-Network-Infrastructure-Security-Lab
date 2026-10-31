# Giới hạn và hướng phát triển

## Giới hạn

- Chỉ một CORE và một FortiGate nên vẫn tồn tại điểm lỗi đơn.
- FortiGate VM 7.6.8 Evaluation giới hạn tính năng/tài nguyên theo giấy phép thực tế.
- WAN dùng VMware VMnet8 (192.168.16.0/24), không phải địa chỉ Internet công cộng.
- WEB-SERVER là giao diện quản trị HTTP của Cisco IOS; DNS và TFTP cũng được mô phỏng bằng Cisco IOS/IOL.
- Các file cấu hình trong repository là mẫu tham khảo, chưa được đối chiếu bằng running-config hoàn chỉnh của tất cả thiết bị.
- Chưa có kết quả cuối cho DHCP trên PC, Internet end-to-end, HSRP/STP failover, log deny VLAN50 và checksum TFTP.

## Hướng phát triển

- Hoàn tất các bài kiểm thử chưa xác minh và bổ sung bằng chứng trong `evidence/`.
- Triển khai dịch vụ web/DNS/file server chuyên dụng nếu có tài nguyên.
- Bổ sung CORE thứ hai và FortiGate HA khi cần khảo sát dự phòng toàn hệ thống.
