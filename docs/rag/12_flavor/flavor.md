# Flavor (Gói cấu hình tài nguyên)

## Flavor là gì?

**Flavor** là các gói cấu hình tài nguyên định sẵn cho máy ảo — xác định máy ảo sẽ có bao nhiêu **CPU, RAM và ổ đĩa**. Khi tạo máy ảo, bạn chọn một flavor thay vì phải nhập thủ công từng thông số.

Đây giống như chọn gói dịch vụ: Basic, Standard, Premium — mỗi gói có cấu hình khác nhau với chi phí khác nhau.

---

## Đọc thông số Flavor

Khi xem danh sách Flavor, mỗi flavor hiển thị:

| Thông số | Ý nghĩa |
|---------|---------|
| **Tên** | Tên gói (ví dụ: small, medium, large) |
| **vCPU** | Số nhân CPU ảo được phân bổ |
| **RAM** | Dung lượng bộ nhớ (đơn vị GB) |
| **Root Disk** | Dung lượng ổ đĩa hệ điều hành (đơn vị GB) |

---

## Hướng dẫn chọn Flavor phù hợp

### Theo loại ứng dụng

| Ứng dụng | CPU | RAM | Gợi ý Flavor |
|---------|-----|-----|-------------|
| Web server nhỏ (blog, landing page) | 1 vCPU | 1–2 GB | tiny / small |
| Web server trung bình | 2 vCPU | 4 GB | medium |
| Backend API | 2 vCPU | 4 GB | medium |
| MySQL / PostgreSQL nhỏ | 2 vCPU | 4–8 GB | medium / large |
| MySQL / PostgreSQL lớn | 4 vCPU | 8–16 GB | large / xlarge |
| Cache (Redis, Memcached) | 1 vCPU | 2–4 GB | small / medium |
| Môi trường dev/test | 1–2 vCPU | 2–4 GB | small / medium |

### Nguyên tắc chọn
- **Không đoán quá lớn**: Bắt đầu từ flavor nhỏ hơn, sau đó nâng cấp khi cần
- **RAM quan trọng hơn CPU**: Hầu hết ứng dụng web bị giới hạn bởi RAM hơn CPU
- **Disk**: Chọn đủ cho OS + ứng dụng, dùng **Volume** để mở rộng data

---

## Xem danh sách Flavor

Vào menu **Flavor** trong thanh điều hướng.

Bảng hiển thị: **Tên, vCPU, RAM (GB), Root Disk (GB), Trạng thái**.

Tìm kiếm: Nhập tên flavor vào ô tìm kiếm.

---

## Chọn Flavor khi tạo máy ảo

Trong form tạo máy ảo, phần **"Chọn Flavor"**:
- Hiển thị danh sách các flavor đang Active
- Mỗi mục hiển thị đầy đủ: Tên, vCPU, RAM, Disk
- Nhấn để chọn → thông số tự động cập nhật

---

## Nâng cấp Flavor cho máy ảo đang có

Khi máy ảo cần thêm tài nguyên:
1. **Tắt máy ảo** (Turn Off) — bắt buộc phải tắt trước
2. Vào chi tiết VM → Tìm nút **"Đổi cấu hình"** / **"Upgrade Flavor"**
3. Chọn flavor mới có thông số cao hơn
4. Xác nhận
5. Bật lại máy ảo (Turn On)

> ⚠️ Chỉ có thể chuyển sang flavor **lớn hơn hoặc bằng**. Không thể hạ xuống flavor nhỏ hơn.

---

## Lưu ý quan trọng

- Flavor chỉ do **quản trị viên** tạo/sửa/xóa
- Người dùng thông thường chỉ **xem và chọn** khi tạo VM
- Nếu không thấy flavor phù hợp, hãy gửi **Ticket hỗ trợ** để yêu cầu thêm

---

## Câu hỏi thường gặp

**Q: Flavor tôi muốn không có trong danh sách, phải làm thế nào?**
A: Gửi Ticket hỗ trợ yêu cầu quản trị viên tạo thêm flavor mới với thông số bạn cần.

**Q: Tôi chọn nhầm flavor khi tạo VM, có đổi được không?**
A: Được. Tắt VM rồi dùng tính năng "Đổi cấu hình" để chuyển sang flavor khác.

**Q: Đổi flavor có mất dữ liệu không?**
A: Không. Đổi flavor chỉ thay đổi phân bổ CPU/RAM, dữ liệu trên ổ đĩa không bị ảnh hưởng.

**Q: Sao máy ảo của tôi vẫn chậm dù đã chọn flavor lớn?**
A: Kiểm tra trong VM bằng lệnh `top` hoặc `htop` xem tiến trình nào đang ngốn tài nguyên. Có thể vấn đề không phải ở flavor mà ở cấu hình ứng dụng.
