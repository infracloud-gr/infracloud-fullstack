# Trang Dashboard (Tổng quan hệ thống)

## Dashboard là gì?

Dashboard là **trang đầu tiên** bạn thấy sau khi đăng nhập. Đây là trang tổng quan cho phép bạn nắm bắt nhanh tình trạng toàn bộ hệ thống của mình mà không cần vào từng mục riêng lẻ.

---

## Những gì bạn thấy trên Dashboard

### 1. Các thẻ thống kê nhanh (Summary Cards)

Đây là các ô tóm tắt hiển thị **tổng số lượng** tài nguyên bạn đang có:

| Thẻ | Ý nghĩa |
|-----|---------|
| **Máy ảo** | Tổng số Virtual Machine đã tạo |
| **Máy chủ** | Tổng số Compute Server trong hệ thống |
| **Mạng** | Tổng số Network đã tạo |
| **Volume** | Tổng số ổ đĩa mở rộng |
| **Ticket** | Số yêu cầu hỗ trợ đang mở |

Nhấn vào một thẻ sẽ điều hướng thẳng đến trang quản lý tương ứng.

### 2. Biểu đồ máy ảo theo tháng

Biểu đồ cột thể hiện **số lượng máy ảo được tạo mới theo từng tháng** trong một năm. Điều này giúp bạn theo dõi xu hướng sử dụng tài nguyên.

**Cách dùng biểu đồ:**
- Có một ô chọn năm (ví dụ: 2025, 2026...)
- Thay đổi năm để xem biểu đồ của năm tương ứng
- Di chuột vào từng cột để xem số lượng chính xác của tháng đó

---

## Cách điều hướng từ Dashboard

Từ Dashboard, bạn có thể:
- Nhấn vào các thẻ thống kê để đi đến module tương ứng
- Sử dụng menu bên trái để chuyển đến bất kỳ module nào
- Xem nhanh tình trạng và quyết định cần làm gì tiếp theo

---

## Câu hỏi thường gặp

**Q: Con số trên thẻ thống kê không đúng, làm thế nào để cập nhật?**
A: Tải lại trang (F5 hoặc Ctrl+R). Dashboard hiển thị dữ liệu thời gian thực từ hệ thống.

**Q: Biểu đồ không hiển thị dữ liệu năm hiện tại?**
A: Kiểm tra xem ô chọn năm đã đúng chưa. Nếu năm hiện tại chưa có máy ảo nào được tạo, biểu đồ sẽ trống.

**Q: Tôi thấy số máy ảo bằng 0 nhưng thực ra đã tạo rồi?**
A: Có thể máy ảo đã bị xóa hoặc đang ở trạng thái bất thường. Vào menu Virtual Machine để xem chi tiết.
