---
tags:
  - claude/cowork
aliases:
  - Mô hình Phân quyền Cowork
  - Permissions Model Cowork
  - Approval Modes
up: "[[Claude Cowork]]"
---

# Mô hình Phân quyền (Permissions Model)

Chọn một thư mục thì Claude tự động có quyền đọc/ghi trong thư mục đó. Với thao tác tác động **ra bên ngoài** (gửi mail, đăng tin nhắn, chia sẻ tệp), chọn 1 trong 2 chế độ phê duyệt.

## 2 chế độ phê duyệt

- **Ask before acting (mặc định)** — Claude tạm dừng, chờ phê duyệt trước mỗi hành động tác động ra ngoài. Phù hợp công cụ mới, file lạ, hoặc quy trình muốn giám sát chặt.
- **Act without asking** — chạy xuyên suốt không dừng hỏi; nhanh hơn nhưng rủi ro hơn. Phù hợp workflow lặp lại đã quen thuộc, dữ liệu/trang web đáng tin cậy, và đang chủ động ngồi giám sát.

## Guardrail bất biến

> Claude **LUÔN LUÔN** hỏi ý kiến trước khi xóa vĩnh viễn một tệp tin — quy tắc bắt buộc, không thể tắt hay bỏ qua trong cấu hình, áp dụng bất kể đang chọn chế độ phê duyệt nào.

## Các điểm kiểm soát khác

- **Connectors & MCP** — quyết định ứng dụng nào Claude được phép truy cập, và tần suất xin phép của từng connector.
- **Truy cập Web** — Admin có thể tắt tìm kiếm web; người dùng có thể giới hạn danh sách trang web mà [[Claude in Chrome]] được phép ghé thăm.

## Ý nghĩa cốt lõi

Là chìa khóa để trải nghiệm "**Ủy quyền (Delegating), không chỉ Trò chuyện (Chatting)**" hoạt động thực sự: AI tự do thực thi tác vụ nặng, nhưng con người luôn nắm quyền quyết định ở các bước quan trọng.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Bản chất & Triết lý]], [[Chọn Thư mục & Phân quyền Đọc-Ghi]], [[Thêm & Quản lý Connectors (Cowork)]]
