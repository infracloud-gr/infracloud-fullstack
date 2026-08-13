# Cloud Image (Hình ảnh hệ điều hành)

## Cloud Image là gì?

**Cloud Image** là các bản hệ điều hành (OS) đã được chuẩn bị sẵn để cài lên máy ảo. Khi tạo máy ảo, bạn chọn một Cloud Image để xác định máy ảo sẽ chạy hệ điều hành nào.

Đây giống như bạn chọn đĩa cài Windows/Ubuntu khi mua máy tính mới — nhưng ở đây hệ thống đã chuẩn bị sẵn các đĩa này cho bạn.

---

## Các hệ điều hành thường có

| Image | Phù hợp với |
|-------|------------|
| **Ubuntu 22.04 LTS** | Web server, ứng dụng Node.js/Python/Java, môi trường dev |
| **Ubuntu 20.04 LTS** | Tương tự Ubuntu 22.04, phiên bản cũ hơn nhưng ổn định |
| **Debian 11** | Server nhẹ, ổn định, phù hợp dịch vụ nhỏ |
| **CentOS 8** | Môi trường doanh nghiệp, tương thích RedHat |
| **Windows Server** | Ứng dụng chạy trên nền tảng Windows |

> Danh sách image thực tế phụ thuộc vào những gì quản trị viên đã cài sẵn trong hệ thống.

---

## Xem danh sách Cloud Image

Vào menu **Cloud Image** trong thanh điều hướng.

Bảng hiển thị: **Tên image, Mô tả, Trạng thái, Ngày tạo**.

Tìm kiếm: Nhập tên hệ điều hành vào ô tìm kiếm.

---

## Chọn Cloud Image khi tạo máy ảo

Khi tạo máy ảo, bạn sẽ thấy dropdown **"Chọn hệ điều hành"** (Cloud Image):
- Chỉ các image đang **Active** mới hiển thị để chọn
- Đọc kỹ tên và mô tả để chọn đúng hệ điều hành
- Sau khi tạo VM, **không thể đổi hệ điều hành** mà không recreate VM

---

## Lưu ý quan trọng

- Chỉ **quản trị viên** mới có thể thêm Cloud Image mới vào hệ thống
- Người dùng thông thường chỉ **xem và chọn** image khi tạo VM
- Nếu bạn cần một hệ điều hành chưa có trong danh sách, hãy gửi **Ticket hỗ trợ** để yêu cầu

---

## Câu hỏi thường gặp

**Q: Tôi muốn đổi hệ điều hành của máy ảo đang chạy, có được không?**
A: Không thể đổi trực tiếp. Bạn cần **Recreate** máy ảo (chọn image mới khi recreate). Tuy nhiên, toàn bộ dữ liệu sẽ mất — hãy backup trước.

**Q: Image nào nên chọn cho web server PHP/Laravel?**
A: **Ubuntu 22.04 LTS** hoặc **Ubuntu 20.04 LTS** là lựa chọn phổ biến và được hỗ trợ tốt.

**Q: Image nào nên chọn cho MySQL/PostgreSQL?**
A: Ubuntu hoặc Debian đều ổn cho database. Ưu tiên bản LTS (Long-Term Support) để được hỗ trợ lâu dài.

**Q: Danh sách image của tôi trống, tại sao?**
A: Có thể quản trị viên chưa thêm image nào. Hãy gửi Ticket hỗ trợ để yêu cầu.
