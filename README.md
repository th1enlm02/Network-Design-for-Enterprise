# 🛜 Network Design for Enterprise (Thiết kế mạng cho doanh nghiệp)

## 🛈 Tổng quan

Công ty Outsource O-UIT là một doanh nghiệp hoạt động trong lĩnh vực outsourcing và phát triển ứng dụng phần mềm. Trụ sở chính của công ty nằm tại Thủ Đức, gồm một tòa nhà 5 tầng với Data Center và các văn phòng làm việc của các phòng ban quan trọng như CEO, HR, Project Manager, Technical Manager, Business Analyst, IT Manager, Developer và Tester cho các dự án thuộc thị trường nước ngoài. Công ty cũng có một chi nhánh tại Quận 3, chuyên về các dự án phát triển ứng dụng phần mềm cho thị trường trong nước.

👉 **Mục tiêu**: Đưa ra một hướng dẫn và giải pháp cho việc thiết kế hệ thống mạng cho công ty Outsource O-UIT. Dự án sẽ trình bày một kế hoạch chi tiết cho việc triển khai mạng tại trụ sở chính và chi nhánh của công ty, đáp ứng các yêu cầu cụ thể của khách hàng.

<br>

<p align="center">
    <img src="./images/mo-hinh-tong-quan.png" alt="Mô hình mạng tổng quan"></img>
</p>

## 🤝 Yêu cầu tiên quyết

1. **Trụ sở chính**:
    - Developer và Tester chỉ được sử dụng máy bàn tại công ty, không được sử dụng laptop riêng để truy cập vào mạng của công ty.
    CEO, HR, Project manager, Technical Manager, Business Analyst, IT operation được sử dụng Laptop, truy cập vào hệ thống wifi nội bộ sử dụng tài khoản xác thực.
    - Một hệ thống wifi public với đường kết nối Internet riêng.
    - Hệ thống phần cứng để triển khai hệ thống server ảo phục vụ cho việc deploy các ứng dụng trong giai đoạn test.
    - Sử dụng các dịch vụ Cloud để deploy các ứng dụng trong giai đoạn staging để khách hàng sử dụng thử trước khi đưa ra thực tế.

<p align="center">
    <img src="./images/minh-hoa-tru-so-chinh.png" alt="Minh họa trụ sở chính"></img>
</p>

2. **Chi nhánh**:
    - Developer và Tester chỉ được sử dụng máy bàn tại công ty, không được sử dụng laptop riêng để truy cập vào mạng của công ty.
    - Sử dụng kết nối VPN site-to-site để truy cập server nội bộ và deploy ứng dụng lên hệ thống tại Data Center.
    - Một hệ thống wifi với đường kết nối Internet riêng.

<p align="center">
    <img src="./images/minh-hoa-chi-nhanh.png" alt="Minh họa chi nhánh"></img>
</p>

## 🔌 Thiết kế hệ thống mạng

### Thiết kế mô hình mạng logic

<p align="center">
    <img src="./images/mo-hinh-mang-logic-pkt.png" alt="Mô hình mạng logic"></img>
</p>

1. **Mô hình mạng logic trụ sở chính**:

- Triển khai mô hình mạng phân cấp (**Hierarchical Networks**) 👉 Là một kiến trúc mạng được thiết kế theo cấu trúc có tổ chức, giúp tối ưu hóa hiệu suất và quản lý trong một môi trường mạng lớn 👉 Sử dụng các lớp để đơn giản nhiệm vụ kết nối mạng, mỗi lớp có thể chỉ tập trung vào một chức năng cụ thể, cho phép chúng ta lựa chọn các tính năng và các hệ thống thích hợp cho mỗi lớp.<br>
💡 Cụ thể đối với mô hình đề xuất triển khai, với quy mô doanh nghiệp nhỏ và vừa có thể gộp _Core layer_ vào _Distribution layer_ và phần còn lại là _Access layer_.<br>
    - **Kết nối internet**: Gồm các thiết bị router kết nối với nhà cung cấp dịch vụ internet (ISP) để cung cấp các dịch vụ liên quan đến kết nối và tốc độ mạng, đồng thời đảm nhiệm vai trò là thiết bị nằm giữa mạng nội bộ với thế giới bên ngoài, cần có các cấu hình cần thiết (NAT, VPN…) để các thiết bị bên trong mạng có thể giao tiếp ra bên ngoài internet và ngược lại.
    - **Distribution layer**: Lớp này gồm các thiết bị như router, firewall và các multilayer switch chịu trách nhiệm đảm bảo khả năng định tuyến giữa các mạng LAN và các subnet trong mạng, layer này cũng chịu trách nhiệm áp dụng các policy-based cho các kết nối trong mạng.
    - **Access layer**: Lớp này ở phía cạnh người dùng cuối, bao gồm các thiết bị như switch, access point và các thiết bị truy cập khác. Nó là điểm kết nối trực tiếp giữa thiết bị người dùng và mạng nội bộ.
    - **Hệ thống access point Wi-Fi**:
        - Wi-Fi nội bộ: Triển khai trên mỗi phòng của các tầng một access point Wi-Fi với tốc độ mạng khác nhau tuỳ thuộc vào nhu cầu sử dụng của từng nhóm nhân viên trong công ty.
        - Wi-Fi công cộng: Hệ thống Wi-Fi công cộng có đường kết nối internet riêng tách biệt với mạng nội bộ, được triển khai trên các tầng 1, 2, 3 đảm bảo có thể phục vụ được nhu cầu kết nối internet khi cần thiết. Gồm các access point kết nối đến switch và kết nối đến router có đường kết nối với internet, có các cấu hình cần thiết trên các thiết bị switch và router để có thể cấp địa chỉ IP, chia vùng mạng...
    - **Vùng DMZ**: Là 1 vùng nằm giữa mạng nội bộ (LAN) và internet. DMZ là nơi chứa các public server và cung cấp các service cho các host trong LAN cũng như các host từ các LAN bên ngoài. DMZ được sinh ra để đảm bảo hai vai trò: Vẫn có thể cung cấp các dịch vụ cho host của LAN và các host từ các LAN khác, đồng thời có thể bảo vệ các host trong LAN khỏi việc bị tấn công bởi các host từ LAN khác thông qua các public server.

<p align="center">
    <img src="./images/mo-hinh-mang-logic-tru-so-chinh.png" alt="Mô hình mạng logic trụ sở chính"></img>
</p>

2. **Mô hình mạng logic chi nhánh**:

- Chi nhánh của công ty được chia ra làm 3 khu vực chính: Sảnh; Phòng Developer; Phòng Tester.
    - 2 router có kết nối đến internet:
        - Router kết nối với mạng nội bộ.
        - Router kết nối với mạng Wi-Fi công cộng.
    - 1 multilayer switch và các switch trong mạng nội bộ.
    - 1 firewall giữa kết nối của multilayer switch với router để thiết lập các rule bảo mật cho hệ thống mạng.
    - 3 access point Wi-Fi nội bộ.
    - 1 access point Wi-Fi công cộng với địa chỉ mạng riêng tách biệt với mạng nội bộ.

<p align="center">
    <img src="./images/mo-hinh-mang-logic-chi-nhanh.png" alt="Mô hình mạng logic chi nhánh"></img>
</p>

3. **Các giao thức và cấu hình triển khai trong hệ thống**:
- **Địa chỉ IP**: Cấu hình địa chỉ IP cho các interface của các thiết bị. Đối với Firewall sẽ cấu hình thêm mức độ bảo mật (security-level) phù hợp cho từng vùng khác nhau.
- **Multilayer Switch Inter-VLAN Routing**: Cấu hình Multilayer Switch Inter-VLAN Routing để chia VLAN và định tuyến giữa các VLAN trong mạng.
- **RoS-InterVLAN Routing**: Cấu hình RoS-InterVLAN Routing để chia VLAN cho các điểm truy cập Wi-Fi công cộng.
- **RIPv2**: Cấu hình định tuyến động RIPv2 để các thiết bị trong mạng nội bộ có thể giao tiếp với nhau.
- **Static routing**: Cấu hình định tuyến tĩnh để các thiết bị trong hệ thống Wi-Fi công cộng có thể truy cập đến nhau. Đồng thời cấu hình thêm đường default static route kết nối đến interface ngoài internet.
- **DHCP**: Cấu hình DHCP để tự động cấp địa chỉ IP cho các thiết bị trong mạng.
- **NAT Overloading – PAT (Port Address Translation)**: Cấu hình NAT Overloading – PAT để ánh xạ nhiều địa chỉ IP cục bộ sang địa chỉ IP công cộng, mỗi địa chỉ IP được phân biệt bằng số port cho truy cập từ mạng bên trong ra ngoài internet và ngược lại.
- **Static NAT**: Cấu hình static NAT để ánh xạ địa chỉ IP cục bộ sang địa chỉ IP công cộng dành cho các public server.
- **ACL (Access Control List)**: Cấu hình ACL quản lý truy cập giữa các vùng mạng.
- **STP (Spanning Tree Protocol)**: Cấu hình STP để chống loop trong hệ thống mạng khi kết nối 2 core switch với nhau.
- **VPN site-to-site**: Cấu hình VPN site-to-site để vùng mạng của công ty và chi nhánh có thể truy cập với nhau.

### Các dịch vụ, thiết bị và chi phí cho hệ thống
👉 Đọc thêm trong báo cáo chi tiết [**tại đây**](./[Bao%20cao%20TKM%20HK1%202023-2024]%20-%20Nhom%2007.pdf).

## Đánh giá kết quả đạt được

1. **Phân tích yêu cầu** 👉 <span style='color: red;'> 100% </span>
    - ✅ Yêu cầu về người dùng.
    - ✅ Yêu cầu về mạng không dây.
    - ✅ Yêu cầu về dịch vụ.
2. **Thiết kế hệ thống mạng** 👉 <span style='color: red;'> 100% </span>
    - ✅ Thiết kế mô hình mạng logic.
    - ✅ Thiết kế mô hình địa chỉ IP cho hệ thống mạng.
    - ✅ Thiết kế sơ đồ vật lý cho toàn bộ hệ thống mạng.
3. **Chi phí cho hệ thống** 👉 <span style='color: red;'> 100% </span>
    - ✅ Chi phí cho thiết bị.
    - ✅ Chi phí cho dịch vụ.
