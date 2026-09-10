---
tags:
  - claude/cowork
aliases:
  - Steer mid-task
  - Queue Interrupt Cowork
up: "[[Claude Cowork]]"
---

# Điều chỉnh Hướng đi Mid-task (Steer & Interrupt)

## Thay đổi tư duy: can thiệp ngay, không chờ chạy xong

- **Thói quen cũ (Chat):** ngồi chờ AI tạo xong toàn bộ, rồi mới gõ lệnh sửa hoặc bấm Regenerate.
- **Tư duy mới (Cowork):** thấy Claude làm sai hướng (sai file nguồn, sai định dạng, sai văn phong) là **ngắt hoặc bổ sung lệnh ngay lập tức** qua nút Queue/Interrupt — chi phí điều chỉnh giữa chừng rất thấp.

## 2 hình thức can thiệp

1. **Queue (xếp hàng lệnh mới):** gửi thêm ghi chú chỉnh sửa khi Claude đang chạy — AI tiếp thu hướng mới và điều chỉnh ngay từ bước hiện tại, không phải làm lại từ đầu.
2. **Stop & Refine (dừng hẳn):** kế hoạch lệch quá xa thì bấm Dừng, tinh chỉnh lại prompt ban đầu dựa trên những gì vừa quan sát được, rồi khởi chạy lại.

## Ví dụ minh họa

Đang ở bước 4/5 trên [[Progress Panel (Bảng tiến độ)]] (viết bản thảo Section 2/4 của memo cạnh tranh Q3), sau khi đã đọc xong nguồn, đối chiếu giá, lập xong dàn ý → gõ vào ô nhập liệu: *"Đổi văn phong sang bullet points ngắn gọn, không viết đoạn văn dài"* và bấm **Queue** → Claude áp dụng ngay cho các phần nội dung còn lại.

## Bài học cốt lõi

Luôn quan sát [[Progress Panel (Bảng tiến độ)]] trong lúc Cowork chạy. Điều chỉnh giữa chừng giúp tiết kiệm thời gian xử lý và giữ quy trình tạo file đi đúng quỹ đạo.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Progress Panel (Bảng tiến độ)]], [[Nhịp điệu Làm việc (Work Rhythm)]], [[Cơ chế Câu hỏi Làm rõ (Clarifying Questions)]]
