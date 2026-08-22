---
tags:
  - claude/nen-tang
aliases:
  - REST API
  - Anthropic API
up: "[[Claude Platform]]"
---

# REST API

Giao thức kết nối chuẩn (HTTP) để gửi yêu cầu và nhận phản hồi từ Claude, gọi được từ **bất kỳ ngôn ngữ lập trình nào** hỗ trợ HTTP request.

- Nền tảng thấp nhất — mọi SDK đều xây dựng bên trên REST API này.
- Dùng khi ngôn ngữ/môi trường không có SDK chính thức, hoặc cần kiểm soát request/response ở mức chi tiết nhất.
- Endpoint chính: Messages API (gửi hội thoại, nhận phản hồi từ model).

## Ví dụ: soạn nháp trả lời support ticket bằng Messages API

Flow: khởi tạo client → lấy nội dung ticket → gọi `messages.create` → hiển thị bản nháp trả về.

```python
client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-haiku-4-5",   # chọn model — Haiku vì tác vụ soạn nháp đơn giản, ưu tiên tốc độ/chi phí
    max_tokens=1024,            # giới hạn độ dài phản hồi
    system=TONE_AND_GUIDELINES, # system prompt: vai trò & văn phong
    messages=[
        {"role": "user", "content": ticket_content}
    ],
)

draft = response.content
```

- **`model`** — chọn dòng model theo độ phức tạp tác vụ (Haiku cho việc đơn giản/nhanh/rẻ). Chi tiết các cấp model và cách chọn: xem [[Model Tiers]].
- **`max_tokens`** — chặn phản hồi quá dài, tránh phát sinh chi phí ngoài ý muốn.
- **`system`** — thiết lập vai trò/quy chuẩn hành văn cho Claude, tách biệt với nội dung yêu cầu thực tế.
- **`messages`** — mảng hội thoại; `role: "user"` mang nội dung ticket cần xử lý.

## Liên kết

- Thuộc nhóm: [[Claude Platform]]
- Xem thêm: [[SDKs]], [[Console]], [[Model Tiers]]
- Chi tiết tham số/pricing đầy đủ: xem skill `claude-api` khi cần tra cứu sâu.
