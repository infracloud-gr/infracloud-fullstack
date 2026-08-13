# Snapshot (Ảnh chụp backup)

## Snapshot là gì?

**Snapshot** là ảnh chụp nhanh trạng thái ổ đĩa của máy ảo tại một thời điểm cụ thể. Hãy hình dung như tính năng **"Lưu game"** — bạn lưu lại trạng thái hiện tại, và có thể quay lại đúng điểm đó bất cứ lúc nào nếu có sự cố.

### Snapshot vs Backup — Khác nhau như thế nào?

| Tiêu chí | Snapshot | Backup đầy đủ |
|---------|---------|--------------|
| **Tốc độ tạo** | Rất nhanh (vài giây) | Chậm hơn |
| **Mục đích** | Khôi phục nhanh trước thay đổi | Lưu trữ dài hạn |
| **Phù hợp** | Trước khi update, cài phần mềm | Sao lưu định kỳ |

---

## Khi nào nên tạo Snapshot?

✅ Tạo snapshot **trước khi**:
- Cập nhật hệ điều hành (`apt upgrade`, `yum update`)
- Cài đặt phần mềm mới
- Thay đổi cấu hình quan trọng (Nginx, Apache, database...)
- Chạy script có thể làm hỏng hệ thống
- Nâng cấp ứng dụng lên phiên bản mới

---

## Xem danh sách Snapshot

Snapshot được quản lý **theo từng máy ảo**:
1. Vào trang chi tiết máy ảo
2. Nhấn tab **"Snapshot"**
3. Xem danh sách tất cả snapshot của VM đó

Danh sách hiển thị: **Tên snapshot, Mô tả, Ngày tạo, Thao tác**.

---

## Tạo Snapshot

### Bước 1: Vào trang chi tiết máy ảo
Từ danh sách VM → Nhấn vào VM muốn snapshot.

### Bước 2: Chuyển đến tab Snapshot
Nhấn tab **"Snapshot"** trong trang chi tiết VM.

### Bước 3: Tạo Snapshot mới
Nhấn **"Tạo Snapshot"** và điền:

**Tên Snapshot**: Đặt tên mô tả rõ ràng thời điểm và lý do
- Tốt: `2025-06-15-truoc-khi-update-nginx`
- Tốt: `truoc-nang-cap-app-v2.0`
- Tránh: `snapshot1`, `backup`, `test`

**Mô tả (tùy chọn)**: Ghi chú thêm về lý do tạo snapshot

### Bước 4: Xác nhận
Nhấn **"Tạo"**. Snapshot được tạo ngay lập tức (VM vẫn chạy bình thường).

---

## Khôi phục từ Snapshot (Revert)

Khi có sự cố sau một thay đổi, bạn có thể "quay ngược thời gian" về thời điểm snapshot.

### ⚠️ CẢNH BÁO QUAN TRỌNG
**Revert sẽ xóa toàn bộ thay đổi xảy ra SAU thời điểm snapshot.** Dữ liệu được tạo sau khi snapshot sẽ mất vĩnh viễn. **Không thể hoàn tác.**

### Bước 1: Vào tab Snapshot của VM
### Bước 2: Tìm snapshot muốn khôi phục
### Bước 3: Nhấn **"Revert"** (Khôi phục)
### Bước 4: Đọc kỹ cảnh báo và xác nhận

Sau khi revert, máy ảo sẽ ở trạng thái đúng như thời điểm snapshot được tạo.

---

## Đổi tên Snapshot

1. Trong danh sách snapshot → Tìm snapshot muốn đổi tên
2. Nhấn icon chỉnh sửa (bút chì)
3. Nhập tên/mô tả mới → Lưu

---

## Xóa Snapshot

Khi snapshot không còn cần thiết, nên xóa để tiết kiệm không gian lưu trữ:
1. Trong danh sách snapshot → Tìm snapshot muốn xóa
2. Nhấn icon xóa → Xác nhận

---

## Quy trình thực hành tốt

```
Trước khi làm thay đổi rủi ro:
  1. [Tạo Snapshot] "truoc-khi-<mo-ta-thay-doi>"

Thực hiện thay đổi:
  2. Cập nhật / cài phần mềm / thay đổi config...

Kiểm tra kết quả:
  3. Nếu OK → Xóa snapshot cũ (không cần nữa)
  4. Nếu có vấn đề → Revert về snapshot
```

---

## Câu hỏi thường gặp

**Q: Tôi có thể tạo snapshot khi VM đang chạy không?**
A: Có. VM vẫn chạy bình thường trong khi snapshot được tạo.

**Q: Snapshot chiếm bao nhiêu dung lượng?**
A: Snapshot ban đầu nhỏ, nhưng sẽ tăng dần khi có nhiều thay đổi trong VM. Nên xóa snapshot cũ không dùng.

**Q: Tôi nên giữ bao nhiêu snapshot cho một VM?**
A: Khuyến nghị **3–5 snapshot** gần nhất. Quá nhiều snapshot tốn dung lượng và khó quản lý.

**Q: Snapshot có thể dùng để chuyển dữ liệu sang VM khác không?**
A: Không trực tiếp. Snapshot chỉ dùng để khôi phục VM hiện tại về thời điểm trước đó.

**Q: Tôi revert nhầm và muốn lấy lại dữ liệu sau revert, có được không?**
A: Không. Revert là thao tác **không thể hoàn tác**. Hãy chắc chắn trước khi revert.

**Q: Có thể đặt lịch tự động tạo snapshot không?**
A: Hiện tại hệ thống chưa hỗ trợ tự động. Bạn cần tạo thủ công. Nếu cần tính năng này, hãy gửi Ticket yêu cầu.
