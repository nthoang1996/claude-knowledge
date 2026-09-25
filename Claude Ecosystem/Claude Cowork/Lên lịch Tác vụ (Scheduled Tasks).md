---
tags:
  - claude/cowork
aliases:
  - Scheduled Tasks
  - Lên lịch Tác vụ
up: "[[Claude Cowork]]"
---

# Lên lịch Tác vụ (Scheduled Tasks)

Cho phép thiết lập một quy trình **một lần** rồi để Claude tự động chạy lại theo chu kỳ, thay vì phải gõ lệnh thủ công mỗi lần — biến Cowork từ "trợ lý nhận lệnh trực tiếp" thành một hệ thống tự động hóa công việc ngầm.

## 2 cách thiết lập

1. **Tạo mới (start fresh):** `/schedule` → mô tả công việc + tần suất → Claude dự thảo prompt → xem lại và chấp nhận.
2. **Chuyển đổi từ việc đã làm (khuyên dùng):** chạy tác vụ trong Cowork một lần → kiểm tra kết quả đúng ý → gõ `/schedule` để biến chính quy trình đó thành tác vụ lặp định kỳ.

Tần suất: hourly, daily, weekdays, hoặc manual.

## Desktop vs Cloud (Beta)

Nơi khởi tạo tác vụ quyết định nơi nó chạy — tạo ở Desktop thì ở lại Desktop, tạo ở Cloud thì ở lại Cloud.

| Tiêu chí | Desktop App | Cloud (Beta) |
| --- | --- | --- |
| Điều kiện chạy | Máy tính phải bật, app Desktop đang mở | Chạy trên server Cloud, không cần bật máy |
| Khi máy tắt/gập | Tác vụ hoãn, tự chạy lại khi bật máy | Vẫn chạy đúng giờ 100% |
| Nguồn dữ liệu | Thư mục local + Connectors | Chỉ Cloud Connectors (Drive, Slack, Gmail...), không đọc file local |

## Ví dụ ứng dụng

- **Tổng kết chiều Thứ Sáu** (weekdays/Friday) — quét sản phẩm hoàn thành trong tuần + tóm tắt quyết định trên Slack/Teams → ra bản tổng kết tuần kèm việc cần làm tuần tới.
- **Dồn số liệu hàng tháng** (monthly, ngày 1) — trích số liệu thô từ Excel/Sheets, tổng hợp KPI tháng cũ → xuất báo cáo hoàn chỉnh, không cần tính tay.
- **Bản tin đầu ngày** (weekdays, sáng) — đối chiếu Calendar với ghi chú họp cũ + email gần nhất → tóm tắt bối cảnh trước từng cuộc họp trong ngày.

| Tác vụ | Nguồn dữ liệu | Đầu ra |
| --- | --- | --- |
| Friday Review | Slack/Teams + thư mục làm việc | Tổng kết tuần & kế hoạch tuần tới |
| Monthly Metrics | Spreadsheets (Excel/Sheets) | Báo cáo số liệu tháng |
| Morning Briefing | Calendar + Email + ghi chú local | Tóm tắt bối cảnh trước họp |

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Tiêu chí chọn Task phù hợp (3 Patterns)]], [[Thêm & Quản lý Connectors (Cowork)]], [[Cloud Cowork (Mobile & Web)]], [[Viết Prompt An toàn (Tránh Hiểu Lầm)]]
