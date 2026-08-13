# Máy ảo (Virtual Machine)

## Máy ảo là gì?

**Máy ảo (Virtual Machine - VM)** là một máy tính hoàn chỉnh chạy bên trong máy chủ vật lý của hệ thống. Dù là "ảo", nhưng nó hoạt động như một máy thật — có hệ điều hành, ổ đĩa, địa chỉ IP, và bạn có thể SSH vào để cài phần mềm, chạy ứng dụng bình thường.

Ví dụ sử dụng phổ biến:
- Chạy web server (Nginx, Apache)
- Chạy database (MySQL, PostgreSQL, MongoDB)
- Môi trường phát triển/test ứng dụng
- Chạy các dịch vụ backend

---

## Hiểu các thông số của máy ảo

Khi tạo hoặc xem máy ảo, bạn sẽ thấy các thông số sau:

| Thông số | Ý nghĩa | Ví dụ |
|---------|---------|-------|
| **Tên** | Tên định danh máy ảo trong hệ thống | `web-server-01` |
| **vCPU** | Số lõi CPU ảo được phân bổ | 2 vCPU |
| **RAM** | Bộ nhớ máy ảo | 4 GB |
| **Root Disk** | Ổ đĩa gốc chứa hệ điều hành | 40 GB |
| **IP** | Địa chỉ mạng nội bộ của máy ảo | `10.0.1.5` |
| **Trạng thái** | Tình trạng hoạt động hiện tại | Running / Stopped |
| **Ngày tạo** | Thời điểm máy ảo được khởi tạo | |

### Trạng thái máy ảo

| Trạng thái | Chip màu | Ý nghĩa |
|-----------|---------|---------|
| **Running** | 🟢 Xanh lá | Đang chạy, có thể dùng bình thường |
| **Stopped** | ⚫ Xám | Đang tắt, dữ liệu vẫn còn nguyên |
| **Pending** | 🟡 Vàng | Đang được hệ thống xử lý — hãy chờ |
| **Error** | 🔴 Đỏ | Gặp lỗi — liên hệ hỗ trợ |

---

## Xem danh sách máy ảo

Vào menu **Virtual Machine** ở thanh bên trái.

Trang danh sách hiển thị bảng gồm: **Tên, IP, Ngày tạo, Trạng thái, Thao tác**.

**Tìm kiếm và lọc:**
- Ô tìm kiếm: Nhập tên máy ảo để lọc kết quả
- Dropdown trạng thái: Lọc máy ảo theo trạng thái (Running, Stopped, ...)
- Phân trang phía dưới bảng để xem thêm

---

## Tạo máy ảo mới

### Bước 1: Mở form tạo
Từ trang danh sách máy ảo, nhấn nút **"Thêm máy ảo"** góc trên bên phải.

### Bước 2: Điền thông tin

**Tên máy ảo**
- Nhập tên dễ nhớ, mô tả được mục đích sử dụng
- Ví dụ: `web-server-01`, `database-prod`, `test-env`
- Không dùng dấu cách, dùng dấu gạch ngang thay thế

**Chọn Flavor (Gói cấu hình)**
- Flavor là gói CPU/RAM/Disk định sẵn
- Hiển thị dưới dạng danh sách: ví dụ `small (1 vCPU, 2GB RAM, 40GB Disk)`
- Chọn flavor phù hợp với nhu cầu sử dụng
- → Xem thêm: [Hướng dẫn chọn Flavor phù hợp](../12_flavor/flavor.md)

**Chọn Cloud Image (Hệ điều hành)**
- Chọn hệ điều hành muốn cài trên máy ảo
- Ví dụ: Ubuntu 22.04 LTS, CentOS 8, Debian 11
- → Xem thêm: [Cloud Image là gì](../06_cloud_image/cloud_image.md)

**Chọn Subnet (Mạng)**
- Chọn mạng con mà máy ảo sẽ kết nối vào
- Máy ảo sẽ nhận địa chỉ IP từ mạng này
- Cần tạo Network và Subnet trước nếu chưa có
- → Xem thêm: [Tạo Network và Subnet](../07_network/network.md)

**Chọn SSH Key** *(Tùy chọn, nhưng khuyến nghị)*
- Chọn SSH Key để đăng nhập máy ảo không cần mật khẩu
- Nếu không chọn, hệ thống tạo mật khẩu root tự động
- → Xem thêm: [Quản lý SSH Key](../10_ssh_key/ssh_key.md)

**Chọn Firewall** *(Tùy chọn)*
- Chọn tường lửa bảo vệ máy ảo
- Nếu không chọn, máy ảo có thể không được bảo vệ
- → Xem thêm: [Tạo Firewall](../09_firewall/firewall.md)

### Bước 3: Xác nhận tạo
Nhấn nút **"Tạo"**. Máy ảo sẽ chuyển sang trạng thái **Pending**.

### Bước 4: Chờ hoàn thành
- Hệ thống tự động chọn máy chủ vật lý phù hợp
- Cài hệ điều hành và khởi động máy ảo
- Thời gian thường từ **1–5 phút**
- Khi hoàn thành, trạng thái chuyển sang **Running**

---

## Xem chi tiết máy ảo

Từ danh sách, nhấn **icon con mắt 👁** hoặc tên máy ảo để vào trang chi tiết.

Trang chi tiết hiển thị:
- **Thông tin cấu hình**: vCPU, RAM, Disk, IP, hệ điều hành
- **Trạng thái** và các nút điều khiển (Bật/Tắt)
- **Tab Volume**: Các ổ đĩa đang đính kèm
- **Tab Firewall**: Tường lửa đang áp dụng
- **Tab Snapshot**: Các điểm backup
- **Tab Port Forwarding**: Cấu hình truy cập từ ngoài
- **Console VNC**: Truy cập màn hình máy ảo trực tiếp qua trình duyệt

---

## Bật máy ảo (Turn On)

Khi máy ảo đang **Stopped**, bạn có thể bật lại:
1. Vào trang chi tiết máy ảo
2. Nhấn nút **"Bật"** (Turn On)
3. Chờ trạng thái chuyển sang **Running** (vài giây đến 1 phút)

---

## Tắt máy ảo (Turn Off)

Để tắt máy ảo đang chạy:
1. Vào trang chi tiết máy ảo
2. Nhấn nút **"Tắt"** (Turn Off)
3. Hệ thống sẽ gửi tín hiệu tắt an toàn đến máy ảo
4. Trạng thái chuyển sang **Stopped**

> **Lưu ý:** Dữ liệu trên ổ đĩa vẫn được giữ nguyên khi tắt. Chỉ khi **xóa** máy ảo mới mất dữ liệu.

---

## Nâng cấp cấu hình máy ảo (Đổi Flavor)

Khi máy ảo cần thêm CPU/RAM:
1. **Tắt máy ảo** trước (bắt buộc)
2. Vào trang chi tiết → Tìm nút **"Đổi cấu hình"** hoặc **"Upgrade Flavor"**
3. Chọn flavor mới có thông số cao hơn
4. Xác nhận — hệ thống áp dụng cấu hình mới
5. Bật lại máy ảo

> **Lưu ý:** Chỉ có thể **nâng cấp** lên flavor lớn hơn, không thể hạ xuống flavor nhỏ hơn.

---

## Tạo lại máy ảo (Recreate)

Recreate sẽ **xây dựng lại máy ảo từ đầu** — hệ điều hành được cài lại sạch.

⚠️ **CẢNH BÁO:** Toàn bộ dữ liệu trong máy ảo sẽ bị xóa khi Recreate. Hãy backup trước.

Khi nào dùng Recreate:
- Máy ảo bị hỏng hệ điều hành không khôi phục được
- Muốn reset về trạng thái ban đầu

---

## Xóa máy ảo

⚠️ **CẢNH BÁO:** Xóa máy ảo là thao tác **không thể hoàn tác**. Toàn bộ dữ liệu sẽ mất.

Để xóa:
1. Từ danh sách, nhấn **icon thùng rác 🗑** của máy ảo muốn xóa
2. Hộp thoại xác nhận hiện ra — đọc kỹ tên máy ảo được hiển thị
3. Nhấn **"Xóa"** để xác nhận

---

## Kết nối vào máy ảo

### Cách 1: SSH (Khuyến nghị)
Dùng terminal/command prompt:
```
ssh root@<IP-máy-ảo>
# Hoặc dùng SSH Key:
ssh -i /đường/dẫn/private-key.pem root@<IP-máy-ảo>
```

Nếu máy ảo không có IP công khai, cần cấu hình Port Forwarding qua Router.
→ Xem thêm: [Cấu hình Port Forwarding](../08_router/router.md)

### Cách 2: Console VNC (Khi không SSH được)
1. Vào trang chi tiết máy ảo
2. Tìm nút/tab **"Console"** hoặc **"VNC"**
3. Một cửa sổ hiển thị màn hình của máy ảo ngay trong trình duyệt
4. Dùng bàn phím để thao tác trực tiếp

> Console VNC hữu ích khi SSH không hoạt động hoặc cần thao tác ở giai đoạn boot.

---

## Câu hỏi thường gặp

**Q: Máy ảo của tôi mãi ở trạng thái Pending, phải làm gì?**
A: Chờ thêm 5 phút và tải lại trang. Nếu vẫn Pending sau 10 phút, hệ thống có thể không đủ tài nguyên — hãy gửi Ticket hỗ trợ.

**Q: Tôi không SSH được vào máy ảo dù máy đang Running?**
A: Kiểm tra: (1) Firewall có rule ALLOW port 22 chưa? (2) Đang dùng đúng IP chưa? (3) Máy ảo có IP công khai không hay cần Port Forwarding? (4) Thử kết nối qua Console VNC để xem bên trong máy.

**Q: Tôi tắt máy ảo thì dữ liệu có mất không?**
A: Không. Tắt (Turn Off) chỉ dừng hoạt động, dữ liệu trên ổ đĩa vẫn an toàn. Chỉ Xóa (Delete) mới mất dữ liệu.

**Q: Làm sao biết mật khẩu root của máy ảo?**
A: Nếu bạn chọn SSH Key khi tạo — dùng SSH Key để đăng nhập (không cần mật khẩu). Nếu không chọn SSH Key — hệ thống tạo mật khẩu ngẫu nhiên, có thể xem trong phần chi tiết máy ảo.

**Q: Tôi muốn cài thêm ổ đĩa cho máy ảo, làm thế nào?**
A: Vào mục Volume để tạo ổ đĩa mới, sau đó Attach vào máy ảo.
→ Xem thêm: [Quản lý Volume](../05_volume/volume.md)
