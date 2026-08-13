# Nhật ký hoạt động (Activity Log)

## Nhật ký hoạt động là gì?

**Nhật ký hoạt động** ghi lại toàn bộ các thao tác quan trọng được thực hiện trong hệ thống — bao gồm tạo, sửa, xóa tài nguyên, đăng nhập, bật/tắt máy ảo... Đây là công cụ giúp bạn:
- Theo dõi **ai đã làm gì và khi nào**
- Tìm lại **nguyên nhân** khi có sự cố
- Kiểm tra xem tài khoản có bị **truy cập trái phép** không

---

## Xem Nhật ký hoạt động

Vào menu **Nhật ký hoạt động** trong thanh điều hướng.

Bảng hiển thị các cột:
- **Thời gian**: Khi nào thao tác xảy ra
- **Người dùng**: Ai thực hiện
- **Hành động**: Làm gì (Tạo, Xóa, Cập nhật, Đăng nhập...)
- **Phân hệ**: Thao tác trên module nào (VM, Network, Firewall...)
- **Mô tả**: Giải thích chi tiết thao tác
- **Địa chỉ IP**: IP người dùng khi thực hiện
- **Thiết bị**: Loại thiết bị (Desktop, Mobile...)

---

## Lọc và tìm kiếm Log

### Lọc theo Hành động
Dùng dropdown **"Hành động"** để chọn loại thao tác muốn xem:

| Hành động | Xem để làm gì |
|---------|--------------|
| **Tạo (Create)** | Xem ai đã tạo tài nguyên gì |
| **Xóa (Delete)** | Xem ai đã xóa tài nguyên nào |
| **Cập nhật (Update)** | Xem những thay đổi cấu hình |
| **Đăng nhập (Login)** | Kiểm tra lịch sử đăng nhập |
| **Bật VM (Turn On)** | Xem khi nào VM được bật |
| **Tắt VM (Turn Off)** | Xem khi nào VM bị tắt |
| **Gắn (Attach)** | Volume hoặc Firewall được gắn vào VM |
| **Tháo (Detach)** | Volume hoặc Firewall bị tháo ra |
| **Snapshot** | Khi nào snapshot được tạo |
| **Khôi phục (Revert)** | Khi nào VM được revert về snapshot |

### Lọc theo Phân hệ (Module)
Dùng dropdown **"Phân hệ"** để chỉ xem log của một module cụ thể:
- Máy ảo, Mạng, Tường lửa, Volume, SSH Key, Ticket...

### Tìm kiếm tự do
Nhập từ khóa vào ô tìm kiếm để lọc theo nội dung mô tả.

---

## Đọc hiểu log — Ví dụ thực tế

### Ví dụ 1: Log tạo máy ảo
```
Thời gian:  2025-06-15 14:30:00
Người dùng: nguyen.van.a@company.com
Hành động:  Tạo
Phân hệ:    Máy ảo
Mô tả:      Tạo máy ảo "web-server-01" với flavor medium (2 vCPU, 4GB RAM, 60GB Disk) trên subnet "subnet-web"
IP:         203.0.113.100
Thiết bị:   Desktop / Windows 11 / Chrome
```

### Ví dụ 2: Log xóa volume
```
Thời gian:  2025-06-15 15:00:00
Người dùng: admin@company.com
Hành động:  Xóa
Phân hệ:    Volume
Mô tả:      Xóa volume "data-volume-01" (50GB) khỏi hệ thống
IP:         192.168.1.50
Thiết bị:   Desktop / macOS / Safari
```

### Ví dụ 3: Log đăng nhập
```
Thời gian:  2025-06-15 08:00:00
Người dùng: nguyen.van.a@company.com
Hành động:  Đăng nhập
Phân hệ:    Xác thực
Mô tả:      Đăng nhập thành công
IP:         1.2.3.4
Thiết bị:   Mobile / iOS / Safari Mobile
```

---

## Sử dụng log để giải quyết vấn đề

### Kịch bản 1: VM bị xóa, muốn biết ai xóa
1. Lọc: **Hành động = Xóa** + **Phân hệ = Máy ảo**
2. Tìm theo thời gian gần đây
3. Xem cột Người dùng và Mô tả để biết ai xóa máy ảo nào

### Kịch bản 2: Kiểm tra tài khoản bị truy cập trái phép
1. Lọc: **Hành động = Đăng nhập**
2. Xem các log đăng nhập của tài khoản mình
3. Kiểm tra: IP lạ? Thiết bị lạ? Thời gian bất thường (3h sáng)?

### Kịch bản 3: Tìm nguyên nhân VM tự nhiên bị tắt
1. Lọc: **Hành động = Tắt VM** + **Phân hệ = Máy ảo**
2. Tìm VM bị ảnh hưởng trong cột Mô tả
3. Xem ai đã tắt và vào thời điểm nào

### Kịch bản 4: Xem toàn bộ thay đổi của một người dùng
1. Tìm kiếm theo email của người dùng đó
2. Xem toàn bộ hành động của họ theo thứ tự thời gian

---

## Lưu ý về Nhật ký hoạt động

- Log **chỉ để xem**, không thể sửa hoặc xóa — đảm bảo tính toàn vẹn
- Toàn bộ thao tác trong hệ thống đều được ghi lại **tự động**
- Log được lưu trong một khoảng thời gian nhất định (hỏi quản trị viên về chính sách lưu trữ log)
- Nếu phát hiện hoạt động bất thường, hãy **đổi mật khẩu ngay** và gửi Ticket báo cáo

---

## Câu hỏi thường gặp

**Q: Tôi không thấy log của mình trong nhật ký, tại sao?**
A: Kiểm tra lại bộ lọc — có thể đang lọc sai phân hệ hoặc hành động. Thử xóa tất cả bộ lọc để xem toàn bộ log.

**Q: Log hiển thị thao tác xóa VM nhưng tôi không xóa, phải làm gì?**
A: Đây là dấu hiệu tài khoản bị xâm phạm. Đổi mật khẩu ngay lập tức và gửi Ticket với mức **Critical** để báo cáo.

**Q: Tôi muốn xuất log ra file Excel để phân tích, có được không?**
A: Tính năng xuất log hiện chưa có trên giao diện. Nếu cần, hãy gửi Ticket yêu cầu.

**Q: Log lưu trong bao lâu?**
A: Phụ thuộc vào cấu hình hệ thống. Hỏi quản trị viên để biết chính xác.
