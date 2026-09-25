---
tags:
  - claude/cowork
aliases:
  - Viết Prompt An toàn
  - Safe Prompting
  - Destructive Verbs
up: "[[Claude Cowork]]"
---

# Viết Prompt An toàn (Tránh Hiểu Lầm)

Bên cạnh giới hạn thư mục truy cập ([[Chọn Thư mục & Phân quyền Đọc-Ghi]]) và các guardrail hệ thống ([[Mô hình Phân quyền (Permissions)]]), **cách đặt câu hỏi (prompt)** cũng quyết định độ an toàn — tránh Cowork hiểu lầm ý và thực hiện sai, nhất là với các thao tác thay đổi/xóa dữ liệu không thể khôi phục.

## 1. Cụ thể hóa động từ có tính phá hủy (Destructive verbs)

Từ ngữ mơ hồ dễ bị AI hiểu theo hướng không thể khôi phục — nói rõ bản chất hành động và ràng buộc muốn giữ lại:

| Mơ hồ (dễ hiểu lầm) | Rõ ràng (nên dùng) |
| --- | --- |
| "Cut the section" (cắt phần này) — có thể hiểu là xóa vĩnh viễn | "Remove the section from the draft, but keep the file" (xóa khỏi bản nháp, vẫn giữ tệp) |
| "Update the file" (cập nhật tệp) — có thể hiểu là ghi đè toàn bộ | "Add a new appendix; don't rewrite the existing sections" (thêm phụ lục mới, không viết lại phần hiện có) |

## 2. Thiết lập ranh giới phạm vi rõ ràng (Name the bounds)

Giới hạn chính xác đối tượng Cowork được phép thao tác, tránh AI làm lan sang dữ liệu ngoài ý muốn:

- **Theo số lượng/thời gian:** "Only the 3 most recently updated files in this folder".
- **Theo điều kiện:** "Only contracts that closed in Q3".
- **Theo hành động:** "Don't message anyone — draft only".

## 3. Chỉ tạo bản nháp khi mới thiết lập Scheduled Tasks

[[Lên lịch Tác vụ (Scheduled Tasks)|Tác vụ tự động]] chạy ngầm, không có người giám sát trực tiếp:

- Giai đoạn đầu, chỉ yêu cầu Claude **tạo bản nháp (draft)** để tự kiểm tra nội dung trước.
- Chỉ khi đã tin tưởng quy trình chạy ổn định, đúng ý qua nhiều lần, mới cho phép Claude tự động gửi/xuất bản thay mặt mình.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Công thức Prompt chuẩn (Deliverable + Inputs + Nuances)]], [[Mô hình Phân quyền (Permissions)]], [[Lên lịch Tác vụ (Scheduled Tasks)]], [[Chọn Thư mục & Phân quyền Đọc-Ghi]], [[Giám sát Real-time khi Cowork chạy (Watch the Run)]], [[Khi nào KHÔNG nên dùng Cowork (Giới hạn Sử dụng)]]
