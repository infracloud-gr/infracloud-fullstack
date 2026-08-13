# 📚 Infra Cloud — Tài liệu người dùng cuối (RAG)

Tài liệu này được viết **dành cho người dùng cuối** — tập trung vào giải thích khái niệm và hướng dẫn thao tác trực tiếp trên giao diện web. Mỗi folder là một topic độc lập, tối ưu để build vector database cho RAG.

---

## Cấu trúc thư mục

```
docs/rag/
├── 00_overview/
│   └── system_overview.md        # Hệ thống là gì, các module, luồng sử dụng ban đầu
│
├── 01_authentication/
│   └── authentication.md         # Đăng nhập, quên mật khẩu, phiên làm việc
│
├── 02_dashboard/
│   └── dashboard.md              # Trang tổng quan: đọc thống kê, biểu đồ
│
├── 03_virtual_machine/
│   └── virtual_machine.md        # Tạo/bật/tắt/nâng cấp/xóa/kết nối máy ảo
│
├── 04_compute_server/
│   └── compute_server.md         # Xem máy chủ vật lý, đọc tài nguyên còn lại
│
├── 05_volume/
│   └── volume.md                 # Tạo/gắn/tháo/mở rộng ổ đĩa
│
├── 06_cloud_image/
│   └── cloud_image.md            # Chọn hệ điều hành khi tạo máy ảo
│
├── 07_network/
│   └── network.md                # Tạo mạng ảo và mạng con (Subnet)
│
├── 08_router/
│   └── router.md                 # Router ảo và Port Forwarding để truy cập từ ngoài
│
├── 09_firewall/
│   └── firewall.md               # Tạo tường lửa, thêm rule, áp dụng vào VM
│
├── 10_ssh_key/
│   └── ssh_key.md                # Tạo khóa SSH, đăng nhập máy ảo không cần mật khẩu
│
├── 11_snapshot/
│   └── snapshot.md               # Chụp ảnh backup VM, khôi phục khi có sự cố
│
├── 12_flavor/
│   └── flavor.md                 # Chọn gói cấu hình CPU/RAM/Disk phù hợp
│
├── 13_ticket_support/
│   └── ticket_support.md         # Gửi yêu cầu hỗ trợ kỹ thuật
│
└── 14_activity_log/
    └── activity_log.md           # Xem lịch sử thao tác, điều tra sự cố
```

---

## Gợi ý cấu hình RAG

### Chunk size
- **Chunk size**: 512–800 tokens
- **Overlap**: 100–150 tokens
- Mỗi `##` section có thể là một chunk tốt

### Metadata cho mỗi chunk
```json
{
  "source_file": "docs/rag/03_virtual_machine/virtual_machine.md",
  "topic": "virtual_machine",
  "section": "Tạo máy ảo mới",
  "language": "vi",
  "audience": "end_user"
}
```

---

## Câu hỏi mẫu RAG có thể trả lời

- "Làm thế nào để tạo máy ảo?"
- "Tôi không SSH được vào máy ảo, làm thế nào?"
- "Snapshot là gì và khi nào nên dùng?"
- "Làm sao để truy cập máy ảo từ internet?"
- "Tôi nên chọn flavor nào cho web server?"
- "Private key SSH bị mất thì làm gì?"
- "Port Forwarding dùng để làm gì?"
- "Máy ảo đang Pending mà không chuyển sang Running?"
- "Cách gắn thêm ổ đĩa vào máy ảo?"
- "Firewall rule DENY ALL là gì?"
- "Tôi muốn backup máy ảo trước khi cập nhật?"
- "Nhật ký hoạt động dùng để làm gì?"
- "Ai đã xóa máy ảo của tôi?"
- "Làm thế nào để mở rộng ổ đĩa?"
- "Network và Subnet khác nhau như thế nào?"

---

## Cập nhật tài liệu

Cập nhật lần cuối: **2026-08-13**. Khi có tính năng mới, cập nhật file tương ứng và rebuild vector database.
