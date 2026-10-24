---
tags:
  - claude/su-dung
  - ai/fluency
aliases:
  - AI Fluency
  - Thành thạo AI
  - 3 Engagement Modes
  - Automation Augmentation Agency
up: "[[Tinh chỉnh kết quả]]"
---

# AI Fluency (Thành thạo AI)

Không chỉ là biết gõ prompt — là sự kết hợp của **kỹ năng thực hành, kiến thức, góc nhìn và giá trị đạo đức** để tương tác với AI sao cho:

- **Effective** — đạt đúng kết quả mong muốn.
- **Efficient** — tối ưu thời gian, tài nguyên.
- **Ethical** — tôn trọng bản quyền, minh bạch, chuẩn mực xã hội.
- **Safe** — an toàn thông tin, bảo mật dữ liệu, kiểm soát rủi ro.

Năng lực cần có để đạt 4 tiêu chí trên: **4D** (Delegation, Description, Discernment, Diligence) — xem [[Bộ khung 4D]].

## Bức tranh tổng thể

```mermaid
graph TD
    Goal["Mục tiêu: AI Fluency<br/>Effective • Efficient • Ethical • Safe"]
    Goal --> M1["Automation"]
    Goal --> M2["Augmentation"]
    Goal --> M3["Agency"]
    M1 --> D["Nền tảng 4D<br/>Delegation • Description • Discernment • Diligence"]
    M2 --> D
    M3 --> D
```

- **3 mode** = *cách* hợp tác; **4D** = *kỹ năng nền* dùng chung cho cả 3 mode (trọng tâm mỗi mode khác nhau, xem [[Bộ khung 4D]]).
- Mô hình không lỗi thời khi model AI tiến hóa: luôn cần giao đúng việc, mô tả đúng, thẩm định đúng, giữ trách nhiệm.

## 3 hình thức tương tác (Engagement Modes)

```mermaid
graph LR
    A["Automation<br/>AI là công cụ thực thi"] --> B["Augmentation<br/>AI là bạn đồng hành tư duy"] --> C["Agency<br/>AI là đại lý tự trị"]
```

### Automation (Tự động hóa)

- Giao **một tác vụ cụ thể** theo chỉ dẫn trực tiếp; AI chỉ thực thi đúng việc được bảo.
- Ví dụ: viết câu SQL, tóm tắt văn bản, đổi định dạng file.

### Augmentation (Cộng tác)

- Người và AI **cùng tư duy**, trao đổi qua lại: mở rộng ý tưởng, phản biện, hoàn thiện.
- Ví dụ: brainstorm kiến trúc hệ thống, phản biện quy trình, code review.

### Agency (Ủy quyền tự trị)

- Không giao từng việc nhỏ mà **cấu hình AI** (tri thức, hành vi, quy tắc kiểm soát) để nó tự lập kế hoạch và hành động thay mình; người dùng đóng vai quản lý đặt mục tiêu + giới hạn.
- Ví dụ: Sub-agent / Plugin tự chạy test, phân tích và sửa lỗi trong CI/CD, dùng Hook để kiểm soát.

## So sánh nhanh

| Hình thức | Vai trò người | Vai trò AI | Mức tự trị |
| --- | --- | --- | --- |
| Automation | Người ra lệnh cụ thể | Trình thực thi | Thấp |
| Augmentation | Đối tác sáng tạo | Bạn đồng hành tư duy | Trung bình |
| Agency | Người đặt luật/quy chuẩn | Đại lý độc lập | Cao |

## Lưu ý

- Ba hình thức **không loại trừ nhau** — một dự án thường trộn cả ba; chọn theo mức rủi ro và độ rõ ràng của việc.
- Càng lên Agency, **Discernment + Diligence** càng quan trọng vì AI chạy lâu, ít người giám sát từng bước → cần guardrail (permission, hook, review kết quả).

## Tổng kết khóa học

Key takeaways chốt lại toàn bộ khung AI Fluency:

- **Thành thạo AI là kỹ năng rèn luyện, không đạt sau một đêm** — phát triển qua *intentional practice* (thực hành có chủ đích) cả 4 năng lực; 4D áp dụng xuyên suốt cả 3 mode, càng dùng càng tinh.
- **Human + AI bổ trợ thế mạnh cho nhau** — kết quả tốt nhất không phải AI thay thế hoàn toàn hay người làm thủ công hết: AI mạnh ở tốc độ và xử lý dữ liệu, người mạnh ở tư duy phản biện, sáng tạo, sự tinh tế; chuyên môn và phán đoán của người vẫn là nền tảng (người quyết định chia việc nào cho AI).
- **Khung không lỗi thời** — công cụ/model đổi nhanh, nhưng 4D xây từ nguyên lý gốc nên vẫn dùng được khi AI tiến hóa.
- **AI không phải phép thuật** — là công cụ mạnh nhưng không thần thánh; mức hữu ích và an toàn phụ thuộc hoàn toàn vào cách người dùng *định hướng, giám sát và tương tác* (Delegation → Description → Discernment → Diligence).

```mermaid
graph LR
    A["Delegation<br/>giao đúng việc"] --> B["Description<br/>mô tả rõ"] --> C["Discernment<br/>thẩm định"] --> D["Diligence<br/>chịu trách nhiệm"]
    D -.->|"rút kinh nghiệm, lặp lại"| A
```

## Liên kết

- Thuộc: [[Tinh chỉnh kết quả]]
- Chi tiết: [[Bộ khung 4D]]
- Công cụ ứng với Agency: [[Subagents (Tác vụ phụ)]], [[Hooks]], [[Claude Code]], [[Tự động hóa Claude Code (Routines & Headless)]]
- Xem thêm: [[Bắt đầu với Claude]], [[Ứng dụng thực tế]]
