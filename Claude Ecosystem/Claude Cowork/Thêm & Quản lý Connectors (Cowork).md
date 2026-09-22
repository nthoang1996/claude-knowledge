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

Với hệ thống nội bộ, trang nhà cung cấp hoặc portal cần đăng nhập (behind a login) mà chưa có Connector API — dùng tiện ích [[Claude in Chrome]] làm cầu nối: đọc nội dung và thao tác trực tiếp trên giao diện trang web đang mở (đọc, bấm nút, điền form) giống như một người dùng thật, thay vì gọi API.

- **Phối hợp trong cùng một hội thoại:** Chrome và Cowork không tách rời — Chrome lo phần thu thập/thao tác trên trình duyệt (tìm kiếm, lấy dữ liệu, điền form trên web), còn Cowork tổng hợp dữ liệu vừa thu thập để tạo sản phẩm cuối (báo cáo, bảng số liệu, tài liệu...).
- **Người dùng luôn kiểm soát:** hành động rủi ro cao hoặc nhạy cảm (gửi email, thanh toán, xóa dữ liệu...) mặc định dừng lại chờ người dùng bấm **Approve** mới thực thi tiếp — xem chi tiết cơ chế phê duyệt ở [[Claude in Chrome]] và [[Mô hình Phân quyền (Permissions)]].

### Ví dụ: gộp Chrome + nhiều Connector trong một lệnh

Bài toán: cần bản tóm tắt 1 trang về các khách hàng đang ở mức cảnh báo vàng/đỏ trước cuộc họp — nhưng dashboard sức khỏe khách hàng nằm sau đăng nhập (không có API), còn dữ liệu bổ sung lại nằm rải rác ở Drive và Slack.

Prompt: *"Mở dashboard sức khỏe khách hàng trên Chrome, lọc các tài khoản vàng/đỏ. Với mỗi tài khoản, lấy hoạt động 30 ngày qua từ Drive và trao đổi gần đây ở kênh #customer-success trên Slack, rồi tạo bản tóm tắt 1 trang để tôi duyệt."*

Cách hệ thống tự chia việc:

1. **Chrome** — mở dashboard, lọc và thu thập danh sách khách hàng vàng/đỏ (vì không có connector cho trang này).
2. **Cowork** — nhận danh sách từ Chrome, tự gọi connector Drive lấy tài liệu và Slack lấy tin nhắn liên quan cho từng khách hàng.
3. **Tổng hợp** — gộp cả 3 nguồn (Web/Drive/Slack) thành một bản tóm tắt hoàn chỉnh, lưu vào thư mục làm việc.

Chỉ **một lần giao việc** duy nhất thay vì tự tay copy-paste/tải file qua lại giữa 3 hệ thống.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Khái niệm nền (giao thức MCP): [[Connectors (MCP)]]
- Xem thêm: [[Tích hợp môi trường (4 trụ cột)]], [[Claude in Chrome]], [[Lên lịch Tác vụ (Scheduled Tasks)]], [[Cloud Cowork (Mobile & Web)]]
