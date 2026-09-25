---
tags:
  - claude/cowork
aliases:
  - Giới hạn Sử dụng Cowork
  - Use Claude Cowork Safely
  - Khi nào không nên dùng Cowork
up: "[[Claude Cowork]]"
---

# Khi nào KHÔNG nên dùng Cowork

Ranh giới giữa việc dùng Cowork để tăng hiệu suất và rủi ro pháp lý/tuân thủ dữ liệu — 3 trường hợp nên tránh hoặc chỉ dùng có giám sát chặt.

## 1. Quy trình bị kiểm soát nghiêm ngặt (Regulated workflows)

Công việc bắt buộc phải lưu vết thao tác phục vụ kiểm toán (audit trail) thì **không nên** giao cho Cowork — hoạt động của Cowork không tự động ghi vào audit log hay xuất dữ liệu chuẩn kiểm toán. Compliance API có thể trả về lịch sử phiên làm việc, nhưng hiện chỉ dành cho gói **Claude Enterprise** và vẫn ở bản **beta**.

## 2. Việc không thể giao cho đồng nghiệp làm mà không giám sát

Nguyên tắc: **"Claude chuẩn bị — Bạn quyết định xuất bản"** (Claude prepares; you ship).

- Không giao hoàn toàn các việc có tác động trực tiếp ra bên ngoài: gửi tài liệu pháp lý cho đối tác, đăng thông báo công khai, cập nhật thay đổi trực tiếp tới khách hàng.
- Coi Claude như một đồng nghiệp thông minh nhưng mới vào nghề — có thể soạn thảo, tổng hợp, chuẩn bị bản nháp, nhưng người chịu trách nhiệm cuối cùng vẫn là bạn. Đây cũng là lý do nên giữ **Ask before acting** cho các hành động này — xem [[Mô hình Phân quyền (Permissions)]].

## 3. Dữ liệu cá nhân cực kỳ nhạy cảm (Highly sensitive personal data)

Tránh đưa dữ liệu cá nhân nhạy cảm (thông tin y tế, tài chính, danh tính cá nhân...) vào Cowork nếu chưa nằm trong phạm vi được bộ phận IT của doanh nghiệp phê duyệt rõ ràng.

## Tài liệu tham khảo

Tài liệu chính thức **"Use Claude Cowork safely"** hệ thống hoá các hàng rào bảo vệ sẵn có của Cowork và trách nhiệm cá nhân khi cho phép Claude hành động thay mặt mình — nguồn tham khảo nên đọc thêm khi cần quyết định phạm vi cấp quyền cho một use case mới.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Mô hình Phân quyền (Permissions)]], [[Viết Prompt An toàn (Tránh Hiểu Lầm)]], [[Giám sát Real-time khi Cowork chạy (Watch the Run)]], [[Bản chất & Triết lý]]
