---
tags:
  - claude/cowork
aliases:
  - 3 Use Cases Projects
  - Ví dụ Project Northwind
up: "[[Claude Cowork]]"
---

# Mô hình Áp dụng Projects (3 Use Cases)

3 dạng luồng công việc thực tế nên tách thành **Project** riêng trong Cowork, minh họa qua ví dụ Project quản lý khách hàng "Northwind" — phối hợp đủ [[Projects trong Cowork (4 Thành phần & Bộ nhớ)|4 thành phần của Project]].

## Ví dụ mẫu: Project khách hàng "Northwind"

| Thành phần | Nội dung thiết lập |
| --- | --- |
| Instructions | Văn phong trang trọng; luôn gọi đúng tên Executive Buyer (Sarah Chen, VP Ops); cuối mỗi bản thảo ghi "draft for review" |
| Scheduled tasks | Sáng thứ Hai 8h: tóm tắt 3 dòng hoạt động CRM cuối tuần; trước hạn gia hạn 30 ngày: soạn sẵn bản tóm tắt chuẩn bị gia hạn |
| Context | Thư mục `Clients/Northwind/`, `QBR-prep/`; link trang tài khoản Salesforce, hồ sơ khách hàng Notion |
| Memory | Tự tích lũy: sở thích khách hàng, mốc gia hạn (15/03), mức nhạy cảm về giá trong kỳ QBR |

> [!example] Lợi ích
> Thiết lập một lần → không bao giờ phải trả lời lại "Tài liệu này viết cho ai?" hay "Văn phong thế nào?".

## 3 use case chuẩn để tạo Project

1. **Một khách hàng / tài khoản (Customer Account)** — lưu ghi chú họp, tài liệu bàn giao, quy tắc giao tiếp; mọi việc chuẩn bị họp/viết email theo dõi đều làm tại đây, Claude hiểu sâu dần mối quan hệ qua thời gian.
2. **Một sản phẩm bàn giao định kỳ (Recurring Deliverable)** — báo cáo tháng, đánh giá quý, bản tin lãnh đạo hàng tuần; mỗi chu kỳ là 1 cuộc trò chuyện mới nhưng thừa hưởng toàn bộ thông tin/đánh giá chu kỳ trước.
3. **Một chiến dịch / dự án ra mắt (Launch or Initiative)** — tái thiết kế, chuyển đổi hệ thống, chương trình mới; nơi gom brief, quyết định, cập nhật tiến độ đến khi hoàn thành.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Projects trong Cowork (4 Thành phần & Bộ nhớ)]], [[Global Instructions & Bộ nhớ theo Ngữ cảnh]], [[Tiêu chí chọn Task phù hợp (3 Patterns)]]
