# Compute Server (Máy chủ vật lý)

## Compute Server là gì?

**Compute Server** là các máy chủ vật lý (bare-metal) thực sự đang chạy các máy ảo của bạn. Hãy hình dung đây là những chiếc máy tính mạnh mẽ đặt trong trung tâm dữ liệu — mỗi máy ảo bạn tạo ra thực sự "sống" bên trong một hoặc nhiều Compute Server này.

**Người dùng thông thường** chỉ cần xem thông tin để hiểu tài nguyên còn lại. Việc thêm/xóa server thường do **quản trị viên** thực hiện.

---

## Những gì bạn thấy trong danh sách Compute Server

Vào menu **Compute Server** (hoặc **Server**) trong thanh điều hướng bên trái.

Mỗi server trong danh sách hiển thị:
- **Tên server**: Tên định danh
- **Địa chỉ IP**: IP công khai của máy chủ
- **CPU**: Tổng số nhân CPU / Số đã dùng
- **RAM**: Tổng RAM / RAM đã dùng
- **Disk**: Tổng ổ đĩa / Đã dùng
- **Mức sử dụng (%)**: Thanh progress bar hiển thị phần trăm tài nguyên đã dùng
- **Trạng thái**: Active / Inactive

---

## Đọc thông tin tài nguyên server

### Thanh progress bar tài nguyên

Mỗi server có 3 thanh hiển thị mức sử dụng:
- **CPU%** — Phần trăm CPU đang được các VM sử dụng thực tế
- **RAM%** — Phần trăm RAM đã phân bổ cho các VM
- **Disk%** — Phần trăm ổ đĩa đã dùng

**Màu sắc cảnh báo (thông thường):**
- 🟢 Xanh (0–60%): Còn nhiều tài nguyên
- 🟡 Vàng (60–80%): Sắp đầy
- 🔴 Đỏ (>80%): Gần hết tài nguyên

### Ý nghĩa thực tế

Nếu tất cả Compute Server đều đạt >90% tài nguyên, bạn **không thể tạo thêm máy ảo** cho đến khi:
- Xóa bớt máy ảo không dùng
- Quản trị viên thêm Compute Server mới

---

## Xem chi tiết Compute Server

Nhấn vào tên hoặc icon xem chi tiết của một server để thấy:
- Thông số đầy đủ (CPU, RAM, Disk tổng và đã dùng)
- Địa chỉ IP công khai và IP nội bộ
- Danh sách các máy ảo đang chạy trên server đó
- Biểu đồ sử dụng tài nguyên theo thời gian thực

---

## Tại sao tôi cần biết về Compute Server?

- Để hiểu **tại sao không tạo được máy ảo** (server đầy tài nguyên)
- Để biết **máy ảo của mình đang chạy ở đâu**
- Để liên hệ quản trị viên khi cần mở rộng hạ tầng

---

## Câu hỏi thường gặp

**Q: Tôi tạo máy ảo nhưng bị lỗi "không đủ tài nguyên", làm gì?**
A: Vào trang Compute Server để xem mức sử dụng. Nếu tất cả server đều >90%, hãy gửi Ticket hỗ trợ để yêu cầu quản trị viên mở rộng.

**Q: Máy ảo của tôi đang chạy trên server nào?**
A: Vào chi tiết máy ảo, có thể xem thông tin Compute Server đang host máy ảo đó.

**Q: Có thể chuyển máy ảo từ server này sang server khác không?**
A: Tính năng migrate VM giữa các server được thực hiện tự động bởi hệ thống hoặc do quản trị viên thực hiện.
