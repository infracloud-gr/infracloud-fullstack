# Volume (Ổ đĩa mở rộng)

## Volume là gì?

**Volume** là ổ đĩa ảo có thể đính kèm vào máy ảo để tăng thêm không gian lưu trữ. Hãy hình dung giống như bạn cắm thêm một ổ cứng ngoài vào máy tính — bạn có thể gắn vào, tháo ra, và chuyển sang máy khác.

### Sự khác biệt: Root Disk vs Volume

| | Root Disk (Ổ đĩa hệ điều hành) | Volume (Ổ đĩa mở rộng) |
|--|-------------------------------|------------------------|
| **Tạo lúc nào** | Tự động khi tạo VM | Tạo riêng, gắn thủ công |
| **Chứa gì** | Hệ điều hành + ứng dụng | Dữ liệu người dùng |
| **Có thể tháo không** | Không | Có |
| **Khi xóa VM** | Bị xóa theo VM | Vẫn còn (nếu đã detach trước) |

---

## Trạng thái Volume

| Trạng thái | Ý nghĩa |
|-----------|---------|
| **Available** | Chưa gắn vào VM nào, sẵn sàng sử dụng |
| **In Use** | Đang được gắn vào một máy ảo |
| **Pending** | Đang tạo hoặc đang xử lý |
| **Error** | Gặp lỗi |

---

## Xem danh sách Volume

Vào menu **Volume** trong thanh điều hướng.

Danh sách hiển thị: **Tên, Mô tả, Dung lượng (GB), Loại (HDD/SSD), Máy ảo đính kèm, Trạng thái**.

**Lọc danh sách:**
- Lọc theo trạng thái: Available / In Use
- Lọc theo máy ảo cụ thể (xem volume nào đang gắn vào VM nào)

---

## Tạo Volume mới

### Bước 1: Mở form tạo
Từ trang Volume, nhấn nút **"Tạo Volume"**.

### Bước 2: Điền thông tin

**Tên**: Đặt tên mô tả nội dung lưu trữ. Ví dụ: `data-mysql`, `backup-files`, `media-storage`

**Mô tả**: Ghi chú mục đích sử dụng (tùy chọn)

**Dung lượng (GB)**: Nhập dung lượng cần thiết.
- Ổ đĩa sau khi tạo chỉ có thể **tăng**, không thể giảm
- Ước tính nhu cầu và thêm ~20% dư để tránh hết đĩa sớm

**Loại ổ đĩa:**
- **HDD**: Rẻ hơn, phù hợp lưu trữ dữ liệu ít cần truy cập thường xuyên
- **SSD**: Nhanh hơn, phù hợp database, ứng dụng cần I/O cao

### Bước 3: Xác nhận
Nhấn **"Tạo"**. Volume sẽ ở trạng thái **Pending** vài giây rồi chuyển sang **Available**.

---

## Gắn Volume vào máy ảo (Attach)

### Cách 1: Từ trang chi tiết máy ảo
1. Vào chi tiết máy ảo → Tab **Volume**
2. Nhấn **"Gắn Volume"** (Attach Volume)
3. Chọn volume đang ở trạng thái **Available** từ danh sách
4. Xác nhận → Volume được gắn vào VM

### Cách 2: Từ trang danh sách Volume
1. Tìm volume muốn gắn (phải đang Available)
2. Nhấn nút **Attach**
3. Chọn máy ảo muốn gắn vào
4. Xác nhận

### Sau khi Attach — Cần làm gì trong máy ảo?
Volume được gắn vào nhưng **chưa có thể dùng ngay** — bạn cần mount ổ đĩa trong hệ điều hành:

```bash
# Kiểm tra ổ đĩa mới (ví dụ: /dev/vdb)
lsblk

# Định dạng ổ đĩa (chỉ làm lần đầu)
mkfs.ext4 /dev/vdb

# Tạo thư mục mount
mkdir /data

# Mount ổ đĩa
mount /dev/vdb /data

# Để tự động mount khi khởi động (thêm vào /etc/fstab)
echo '/dev/vdb /data ext4 defaults 0 0' >> /etc/fstab
```

---

## Tháo Volume khỏi máy ảo (Detach)

Trước khi tháo, hãy **unmount ổ đĩa trong máy ảo** để tránh mất dữ liệu:
```bash
# Trong máy ảo
umount /data
```

Sau đó:
1. Vào chi tiết máy ảo → Tab **Volume**
2. Tìm volume muốn tháo
3. Nhấn **"Tháo"** (Detach)
4. Xác nhận → Volume trở về trạng thái **Available**

---

## Mở rộng dung lượng Volume (Resize)

Khi ổ đĩa sắp đầy, bạn có thể tăng dung lượng:
1. Từ danh sách Volume, tìm volume muốn mở rộng
2. Nhấn nút **"Resize"** hoặc **"Mở rộng"**
3. Nhập dung lượng mới (phải lớn hơn dung lượng hiện tại)
4. Xác nhận

> **Sau khi resize, cần mở rộng phân vùng trong máy ảo:**
> ```bash
> # Kiểm tra (ví dụ: /dev/vdb)
> resize2fs /dev/vdb
> ```

---

## Xóa Volume

Volume chỉ có thể xóa khi ở trạng thái **Available** (chưa gắn vào VM nào).

1. Đảm bảo volume đã được Detach
2. Từ danh sách, nhấn icon xóa
3. Xác nhận xóa

⚠️ **Xóa volume sẽ mất toàn bộ dữ liệu trên đó — không thể khôi phục!**

---

## Câu hỏi thường gặp

**Q: Tôi gắn volume vào VM nhưng trong máy ảo không thấy ổ đĩa mới?**
A: Cần mount ổ đĩa trong hệ điều hành. Dùng lệnh `lsblk` để xem ổ đĩa mới, sau đó mount như hướng dẫn trên.

**Q: Tôi có thể gắn một volume vào nhiều VM cùng lúc không?**
A: Không. Mỗi volume chỉ có thể gắn vào **1 VM** tại một thời điểm. Muốn chia sẻ dữ liệu, hãy dùng giải pháp chia sẻ file (NFS, Samba...) bên trong máy ảo.

**Q: Khi tôi xóa máy ảo, volume dữ liệu có bị xóa theo không?**
A: **Root Disk (ổ đĩa hệ điều hành)** bị xóa theo. Các **Volume đã detach** vẫn còn nguyên. Để an toàn, hãy detach volume trước khi xóa VM.

**Q: Volume HDD hay SSD nên dùng cho database?**
A: Nên dùng **SSD** cho database (MySQL, PostgreSQL...) vì I/O nhanh hơn nhiều. HDD phù hợp để lưu trữ file backup hoặc log ít truy cập.

**Q: Tôi đã resize volume nhưng trong máy ảo vẫn thấy dung lượng cũ?**
A: Cần chạy lệnh `resize2fs /dev/vdX` trong máy ảo để hệ điều hành nhận diện phần dung lượng mới.
