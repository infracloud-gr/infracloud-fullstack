# Router và Port Forwarding

## Router là gì?

**Router** (bộ định tuyến ảo) là cầu nối giữa mạng nội bộ của bạn và thế giới bên ngoài (internet). Nếu máy ảo của bạn chỉ có IP nội bộ (ví dụ `10.0.1.5`), bạn cần Router để truy cập từ internet vào.

## Port Forwarding là gì?

**Port Forwarding** (chuyển tiếp cổng) là cơ chế ánh xạ một cổng trên IP công khai của Router về một cổng cụ thể trên máy ảo nội bộ.

### Ví dụ thực tế

Router có IP công khai: `203.0.113.50`
Máy ảo có IP nội bộ: `10.0.1.5`

| Muốn làm gì | Cấu hình Port Forwarding |
|------------|--------------------------|
| SSH vào máy ảo từ ngoài | `203.0.113.50:2222` → `10.0.1.5:22` |
| Truy cập web server | `203.0.113.50:80` → `10.0.1.5:80` |
| Truy cập HTTPS | `203.0.113.50:443` → `10.0.1.5:443` |

Người dùng bên ngoài SSH vào `203.0.113.50` cổng `2222`, hệ thống tự động chuyển vào máy ảo cổng `22`.

---

## Tạo Router

### Bước 1: Mở form tạo
Vào menu **Router** → Nhấn **"Tạo Router"**.

### Bước 2: Điền thông tin

**Tên Router**: Đặt tên gợi nhớ. Ví dụ: `router-main`, `router-web`, `router-prod`

**Loại Router:**
- **Router nội bộ**: Kết nối các network với nhau trong hệ thống
- **Router ngoài**: Kết nối network nội bộ ra internet/IP công khai

**Network kết nối**: Chọn network mà router sẽ quản lý

### Bước 3: Xác nhận
Nhấn **"Tạo"**.

---

## Cấu hình Port Forwarding

Port Forwarding được cấu hình **theo từng máy ảo**, thường thực hiện trong trang chi tiết VM.

### Bước 1: Vào trang chi tiết máy ảo
Từ danh sách VM → Nhấn vào tên VM muốn cấu hình.

### Bước 2: Vào tab Port Forwarding
Trong trang chi tiết VM, tìm và nhấn tab **"Port Forwarding"**.

### Bước 3: Thêm rule mới
Nhấn **"Thêm Port Forwarding"** và điền:

**Cổng ngoài (External Port)**: Cổng người dùng bên ngoài sẽ kết nối đến
- Thường dùng cổng khác với cổng gốc để tránh nhầm lẫn
- Ví dụ: SSH dùng cổng ngoài `2222` thay vì `22`
- Không được trùng với rule Port Forwarding khác trên cùng router

**Cổng trong (Internal Port)**: Cổng thực sự của dịch vụ trong máy ảo
- SSH: `22`
- HTTP: `80`
- HTTPS: `443`
- MySQL: `3306`

**Giao thức**: `TCP` (cho hầu hết dịch vụ) hoặc `UDP`

### Bước 4: Xác nhận
Nhấn **"Thêm"**. Rule có hiệu lực ngay lập tức.

---

## Xóa Port Forwarding Rule

1. Vào chi tiết VM → Tab Port Forwarding
2. Tìm rule muốn xóa
3. Nhấn icon xóa → Xác nhận

---

## Hướng dẫn SSH vào máy ảo từ ngoài internet

**Điều kiện:** Máy ảo đang Running, Router đã tạo, Firewall đã cho phép port 22.

**Bước 1:** Tạo Port Forwarding rule:
- External Port: `2222` (hoặc số bất kỳ từ 1024-65535)
- Internal Port: `22`
- Protocol: TCP

**Bước 2:** SSH từ máy tính của bạn:
```bash
# Dùng mật khẩu
ssh -p 2222 root@<IP-công-khai-router>

# Dùng SSH Key
ssh -i /đường/dẫn/private-key.pem -p 2222 root@<IP-công-khai-router>
```

---

## Cập nhật tên Router

1. Từ danh sách Router → Tìm router muốn sửa
2. Nhấn icon chỉnh sửa (bút chì)
3. Nhập tên mới → Lưu

---

## Xóa Router

1. Đảm bảo không còn Port Forwarding rules nào trên router
2. Nhấn icon xóa → Xác nhận

---

## Câu hỏi thường gặp

**Q: Tôi đã tạo Port Forwarding nhưng vẫn không SSH được?**
A: Kiểm tra theo thứ tự:
1. Firewall của VM có rule ALLOW TCP port 22 chưa?
2. VM đang ở trạng thái Running chưa?
3. Bạn đang dùng đúng IP công khai của Router chưa?
4. Thử ping IP công khai đó từ máy tính của bạn

**Q: Tôi có thể dùng cổng 80 và 443 cho Port Forwarding không?**
A: Có thể nếu router cho phép. Tuy nhiên, nếu cổng 80 đã được dùng bởi rule khác, bạn phải chọn cổng khác.

**Q: Máy ảo của tôi có bao nhiêu Port Forwarding rules thì được?**
A: Không giới hạn số lượng, nhưng mỗi cổng ngoài (external port) phải là duy nhất trên cùng một router.

**Q: Tôi muốn truy cập web trực tiếp qua IP công khai, cần làm gì?**
A: Tạo Port Forwarding: External Port `80` → Internal Port `80`, và External Port `443` → Internal Port `443`. Sau đó truy cập `http://<IP-công-khai>` từ trình duyệt.

**Q: Khác nhau giữa Router và Firewall là gì?**
A: **Router** điều hướng traffic từ ngoài vào máy ảo (port forwarding). **Firewall** kiểm soát traffic nào được phép vào/ra khỏi máy ảo. Cả hai cùng phối hợp: Router mở đường, Firewall quyết định cho phép hay không.
