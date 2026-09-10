---
tags:
  - claude/cowork
aliases:
  - Cơ chế Câu hỏi Làm rõ
  - Clarifying Questions
up: "[[Claude Cowork]]"
---

# Cơ chế Câu hỏi Làm rõ (Clarifying Questions)

## Vì sao Cowork hỏi làm rõ trước khi làm

- **Claude Chat:** trao đổi qua lại từng lượt (turn-by-turn) — bối cảnh bổ sung dần trong hội thoại.
- **Claude Cowork:** ủy quyền trọn gói (delegate) → AI quay lại với một **artifact hoàn chỉnh**, không có nhiều lượt qua lại giữa chừng.

Vì vậy, để tránh ra sản phẩm sai lệch, Cowork chủ động tìm và lấp đầy **khoảng trống bối cảnh (context gaps)** bằng cách hỏi lại *ngay từ đầu*, trước khi đụng vào file.

## Cách trả lời câu hỏi làm rõ

- **Chọn phương án có sẵn:** Cowork đưa ra các lựa chọn đánh số kèm đề xuất (`Recommended`) — chọn bằng chuột hoặc phím điều hướng.
- **Trả lời tự do:** không có phương án phù hợp thì gõ câu trả lời riêng.
- **Bỏ qua (Skip):** nhấn `Esc` nếu thấy không cần thiết.

## Ví dụ minh họa

Cowork quét thư mục thấy 2 file dễ gây nhầm: `Q2-board-memo.docx` và `Q2-board-memo-FINAL.docx` → hỏi: *"Tôi thấy có 2 bản memo trong thư mục này. Tôi nên căn chỉnh theo bản nào?"*

1. `Q2-board-memo-FINAL.docx` *(Recommended)*
2. `Q2-board-memo.docx`
3. Bản nào cũng được — chúng khá giống nhau

## Bài học cốt lõi

Việc Cowork hỏi 1–2 câu ở đầu quy trình **không phải phiền phức**, mà là cơ chế bảo vệ giúp tiết kiệm thời gian và đảm bảo sản phẩm đầu ra bám sát đúng tài liệu chuẩn.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Nhịp điệu Làm việc (Work Rhythm)]], [[Công thức Prompt chuẩn (Deliverable + Inputs + Nuances)]], [[Bản chất & Triết lý]]
