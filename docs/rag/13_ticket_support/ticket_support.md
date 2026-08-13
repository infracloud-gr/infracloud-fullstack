# Ticket Hỗ trợ (Support Ticket)

## Ticket là gì?

**Ticket** là yêu cầu hỗ trợ kỹ thuật bạn gửi đến đội ngũ hỗ trợ của hệ thống. Khi gặp vấn đề không tự giải quyết được, tạo ticket là cách nhanh nhất để nhận được trợ giúp.

Mỗi ticket có thể có thảo luận qua lại (comment) giữa bạn và kỹ thuật viên, giúp giải quyết vấn đề hiệu quả.

---

## Trạng thái Ticket

| Trạng thái | Màu | Ý nghĩa |
|-----------|-----|---------|
| **Mở (Open)** | 🔵 Xanh | Mới tạo, chờ kỹ thuật viên xem |
| **Đang xử lý (In Progress)** | 🟡 Vàng | Kỹ thuật viên đang làm việc |
| **Đã giải quyết (Resolved)** | 🟢 Xanh lá | Vấn đề đã được xử lý |
| **Đã đóng (Closed)** | ⚫ Xám | Ticket hoàn tất |
| **Từ chối (Rejected)** | 🔴 Đỏ | Yêu cầu không được chấp nhận |

---

## Mức độ ưu tiên (Priority)

| Mức | Dùng khi nào |
|-----|-------------|
| **Thấp (Low)** | Câu hỏi thông thường, không ảnh hưởng hoạt động |
| **Trung bình (Medium)** | Vấn đề cần giải quyết trong ngày |
| **Cao (High)** | Dịch vụ bị gián đoạn, ảnh hưởng nghiêm trọng |
| **Nghiêm trọng (Critical)** | Toàn bộ hệ thống sập, cần xử lý ngay lập tức |

---

## Xem danh sách Ticket

Vào menu **Ticket** trong thanh điều hướng.

Bảng hiển thị: **Tiêu đề, Danh mục, Độ ưu tiên, Trạng thái, Ngày tạo**.

**Lọc danh sách:**
- Lọc theo **Trạng thái**: Xem ticket đang mở, đang xử lý...
- Lọc theo **Danh mục**: Kỹ thuật, Thanh toán...
- Tìm kiếm theo **Tiêu đề**

---

## Tạo Ticket mới

### Bước 1: Mở form tạo
Từ trang danh sách Ticket → Nhấn **"Tạo Ticket"**.

### Bước 2: Điền thông tin

**Tiêu đề (Bắt buộc)**: Mô tả ngắn gọn vấn đề trong 1 câu
- Tốt: `"Không SSH được vào VM web-server-01"`
- Tốt: `"VM bị lỗi sau khi nâng cấp Nginx"`
- Tránh: `"Lỗi"`, `"Giúp với"`, `"Vấn đề"`

**Danh mục:**
- **Kỹ thuật (Technical)**: Vấn đề về VM, mạng, storage...
- **Thanh toán (Billing)**: Hóa đơn, chi phí
- **Câu hỏi chung (General)**: Hỏi về tính năng, hướng dẫn
- **Yêu cầu tính năng (Feature Request)**: Đề xuất tính năng mới
- **Khác (Other)**: Không thuộc các loại trên

**Độ ưu tiên**: Chọn mức phù hợp với mức độ khẩn cấp

**Nội dung (Bắt buộc)**: Mô tả chi tiết vấn đề. Nên bao gồm:
- Vấn đề gặp phải là gì?
- Bạn đã làm gì trước khi gặp lỗi?
- Thông báo lỗi cụ thể (nếu có)
- Tên/ID của tài nguyên liên quan (VM, Network...)
- Đã thử các cách giải quyết nào rồi?

**Ảnh đính kèm (Tùy chọn)**: Upload screenshot về lỗi
- Nhấn **"Upload ảnh"** hoặc kéo thả file ảnh vào khung
- Có thể đính kèm nhiều ảnh

### Bước 3: Xác nhận
Nhấn **"Gửi"**. Ticket được tạo với trạng thái **Mở**.

---

## Xem chi tiết và theo dõi Ticket

Nhấn vào tiêu đề ticket để xem chi tiết:
- Thông tin ticket đầy đủ
- Timeline trạng thái (mở → đang xử lý → giải quyết)
- **Thread comment**: Lịch sử trao đổi giữa bạn và kỹ thuật viên

---

## Trả lời Comment từ kỹ thuật viên

Khi kỹ thuật viên hỏi thêm thông tin hoặc phản hồi:
1. Vào chi tiết ticket
2. Đọc comment mới nhất ở dưới cùng
3. Nhập trả lời vào ô **"Thêm bình luận"**
4. Có thể upload thêm ảnh trong comment
5. Nhấn **"Gửi"**

---

## Viết Ticket hiệu quả — Ví dụ mẫu

### ❌ Ticket viết kém:
> **Tiêu đề:** Lỗi
> **Nội dung:** VM bị lỗi, không dùng được

### ✅ Ticket viết tốt:
> **Tiêu đề:** Không SSH được vào VM web-server-01 (IP: 10.0.1.5) sau khi thêm Firewall
>
> **Nội dung:**
> Sau khi tôi áp dụng Firewall "fw-strict" vào VM "web-server-01" (ID: abc-123), tôi không còn SSH được nữa.
>
> **Thông báo lỗi:** `ssh: connect to host 10.0.1.5 port 22: Connection refused`
>
> **Đã thử:**
> - Kiểm tra VM đang Running ✓
> - Ping IP vẫn thành công ✓
> - Kiểm tra Firewall thấy có rule DENY ALL priority 10 (có thể đây là vấn đề?)
>
> **Ảnh đính kèm:** Screenshot Firewall rules đang cấu hình
>
> Mong được hỗ trợ xem Firewall có đúng không, hoặc cách debug thêm.

---

## Xóa Ticket

1. Từ danh sách Ticket → Tìm ticket muốn xóa
2. Nhấn icon xóa → Xác nhận

> Thường chỉ nên xóa các ticket tạo nhầm. Ticket đã giải quyết nên để ở trạng thái Closed để lưu lịch sử.

---

## Câu hỏi thường gặp

**Q: Bao lâu thì ticket được phản hồi?**
A: Phụ thuộc vào priority và thời điểm gửi. Ticket Critical thường được xử lý trong vài giờ. Ticket thông thường trong 1 ngày làm việc.

**Q: Tôi muốn cập nhật thêm thông tin cho ticket đã gửi?**
A: Vào chi tiết ticket và thêm comment mới với thông tin bổ sung.

**Q: Kỹ thuật viên nói vấn đề đã giải quyết nhưng tôi vẫn còn vấn đề?**
A: Comment vào ticket mô tả chi tiết vấn đề vẫn còn. Đừng tạo ticket mới — tiếp tục trong ticket cũ để kỹ thuật viên có đầy đủ ngữ cảnh.

**Q: Tôi cần gửi file log (file text lớn) cho kỹ thuật viên?**
A: Hiện chỉ hỗ trợ upload ảnh. Hãy chụp màn hình phần quan trọng nhất của log, hoặc nhờ kỹ thuật viên hướng dẫn cách khác.
