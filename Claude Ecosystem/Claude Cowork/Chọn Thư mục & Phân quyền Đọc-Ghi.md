---
tags:
  - claude/cowork
aliases:
  - Chọn Thư mục & Phân quyền Đọc-Ghi
  - Read Write Cowork
up: "[[Claude Cowork]]"
---

# Chọn Thư mục & Phân quyền Đọc/Ghi

## Chọn thư mục làm việc

Nhấn **"Work in a project or folder"** trên thanh prompt để trỏ Cowork vào một thư mục cục bộ.

- **Phạm vi tối thiểu** — chọn thư mục nhỏ nhất chứa đủ tài liệu liên quan dự án (Word, Excel, PDF, PowerPoint...); tránh cấp quyền toàn ổ đĩa hoặc thư mục `Documents` tổng. Thiếu thì thêm thư mục khác sau, không cần cấp thừa từ đầu.
- **Giàu bối cảnh** — Cowork hoạt động tốt nhất khi thư mục có sẵn tài liệu đầu vào: tài liệu tham khảo, file mẫu/template, dữ liệu thô.

## Quyền Đọc & Ghi: Chat vs Cowork

| Tiêu chí | Claude Chat | Claude Cowork |
| --- | --- | --- |
| Quyền truy cập | Chỉ đọc (read-only) | Đọc và ghi (read & write) |
| Thao tác tệp | Đọc file bạn tải lên (uploads) | Mở, chỉnh sửa, tạo file mới, tự sắp xếp/di chuyển file trong thư mục |
| Lưu trữ | Không thể lưu ngược lại máy tính | Tự động lưu sản phẩm đầu ra thẳng vào thư mục làm việc |

## Local Folder vs Cloud Connectors

- **Thư mục Local (Desktop App)** — nơi Claude chỉnh sửa/tạo file trực tiếp; cần xử lý file có sẵn trên máy thì luôn khởi chạy phiên từ Desktop App.
- **Cloud Connectors (Google Drive, Microsoft 365...)** — cơ chế phân quyền riêng; nhiều connector mặc định chỉ **đọc và tìm kiếm (read-and-search only)**, không sửa được file gốc trên cloud.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Cài đặt & Khởi chạy]], [[Tích hợp môi trường (4 trụ cột)]]
