---
tags:
  - claude/cowork
aliases:
  - Build your own plugin
  - Tự tạo Plugin
  - setup-claude
up: "[[Claude Cowork]]"
---

# Tự xây dựng Plugin (Cowork)

Khi quy trình của team **không giống bất kỳ plugin nào có sẵn**, bạn tự tạo plugin ngay trong Cowork — không cần viết manifest hay dựng thư mục thủ công. Đây là hướng thứ ba trong [[Plugins#Plugin có sẵn từ Anthropic|ba hướng dùng plugin]] (off the shelf → customize → build your own).

```mermaid
graph LR
    A["Kiểm tra Directory<br/>đã có plugin chưa?"] --> B["Tạo 1 skill<br/>cho việc lặp nhiều nhất"]
    B --> C["Thêm dần skill<br/>+ connectors"]
    C --> D["3–4 skills + connectors<br/>= plugin đáng chia sẻ"]
```

## Cách hoạt động

- **Trò chuyện trực tiếp với Cowork:** mô tả quy trình của team; Cowork tự **gom các Skill** cần thiết, **gắn các Connectors** liên quan rồi **đóng gói thành plugin hoàn chỉnh**, sẵn sàng cài vào ứng dụng (kết quả thường là một file `.plugin` để cài).
- **Claude hỏi ngược lại** về quy trình, công cụ và đầu ra mong muốn — càng đưa nhiều ngữ cảnh, tài liệu mẫu thật thì plugin càng sát (xem [[Cài đặt & Tùy biến Plugins (Cowork)]] cho cách đưa mẫu).
- Về bản chất, plugin sinh ra vẫn là tập hợp [[Skills]] + [[Connectors (MCP)]] (+ [[Subagents (Tác vụ phụ)|Subagents]] nếu cần) như mô tả ở [[Plugins]].

## Lộ trình: bắt đầu nhỏ

1. **Skill đầu tiên** cho công việc **lặp lại nhiều nhất** (chọn theo [[Tiêu chí chọn Task phù hợp (3 Patterns)]]).
2. **Bổ sung dần** các skill tiếp theo — mỗi skill chạy thử trên tài liệu thật trước khi thêm skill mới.
3. Khi có khoảng **3–4 skills kèm các connectors quan trọng** → đã là plugin hoàn chỉnh, đáng chia sẻ cho cả team.

Cách chia sẻ plugin cho team (phân phối qua tổ chức) thuộc bài sau — chưa ghi trong vault này.

## Trước khi tạo: kiểm tra Directory

Vào **Customize → Plugins → Directory** xem **Admin công ty đã xuất bản sẵn plugin nào cho tổ chức chưa** — tránh làm trùng, và plugin do admin phát hành thường đã gắn sẵn connectors/quy chuẩn chung của công ty; nếu gần đúng thì dùng rồi [[Cài đặt & Tùy biến Plugins (Cowork)|customize]] sẽ nhanh hơn tự dựng.

## Thực hành nhanh: `/setup-claude`

Cách nhanh nhất để **tìm và cài plugin phù hợp** mà không phải tự duyệt danh mục:

1. Mở **cuộc trò chuyện mới** trong Cowork.
2. Gõ `/setup-claude`.
3. Claude **phỏng vấn ngắn** về loại công việc của bạn, rồi **đề xuất plugin phù hợp nhất**.
4. **Thêm plugin ngay trong khung chat**, dùng thử luôn, sau đó [[Cài đặt & Tùy biến Plugins (Cowork)|customize]] theo quy trình của team.

- Hợp khi mới bắt đầu, chưa biết nên chọn plugin nào — kết quả gợi ý phụ thuộc vào câu trả lời, nên mô tả **vai trò, công việc lặp lại và công cụ đang dùng** càng cụ thể càng tốt.
- Nếu không plugin nào đề xuất khớp → chuyển sang tự xây dựng theo lộ trình ở trên.

## Chọn hướng nào?

| Tình huống | Hướng phù hợp |
| --- | --- |
| Quy trình phổ biến (Finance, Legal, Sales...) | **Off the shelf** — cài bản có sẵn |
| Có sẵn plugin gần đúng nhưng khác template/văn phong | **Customize** |
| Quy trình đặc thù, không plugin nào phủ | **Build your own** |
| Chưa biết bắt đầu từ đâu | `/setup-claude` để được đề xuất |

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Khái niệm nền: [[Plugins]], [[Skills]]
- Xem thêm: [[Cài đặt & Tùy biến Plugins (Cowork)]], [[Tích lũy Giá trị theo Thời gian (Compounding Value)]], [[Thêm & Quản lý Connectors (Cowork)]]
