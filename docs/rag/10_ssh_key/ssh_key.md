# SSH Key (Khóa SSH)

## SSH Key là gì?

**SSH Key** là phương thức đăng nhập vào máy ảo bằng **cặp khóa mã hóa** thay vì mật khẩu. Đây là cách đăng nhập **an toàn hơn** và **tiện lợi hơn** — bạn không cần nhớ mật khẩu, và khó bị tấn công hơn so với mật khẩu thông thường.

### Cặp khóa gồm 2 phần

| Phần | Lưu ở đâu | Dùng để làm gì |
|------|-----------|----------------|
| **Public Key (Khóa công khai)** | Hệ thống lưu, cài vào máy ảo | Như ổ khóa gắn trên cửa |
| **Private Key (Khóa bí mật)** | Bạn giữ trên máy tính | Như chìa khóa — dùng để mở |

> **Quy tắc vàng:** Private Key chỉ có **bạn** được giữ. Không bao giờ chia sẻ qua email, chat, hay lưu lên internet.

---

## Tại sao dùng SSH Key thay vì mật khẩu?

- **An toàn hơn**: Hacker không thể brute-force (thử hàng triệu mật khẩu) vì khóa quá phức tạp
- **Tiện lợi hơn**: Không cần nhớ hoặc nhập mật khẩu mỗi lần kết nối
- **Dễ quản lý**: Một private key có thể dùng cho nhiều máy ảo

---

## Xem danh sách SSH Key

Vào menu **SSH Key** trong thanh điều hướng.

Bảng hiển thị: **Tên, Public Key (rút gọn), Ngày tạo, Thao tác**.

---

## Tạo SSH Key mới

### Bước 1: Mở form tạo
Từ trang SSH Key → Nhấn **"Tạo SSH Key"**.

### Bước 2: Nhập tên
Đặt tên mô tả thiết bị bạn đang dùng. Ví dụ:
- `laptop-ca-nhan`
- `may-tinh-van-phong`
- `macbook-nha`

Đặt tên theo thiết bị giúp bạn biết key nào dùng ở đâu.

### Bước 3: Xác nhận tạo
Nhấn **"Tạo"**.

### Bước 4: ⚠️ Lưu Private Key NGAY LẬP TỨC

Sau khi tạo, hệ thống sẽ hiển thị **Private Key** trong một popup/dialog. Đây là **lần duy nhất** bạn có thể thấy private key này.

**Cách lưu private key:**
1. Nhấn **Copy** hoặc **Tải xuống**
2. Lưu vào file trên máy tính, ví dụ: `C:\Users\TenBan\.ssh\infra-key.pem`
3. **KHÔNG** đóng popup cho đến khi đã lưu xong

> ❌ Nếu bạn đóng popup mà chưa lưu → Private Key bị mất vĩnh viễn. Bạn phải tạo SSH Key mới.

---

## Đặt quyền cho file Private Key (Linux/Mac)

Sau khi lưu private key vào file, cần đặt quyền đúng để SSH chấp nhận:

```bash
chmod 600 ~/.ssh/infra-key.pem
```

Trên Windows, file `.pem` thường hoạt động trực tiếp với các phần mềm như PuTTY hoặc terminal có hỗ trợ SSH.

---

## Sử dụng SSH Key khi tạo máy ảo

Khi tạo máy ảo, trong phần **"Chọn SSH Key"**:
- Chọn SSH Key bạn vừa tạo từ dropdown
- Hệ thống sẽ tự động cài public key vào máy ảo
- Sau khi VM chạy, bạn đăng nhập bằng private key

---

## Đăng nhập máy ảo bằng SSH Key

### Trên Linux/Mac (Terminal)
```bash
ssh -i /đường/dẫn/infra-key.pem root@<IP-máy-ảo>

# Ví dụ:
ssh -i ~/.ssh/infra-key.pem root@10.0.1.5

# Nếu dùng Port Forwarding:
ssh -i ~/.ssh/infra-key.pem -p 2222 root@203.0.113.50
```

### Trên Windows (PuTTY)
1. Mở PuTTYgen → Import file `.pem` → Save private key thành `.ppk`
2. Mở PuTTY → Nhập IP và Port
3. Vào Connection → SSH → Auth → Browse chọn file `.ppk`
4. Nhấn Open để kết nối

### Trên Windows (Terminal/PowerShell mới)
```powershell
ssh -i C:\Users\TenBan\.ssh\infra-key.pem root@10.0.1.5
```

---

## Xóa SSH Key

1. Từ danh sách SSH Key → Tìm key muốn xóa
2. Nhấn icon xóa → Xác nhận

> **Lưu ý:** Xóa SSH Key khỏi hệ thống **không** ảnh hưởng đến các máy ảo đã được cài key này — bạn vẫn SSH được bình thường với private key đang giữ. Chỉ là hệ thống không còn lưu key đó nữa.

---

## Câu hỏi thường gặp

**Q: Tôi lỡ đóng popup sau khi tạo SSH Key và chưa lưu private key, làm thế nào?**
A: Không thể lấy lại private key đó. Hãy xóa SSH Key cũ và tạo lại một cái mới.

**Q: Tôi có thể dùng một SSH Key cho nhiều máy ảo không?**
A: Có. Một SSH Key có thể được chọn khi tạo nhiều VM khác nhau.

**Q: Tôi có nhiều máy tính, cần tạo SSH Key riêng cho từng máy không?**
A: Không bắt buộc, nhưng khuyến nghị. Tạo key riêng cho từng thiết bị giúp bạn kiểm soát tốt hơn — nếu thiết bị bị mất cắp, chỉ cần vô hiệu hóa key của thiết bị đó.

**Q: Tôi không chọn SSH Key khi tạo VM, có đăng nhập được không?**
A: Được, nhưng phải dùng mật khẩu root. Mật khẩu này được tạo tự động và có thể xem trong trang chi tiết máy ảo.

**Q: Lỗi "Permission denied (publickey)" khi SSH, làm thế nào?**
A: Kiểm tra: (1) Đường dẫn đến private key có đúng không? (2) Quyền file private key có là 600 không? (3) Đang SSH đến đúng IP không? (4) VM có được chọn SSH Key này khi tạo không?
