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

## 4 trường bắt buộc của mọi request

Dù dùng [[SDKs|SDK]] hay HTTP thô, request `POST /v1/messages` luôn cần đủ 4 thứ:

| Trường | Nằm ở | Vai trò |
| --- | --- | --- |
| API key (`x-api-key`) | **Header** (không nằm trong JSON body) | Xác thực, tính phí, áp rate limit theo tài khoản/workspace |
| `model` | Body | Chọn model xử lý (tốc độ/chi phí/trí tuệ khác nhau — xem [[Model Tiers]]) |
| `messages` | Body | Mảng hội thoại gửi cho Claude |
| `max_tokens` | Body | Trần số token **output** được sinh ra |

> [!note] Header thứ 3 hay bị quên
> Ngoài `x-api-key`, HTTP thô còn bắt buộc `anthropic-version` và `content-type: application/json` (SDK tự thêm). Chi tiết: [[SDKs|SDK vs HTTP thô]].

Chi tiết từng trường:

- **API key** — SDK tự đọc biến môi trường `ANTHROPIC_API_KEY` nếu không truyền khi khởi tạo client. Key chỉ ở server ([[Vòng đời Request (Request Lifecycle)|vì sao cần server trung gian]]).
- **`model`** — ghi đúng ID model. Có 2 dạng: *alias* (trỏ tới bản mới nhất, tiện nhưng hành vi có thể đổi khi ra bản mới) và *snapshot có ngày* (cố định, ổn định cho production). ID cũ có thể bị ngừng hỗ trợ (deprecated) → kiểm tra danh sách model hiện hành trước khi code.
- **`messages`** — mảng object `{role, content}`:
  - `role` chỉ là `user` hoặc `assistant`; thường xen kẽ nhau và **tin đầu tiên phải là `user`**. System prompt **không** nằm ở đây mà ở tham số `system` riêng.
  - `content` là chuỗi, hoặc mảng *content block* (text, image, document, `tool_use`, `tool_result`...) khi cần đa phương thức hoặc tool.
  - API stateless → muốn Claude nhớ ngữ cảnh phải gửi lại toàn bộ lịch sử mỗi lượt. Có thể đặt sẵn một tin `assistant` ở cuối để "mớm" đầu câu trả lời (prefill) — tuỳ model có hỗ trợ không.
- **`max_tokens`** — chỉ giới hạn **output**, không giới hạn input (input bị giới hạn bởi context window — xem [[Context Management]]). Đây là trần chứ không phải mục tiêu: Claude thường dừng sớm hơn khi đã trả lời xong. Chạm trần thì `stop_reason` = `max_tokens` và câu trả lời bị **cắt giữa chừng**. Mỗi model có mức `max_tokens` tối đa riêng; khi bật [[Extended Thinking]], token suy nghĩ cũng tính vào `max_tokens`. Quy đổi thô: 1 token ≈ 3–4 ký tự tiếng Anh; tiếng Việt thường tốn nhiều token hơn trên cùng độ dài nghĩa.

Payload tối thiểu (JSON body):

```json
{
  "model": "<model-id>",
  "max_tokens": 1024,
  "messages": [
    { "role": "user", "content": "Xin chào, hãy giới thiệu ngắn gọn về bạn!" }
  ]
}
```

Các trường **tuỳ chọn** hay dùng thêm: `system`, `temperature`, `stream`, `tools`, `stop_sequences`, `thinking`.

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
- Xem thêm: [[SDKs]] (so sánh SDK vs HTTP thô), [[Console]], [[Model Tiers]], [[Vòng đời Request (Request Lifecycle)]]
- Chi tiết tham số/pricing đầy đủ: xem skill `claude-api` khi cần tra cứu sâu.
