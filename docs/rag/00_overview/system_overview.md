# Tổng quan hệ thống Infra Cloud

## Infra Cloud là gì?

**Infra Cloud** là nền tảng quản lý hạ tầng đám mây, cho phép bạn tạo và quản lý các tài nguyên máy chủ ảo, mạng, lưu trữ và bảo mật — tất cả thông qua một giao diện web duy nhất, không cần kiến thức lập trình chuyên sâu.

Hãy hình dung hệ thống như một **trung tâm dữ liệu thu nhỏ trong trình duyệt web**: bạn có thể tạo máy chủ ảo, kết nối chúng vào mạng, gắn thêm ổ đĩa, và bảo vệ chúng bằng tường lửa — chỉ bằng vài thao tác bấm chuột.

---

## Các module trong hệ thống

Sau khi đăng nhập, bạn sẽ thấy menu điều hướng bên trái với các mục:

| Menu | Chức năng |
|------|-----------|
| **Dashboard** | Trang tổng quan — xem nhanh số lượng tài nguyên đang có |
| **Virtual Machine** | Tạo và quản lý máy ảo |
| **Compute Server** | Xem danh sách máy chủ vật lý đang chạy các máy ảo |
| **Cloud Image** | Danh sách hệ điều hành có thể cài lên máy ảo |
| **Volume** | Quản lý ổ đĩa mở rộng |
| **Flavor** | Các gói cấu hình tài nguyên (CPU/RAM/Disk) định sẵn |
| **SSH Key** | Khóa bảo mật để đăng nhập vào máy ảo |
| **Network** | Tạo và quản lý mạng ảo |
| **Router** | Kết nối mạng và cấu hình truy cập từ ngoài vào |
| **Firewall** | Quy tắc bảo vệ máy ảo khỏi truy cập không mong muốn |
| **Ticket** | Gửi yêu cầu hỗ trợ kỹ thuật |
| **Nhật ký hoạt động** | Xem lịch sử toàn bộ thao tác trong hệ thống |

---

## Luồng sử dụng cơ bản cho người mới

Khi lần đầu sử dụng hệ thống, thứ tự thiết lập nên theo trình tự sau:

```
Bước 1: Tạo SSH Key
   → Để đăng nhập máy ảo sau này không cần mật khẩu (an toàn hơn)

Bước 2: Tạo Network và Subnet
   → Xác định mạng nội bộ mà máy ảo sẽ dùng

Bước 3: Tạo Firewall và thêm Rule
   → Xác định ai được phép kết nối vào máy ảo của bạn

Bước 4: Tạo máy ảo (Virtual Machine)
   → Chọn hệ điều hành, cấu hình, mạng, firewall vừa tạo

Bước 5: (Tùy chọn) Cấu hình Port Forwarding qua Router
   → Để truy cập máy ảo từ bên ngoài internet

Bước 6: (Tùy chọn) Gắn thêm Volume
   → Nếu cần thêm không gian lưu trữ cho máy ảo
```

---

## Mối quan hệ giữa các tài nguyên

```
Network (Mạng ảo)
  └── Subnet (Mạng con với dải IP)
        └── Virtual Machine (Máy ảo — nhận IP từ subnet)
              ├── Volume (Ổ đĩa đính kèm)
              ├── Firewall (Tường lửa bảo vệ)
              ├── SSH Key (Khóa đăng nhập)
              └── Snapshot (Ảnh chụp backup)

Flavor (Gói cấu hình: CPU/RAM/Disk)
  └── Dùng khi tạo máy ảo

Cloud Image (Hệ điều hành: Ubuntu, CentOS...)
  └── Dùng khi tạo máy ảo

Router (Bộ định tuyến)
  └── Port Forwarding (Ánh xạ cổng từ ngoài vào VM)
```

---

## Trạng thái tài nguyên

Tất cả tài nguyên trong hệ thống đều có trạng thái hiển thị bằng màu chip:

| Trạng thái | Màu | Ý nghĩa |
|-----------|-----|---------|
| **Running / Active** | Xanh lá | Đang hoạt động bình thường |
| **Stopped / Inactive** | Xám | Đang tắt hoặc ngừng hoạt động |
| **Pending** | Vàng cam | Đang được xử lý (tạo/cấu hình) — chờ một lúc |
| **Error** | Đỏ | Gặp lỗi — cần kiểm tra hoặc liên hệ hỗ trợ |

> **Lưu ý:** Khi tài nguyên ở trạng thái **Pending**, hãy chờ và tải lại trang sau vài phút. Không nên thao tác thêm trong khi đang Pending.

---

## Câu hỏi thường gặp về tổng quan

**Q: Tôi cần làm gì đầu tiên sau khi đăng nhập?**
A: Vào Dashboard để xem tổng quan. Sau đó vào menu Network để tạo mạng nội bộ, rồi tạo SSH Key, và cuối cùng tạo máy ảo đầu tiên.

**Q: Tôi có thể tạo bao nhiêu máy ảo?**
A: Phụ thuộc vào tài nguyên còn lại trên các Compute Server trong hệ thống. Nếu tạo VM thất bại, có thể hệ thống đang không đủ tài nguyên.

**Q: Dữ liệu của tôi có an toàn không nếu tôi tắt máy ảo?**
A: Có. Tắt máy ảo (Turn Off) chỉ dừng máy ảo, dữ liệu trên ổ đĩa vẫn được giữ nguyên. Chỉ khi **xóa** máy ảo thì dữ liệu mới bị xóa.

**Q: Sự khác nhau giữa tắt và xóa máy ảo?**
A: **Tắt (Turn Off)** = tạm dừng, có thể bật lại. **Xóa (Delete)** = mất hoàn toàn, không khôi phục được.
