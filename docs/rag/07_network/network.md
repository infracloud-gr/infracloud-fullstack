# Network và Subnet (Mạng ảo)

## Network là gì?

**Network** (mạng ảo) là hệ thống mạng nội bộ dùng để kết nối các máy ảo với nhau. Các máy ảo trong cùng một network có thể giao tiếp trực tiếp với nhau qua địa chỉ IP nội bộ, tương tự như máy tính trong cùng một văn phòng.

## Subnet là gì?

**Subnet** (mạng con) là một dải địa chỉ IP cụ thể nằm trong một Network. Mỗi máy ảo khi được tạo ra phải chọn một Subnet — máy ảo sẽ nhận địa chỉ IP từ dải của Subnet đó.

### Ví dụ minh họa

```
Network: "Mạng-Nội-Bộ" 
  └── Subnet "subnet-web" (dải 10.0.1.0/24)
        ├── VM web-server-01 → IP: 10.0.1.5
        └── VM web-server-02 → IP: 10.0.1.6
  └── Subnet "subnet-db" (dải 10.0.2.0/24)
        └── VM database-01  → IP: 10.0.2.10
```

Trong ví dụ này, `web-server-01` có thể kết nối tới `database-01` qua địa chỉ `10.0.2.10`.

---

## Hiểu về CIDR (Dải địa chỉ IP)

Khi tạo Subnet, bạn cần nhập **CIDR** — đây là cách biểu diễn dải địa chỉ IP.

| CIDR | Số IP có thể dùng | Phù hợp |
|------|-------------------|---------|
| `10.0.1.0/24` | 254 địa chỉ (10.0.1.1 – 10.0.1.254) | Hầu hết trường hợp |
| `10.0.1.0/25` | 126 địa chỉ | Mạng nhỏ |
| `10.0.1.0/28` | 14 địa chỉ | Rất nhỏ |

**Gợi ý cho người mới:** Dùng dải `/24` (ví dụ `10.0.1.0/24`) — đủ dùng cho hầu hết mọi trường hợp.

---

## Tạo Network

### Bước 1: Mở form tạo
Vào menu **Network** → Nhấn **"Tạo Network"**.

### Bước 2: Điền thông tin

**Tên Network**: Đặt tên mô tả. Ví dụ: `mang-san-xuat`, `mang-dev`, `mang-noi-bo`

**Bật NAT**: Tùy chọn cho phép máy ảo trong mạng này truy cập internet
- **Bật NAT**: Máy ảo có thể tải phần mềm, cập nhật hệ điều hành từ internet
- **Tắt NAT**: Mạng hoàn toàn nội bộ, không ra ngoài internet

### Bước 3: Xác nhận
Nhấn **"Tạo"**. Network được tạo ngay lập tức.

---

## Tạo Subnet trong Network

Sau khi tạo Network, bạn cần tạo ít nhất một Subnet để máy ảo có thể kết nối.

### Bước 1: Vào chi tiết Network
Từ danh sách Network, nhấn vào tên network vừa tạo.

### Bước 2: Tạo Subnet
Nhấn nút **"Tạo Subnet"** trong trang chi tiết Network.

### Bước 3: Điền thông tin

**Tên Subnet**: Ví dụ: `subnet-web`, `subnet-backend`, `subnet-db`

**CIDR**: Dải địa chỉ IP. Ví dụ: `10.0.1.0/24`
- Các subnet trong cùng một network không được trùng dải IP
- Mỗi subnet nên dùng dải IP khác nhau (10.0.1.x, 10.0.2.x, 10.0.3.x...)

**Gateway**: Địa chỉ cổng mặc định — thường là địa chỉ đầu tiên của subnet
- Ví dụ với CIDR `10.0.1.0/24` → Gateway là `10.0.1.1`

**Dải DHCP (Bắt đầu – Kết thúc)**: Dải IP sẽ được cấp cho máy ảo
- Ví dụ: Từ `10.0.1.10` đến `10.0.1.250`
- Các IP từ 10.0.1.1 đến 10.0.1.9 để dành cho gateway và dịch vụ hệ thống

### Bước 4: Xác nhận
Nhấn **"Tạo"**.

---

## Xem danh sách Network và Subnet

**Trang danh sách Network:**
- Hiển thị: Tên, Code (mã hệ thống), Trạng thái NAT, Trạng thái
- Nhấn vào network để xem chi tiết

**Trang chi tiết Network:**
- Thông tin network
- Danh sách tất cả Subnet thuộc network này
- Nút tạo Subnet mới

---

## Cập nhật và Xóa Network

**Cập nhật:** Nhấn icon chỉnh sửa → đổi tên network.

**Xóa Network:** 
- Nhấn icon xóa
- Hệ thống sẽ cảnh báo nếu còn subnet hoặc VM trong network
- Phải xóa hết VM và Subnet trước khi xóa Network

---

## Câu hỏi thường gặp

**Q: Tôi nên tạo bao nhiêu Network?**
A: Thông thường 1 network là đủ cho hầu hết trường hợp. Tạo network riêng biệt khi bạn muốn tách biệt hoàn toàn các môi trường (ví dụ: mạng sản xuất tách biệt với mạng phát triển).

**Q: Máy ảo ở hai Subnet khác nhau có liên lạc được với nhau không?**
A: Nếu cùng một Network thì có (thông qua Layer 3 routing). Nếu khác Network thì cần Router để kết nối.

**Q: NAT là gì và tôi có nên bật không?**
A: NAT cho phép máy ảo trong mạng truy cập internet (để tải phần mềm, update...). Nếu máy ảo cần kết nối internet, hãy bật NAT khi tạo Network.

**Q: Tôi nhập CIDR sai, làm thế nào sửa?**
A: Hiện tại subnet không thể sửa CIDR sau khi tạo. Hãy xóa subnet và tạo lại với CIDR đúng (đảm bảo không có VM đang dùng subnet đó).

**Q: Tôi cần subnet mới nhưng quên CIDR nên dùng gì?**
A: Gợi ý đơn giản: Dùng `10.0.1.0/24` cho subnet thứ nhất, `10.0.2.0/24` cho thứ hai, `10.0.3.0/24` cho thứ ba... Mỗi subnet cho 254 máy ảo.
