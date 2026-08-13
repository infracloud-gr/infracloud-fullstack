# Firewall (Tường lửa)

## Firewall là gì?

**Firewall** (tường lửa) là lớp bảo vệ kiểm soát lưu lượng mạng vào/ra khỏi máy ảo. Hãy hình dung như bảo vệ đứng ở cửa tòa nhà — chỉ những người có trong danh sách mới được vào, còn lại đều bị chặn.

Firewall hoạt động dựa trên các **Rule** (quy tắc): mỗi rule xác định loại kết nối nào được **cho phép** hoặc **từ chối**.

---

## Firewall Rule là gì?

Mỗi **Firewall Rule** định nghĩa một quy tắc lọc gói tin dựa trên:

| Thông số | Ý nghĩa | Ví dụ |
|---------|---------|-------|
| **Giao thức** | Loại kết nối | TCP, UDP, ICMP |
| **Dải cổng** | Cổng áp dụng | `22-22` (chỉ port 22), `80-443` (port 80 đến 443) |
| **IP nguồn** | Cho phép từ IP nào | `0.0.0.0/0` (tất cả), `192.168.1.0/24` (một dải cụ thể) |
| **Hành động** | Cho phép hay chặn | ALLOW (cho phép), DENY (chặn) |
| **Ưu tiên** | Thứ tự kiểm tra | Số nhỏ = kiểm tra trước |

### Hiểu về IP nguồn
- `0.0.0.0/0` = Cho phép **tất cả địa chỉ IP** trên thế giới
- `203.0.113.0/24` = Chỉ cho phép IP từ dải `203.0.113.0` đến `203.0.113.255`
- `203.0.113.50/32` = Chỉ cho phép đúng một IP duy nhất `203.0.113.50`

---

## Luồng tạo và sử dụng Firewall

```
1. Tạo Firewall (đặt tên)
2. Thêm các Rule vào Firewall
3. Áp dụng (Attach) Firewall vào máy ảo
4. Khi cần thay đổi rule → Thêm/Xóa rule → Refresh để áp dụng
```

---

## Tạo Firewall mới

### Bước 1: Mở form tạo
Vào menu **Firewall** → Nhấn **"Tạo Firewall"**.

### Bước 2: Điền thông tin
- **Tên**: Đặt tên mô tả mục đích. Ví dụ: `fw-web-server`, `fw-database`, `fw-dev`
- **Mô tả**: Ghi chú thêm về firewall này (tùy chọn)

### Bước 3: Xác nhận
Nhấn **"Tạo"**. Firewall được tạo nhưng chưa có rule nào.

---

## Thêm Rule vào Firewall

Sau khi tạo Firewall, vào chi tiết Firewall để thêm rule.

### Bước 1: Vào chi tiết Firewall
Từ danh sách → Nhấn tên firewall.

### Bước 2: Thêm Rule mới
Nhấn **"Thêm Rule"** và điền:

**Giao thức:**
- `TCP` — Dùng cho hầu hết dịch vụ: SSH, HTTP, HTTPS, database
- `UDP` — Dùng cho DNS, VPN, streaming
- `ICMP` — Dùng để ping kiểm tra kết nối
- `ANY` — Áp dụng cho tất cả giao thức

**Dải cổng:**
- Nhập `22-22` để chỉ áp dụng cho port 22
- Nhập `80-443` để áp dụng cho tất cả port từ 80 đến 443
- Để trống nếu giao thức là ICMP (không có cổng)

**IP nguồn:**
- `0.0.0.0/0` để cho phép/chặn từ tất cả mọi nơi
- Nhập dải IP cụ thể để giới hạn nguồn

**Hành động:** `ALLOW` hoặc `DENY`

**Ưu tiên (Priority):** Số nguyên — số **nhỏ hơn** được kiểm tra **trước**
- Ví dụ: Rule priority 10 được kiểm tra trước rule priority 100

### Bước 3: Lưu rule

---

## Các Rule phổ biến và cách thiết lập

### Template: Firewall cho Web Server (HTTP/HTTPS + SSH)

| Ưu tiên | Giao thức | Cổng | IP nguồn | Hành động |
|---------|---------|------|---------|----------|
| 10 | TCP | 22 | 0.0.0.0/0 | ALLOW |
| 20 | TCP | 80 | 0.0.0.0/0 | ALLOW |
| 30 | TCP | 443 | 0.0.0.0/0 | ALLOW |
| 40 | ICMP | - | 0.0.0.0/0 | ALLOW |
| 9999 | ANY | - | 0.0.0.0/0 | DENY |

Quy tắc cuối cùng (`DENY ANY`) chặn tất cả những gì không được cho phép rõ ràng.

### Template: Firewall cho Database (chỉ cho phép từ máy chủ ứng dụng)

| Ưu tiên | Giao thức | Cổng | IP nguồn | Hành động |
|---------|---------|------|---------|----------|
| 10 | TCP | 22 | 203.0.113.50/32 | ALLOW |
| 20 | TCP | 3306 | 10.0.1.0/24 | ALLOW |
| 9999 | ANY | - | 0.0.0.0/0 | DENY |

Chỉ cho phép SSH từ một IP quản trị cụ thể và MySQL từ dải IP nội bộ.

---

## Áp dụng Firewall vào máy ảo (Attach)

### Cách 1: Từ trang chi tiết máy ảo
1. Vào trang chi tiết VM → Tab **Firewall**
2. Nhấn **"Áp dụng Firewall"** (Attach)
3. Chọn firewall muốn áp dụng từ danh sách
4. Xác nhận

### Cách 2: Từ trang chi tiết Firewall
1. Vào trang chi tiết Firewall → Xem danh sách VM đang dùng
2. Có thể thêm/tháo VM từ đây

---

## Tháo Firewall khỏi máy ảo (Detach)

1. Vào chi tiết VM → Tab **Firewall**
2. Tìm firewall muốn tháo
3. Nhấn **"Tháo"** (Detach) → Xác nhận

> Khi tháo firewall, máy ảo không còn bị bảo vệ bởi firewall đó. Hãy chắc chắn có firewall khác thay thế nếu cần.

---

## Làm mới Firewall (Refresh)

Sau khi **thêm hoặc xóa rule**, bạn cần **Refresh** để áp dụng thay đổi lên máy ảo:

1. Vào chi tiết VM → Tab Firewall
2. Tìm firewall đang áp dụng
3. Nhấn **"Refresh"** → Xác nhận

---

## Xóa Firewall Rule

1. Vào chi tiết Firewall → Danh sách Rules
2. Tìm rule muốn xóa
3. Nhấn icon xóa → Xác nhận
4. Nhớ **Refresh** firewall trên các VM đang dùng

---

## Xóa Firewall

1. Đảm bảo firewall đã được Detach khỏi tất cả VM
2. Từ danh sách Firewall → Nhấn icon xóa → Xác nhận

---

## Câu hỏi thường gặp

**Q: Tôi không SSH được vào máy ảo, có phải do Firewall không?**
A: Có thể. Kiểm tra Firewall của VM có rule `ALLOW TCP port 22` chưa. Nếu có rule `DENY ALL` ở cuối mà không có rule ALLOW port 22 trước đó, SSH sẽ bị chặn.

**Q: Một máy ảo có thể có nhiều Firewall không?**
A: Có, một VM có thể áp dụng nhiều Firewall cùng lúc. Các rules từ tất cả firewall sẽ được kết hợp lại.

**Q: Tôi thêm rule mới nhưng vẫn không có tác dụng?**
A: Sau khi thêm rule, bạn cần nhấn **Refresh** trong tab Firewall của VM để áp dụng thay đổi.

**Q: Rule nào kiểm tra trước khi có nhiều rule?**
A: Rule có **Priority nhỏ hơn** được kiểm tra trước. Nếu một rule khớp, các rule sau không cần kiểm tra nữa.

**Q: Tôi nên đặt rule DENY ALL ở priority bao nhiêu?**
A: Đặt priority lớn nhất (ví dụ: 9999 hoặc 1000) để nó luôn là rule cuối cùng được kiểm tra.

**Q: Firewall của tôi đang áp dụng cho VM nào?**
A: Vào chi tiết Firewall, phần dưới có danh sách tất cả VM đang áp dụng firewall này.

**Q: Xóa Firewall thì các VM đang dùng có bị ảnh hưởng không?**
A: Hệ thống thường tự động detach firewall khỏi các VM khi xóa. Tuy nhiên, khuyến nghị Detach thủ công trước để đảm bảo an toàn.
