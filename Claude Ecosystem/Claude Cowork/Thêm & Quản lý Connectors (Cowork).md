---
tags:
  - claude/cowork
aliases:
  - Connectors trong Cowork
  - Quản lý Connectors Cowork
up: "[[Claude Cowork]]"
---

# Thêm & Quản lý Connectors (Cowork)

## Bản chất & cách thiết lập

- **Khái niệm** — connectors là cầu nối API để Claude truy cập dữ liệu trong các ứng dụng đám mây (Cloud Apps).
- **Quy trình dùng** — cấu hình (set up) một lần trong mục *Customize*, sau đó **bật/tắt (toggle)** linh hoạt từng connector tùy tác vụ.
- **Giao tiếp tự nhiên** — connector đã bật thì chỉ cần gọi tên app tự nhiên trong prompt (vd: "Kiểm tra xem đội ngũ nói gì trên Slack về vụ ra mắt").

## Nhóm connector phổ biến

- **Email & Lịch** (Outlook/M365, Gmail, Google Calendar) — trích xuất nội dung họp, viết email phản hồi, tìm chuỗi email cũ.
- **Nhắn tin** (Slack, Microsoft Teams) — tìm lịch sử kênh, tóm tắt thảo luận nhóm.
- **Lưu trữ đám mây** (SharePoint, OneDrive, Google Drive, Box) — truy cập tài liệu/hồ sơ không lưu trên máy local.
- **Quản lý & CRM** (Notion, Salesforce, HubSpot, Asana, Linear) — truy xuất dữ liệu khách hàng, tiến độ dự án thực tế.

## Ví dụ minh họa

Prompt: *"Viết bản cập nhật trạng thái công việc thứ Hai dựa trên luồng thảo luận Slack tuần này, cuộc họp trên Calendar, email đang trao đổi, và file kế hoạch trong Drive."*

- **0/4 connector bật** — Cowork chỉ phân tích được nội dung đã copy/paste hoặc upload thủ công, không tự đọc được 4 nguồn trên.
- **4/4 connector bật** — tự động quét cả 4 ứng dụng, tổng hợp dữ liệu bất đồng bộ, tạo báo cáo hoàn chỉnh.

## Khi app không có Connector: Claude in Chrome

Với hệ thống nội bộ, trang nhà cung cấp hoặc portal cần đăng nhập (behind a login) mà chưa có Connector API — dùng tiện ích [[Claude in Chrome]] làm cầu nối: đọc nội dung và thao tác trực tiếp trên giao diện trang web đang mở.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Khái niệm nền (giao thức MCP): [[Connectors (MCP)]]
- Xem thêm: [[Tích hợp môi trường (4 trụ cột)]], [[Claude in Chrome]]
