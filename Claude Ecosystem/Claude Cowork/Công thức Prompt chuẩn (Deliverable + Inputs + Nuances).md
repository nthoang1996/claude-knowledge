---
tags:
  - claude/cowork
aliases:
  - Công thức Prompt chuẩn Cowork
  - Deliverable Inputs Nuances
up: "[[Claude Cowork]]"
---

# Công thức Prompt chuẩn cho Cowork

Để Claude Cowork ra đúng sản phẩm ngay từ lần chạy đầu (tránh phải regenerate), một prompt tốt cần đủ **3 thành phần**:

$$\text{Prompt chuẩn} = \text{Deliverable} + \text{Inputs} + \text{Nuances}$$

## 3 thành phần

1. **Deliverable — Tên sản phẩm đầu ra:** định dạng, kiểu tệp, độ dài cụ thể. VD: "bản tóm tắt 1 trang", "1 slide QBR", "bảng xếp hạng ứng viên kèm ghi chú".
2. **Inputs — Nguồn dữ liệu đầu vào:** thư mục làm việc, kênh thảo luận, khoảng thời gian, ứng dụng cần truy cập. VD: "thư mục Q3_Sales", "kênh Slack #proj-launch", "email tháng trước". Cowork chỉ xử lý tốt khi được cấp đúng và đủ bối cảnh.
3. **Nuances — Yêu cầu chi tiết & bối cảnh chuyên môn:** tiêu chí đánh giá, góc nhìn chuyên môn, lưu ý đặc thù mà AI không tự suy ra được. VD: "muốn 3 kịch bản Cơ sở/Tốt nhất/Tồi tệ nhất, có tính đến 3 địa điểm mới mở Q3 năm ngoái".

## Ví dụ minh họa

> *"Hãy tạo cho tôi một tệp Word tóm tắt 1 trang **[Deliverable]** dựa trên các file ghi chú cuộc họp trong thư mục Local 'Q3_Client_Notes' và chuỗi email từ khách hàng Acme Corp trong tháng 8 **[Inputs]**. Trong bản tóm tắt, hãy phân loại các phản hồi thành 2 nhóm: Must-have và Nice-to-have, đồng thời làm nổi bật các rủi ro về tiến độ nếu có **[Nuances]**."*

Ví dụ khác (bản ghi nhớ cạnh tranh):

> *"A four-page memo* **[Deliverable]** *based on the Q3 Competitive Review folder, using last quarter's memo as format reference, and drawing from analyst-call PDFs only* **[Inputs]**, *written for the executive team at the leadership offsite to help them decide on the new pricing tier. Lead with the recommendation and flag anything we can't verify* **[Nuances]**."*

## Nguyên tắc "Điền đầy khoảng trống"

Bỏ trống bất kỳ thành phần nào (không nêu định dạng, không chỉ rõ nguồn dữ liệu...) thì Cowork sẽ tự **hỏi ngược lại** để làm rõ trước khi thực thi, thay vì đoán bừa — chi tiết tại [[Cơ chế Câu hỏi Làm rõ (Clarifying Questions)]].

## Thay đổi tư duy: đầu tư trước thay vì sửa sau

- **Cách cũ (Chatbot):** prompt ngắn → nhận kết quả → sửa lặt vặt nhiều vòng ("thêm cái này", "sửa lại cái kia").
- **Cách mới (Cowork):** đầu tư công sức ở khâu chuẩn bị prompt ban đầu (**upfront work**) — cấp đủ bối cảnh ngay từ đầu để AI làm đúng ngay lần đầu, loại bỏ các vòng chỉnh sửa lặt vặt về sau.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Anatomy của một Task chuẩn]], [[Tiêu chí chọn Task phù hợp (3 Patterns)]], [[Nhịp điệu Làm việc (Work Rhythm)]], [[Cơ chế Câu hỏi Làm rõ (Clarifying Questions)]]
