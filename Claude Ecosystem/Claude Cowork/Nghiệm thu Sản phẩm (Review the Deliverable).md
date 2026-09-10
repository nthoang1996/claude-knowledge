---
tags:
  - claude/cowork
aliases:
  - Review the Deliverable
  - Nghiệm thu sản phẩm Cowork
up: "[[Claude Cowork]]"
---

# Nghiệm thu Sản phẩm (Review the Deliverable)

## Nơi tìm sản phẩm & thái độ nghiệm thu

Xem trước (preview) ngay trong app Claude, hoặc mở trực tiếp từ working folder trên máy. Đọc bản thảo với tinh thần **sắc bén, sáng suốt** — như duyệt bài của một đồng nghiệp đáng tin cậy nhưng còn mới.

## Checklist 3 điểm cần kiểm tra

| Tiêu chí | Nội dung kiểm tra |
| --- | --- |
| **1. Đúng mục tiêu (Objective)** | Sản phẩm có giải quyết đúng bản chất việc cần làm, hay tinh vi nhưng lệch hướng? |
| **2. Độ chính xác số liệu (Accuracy)** | Đối chiếu số liệu/sự kiện với tài liệu gốc. Mẹo: yêu cầu Claude chỉ rõ trích xuất từ file nào, rồi tự kiểm tra lại. |
| **3. Phát hiện suy đoán (Hallucinations)** | Ngày tháng, tên riêng, trích dẫn cụ thể không có trong tài liệu đầu vào = **dấu hiệu cảnh báo lỗi**, không phải thông tin bổ sung hữu ích. |

## Xử lý phản hồi theo chất lượng bản thảo

- **Bản thảo khá ổn (mostly right):** chỉ rõ điểm cần sửa thay vì yêu cầu làm lại từ đầu — Claude nhớ bối cảnh phiên làm việc nên sửa file nhanh hơn nhiều so với regenerate toàn bộ.
- **Sai ở điểm cốt lõi (wrong in a load-bearing way):** nghĩa là prompt ban đầu thiếu bối cảnh quan trọng — trỏ Claude đến nguồn bối cảnh bị thiếu và yêu cầu điều chỉnh lại, thay vì chỉ sửa câu chữ.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Nhịp điệu Làm việc (Work Rhythm)]], [[Điều chỉnh Hướng đi Mid-task (Steer & Interrupt)]], [[Công thức Prompt chuẩn (Deliverable + Inputs + Nuances)]]
