---
tags:
  - claude/cowork
aliases:
  - Cloud Cowork
  - Run Cowork in the cloud
up: "[[Claude Cowork]]"
---

# Cloud Cowork (Mobile & Web)

Giải quyết bài toán **kích hoạt và quản lý công việc khi không ngồi ở bàn làm việc**: giao việc từ Claude Mobile App, theo dõi qua trình duyệt Web trên bất kỳ máy nào, nhận sản phẩm khi quay lại bàn làm việc. Phiên chạy trên máy chủ Cloud của Anthropic nên không cần bật máy tính, không lo máy ngủ hay mất kết nối.

> Đang trong giai đoạn **Beta** — độ khả dụng phụ thuộc gói tài khoản (Pro/Team/Enterprise) và chính sách cấu hình của Organization Admin.

## 2 quy tắc cốt lõi

1. **Phạm vi dữ liệu** — Cloud session chỉ truy cập được **Cloud Connectors** đã kết nối (Drive, Slack, Gmail, Zendesk, Salesforce...), **không** đọc/ghi được thư mục local trên máy cá nhân. Việc cần file chỉ lưu cục bộ bắt buộc phải khởi tạo từ Desktop App.
2. **Session Binding (cố định môi trường)** — khởi tạo ở đâu thì phiên chạy cố định ở đó (Mobile/Web → Cloud; Desktop App → máy local). Không đổi môi trường giữa chừng; muốn đổi phải tạo session mới.

## So sánh Cloud Cowork vs Desktop Cowork

| Tiêu chí | Cloud Cowork (Mobile/Web) | Desktop Cowork |
| --- | --- | --- |
| Nơi thực thi | Máy chủ Cloud của Anthropic | Máy tính local |
| Trạng thái máy tính | Tắt/gập màn hình vẫn chạy | Phải bật máy & mở app |
| Quyền truy cập file | Chỉ Cloud Connectors | File local + Cloud |
| Phê duyệt | Push notification trên Mobile/Web | Pop-up trực tiếp trên Desktop |
| Phù hợp nhất | Thao tác qua API Connectors | Xử lý file trực tiếp |

## Ví dụ

- Đang di chuyển: nhớ ra cần bản tóm tắt ticket hỗ trợ quý trước — giao qua điện thoại, Cowork quét dữ liệu từ công cụ Support (Cloud), viết tóm tắt 1 trang trước khi đến văn phòng.
- Đi công tác: nhận điều khoản hợp đồng mới — dùng điện thoại yêu cầu đối chiếu với hợp đồng năm ngoái trên Google Drive, liệt kê thay đổi.

## Thực hành tốt

- Đồng bộ sẵn thư mục dự án quan trọng lên Drive/OneDrive/SharePoint để Cloud Cowork truy cập được ngay khi giao việc từ xa.
- Bật push notification cho Claude Mobile App để nhận phê duyệt kịp thời khi Cowork chạy đến bước nhạy cảm (gửi mail, chia sẻ file) — xem cơ chế phê duyệt tại [[Mô hình Phân quyền (Permissions)]].

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Lên lịch Tác vụ (Scheduled Tasks)]], [[Thêm & Quản lý Connectors (Cowork)]], [[Mô hình Phân quyền (Permissions)]], [[Tích hợp môi trường (4 trụ cột)]]
