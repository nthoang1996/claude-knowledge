---
tags:
  - claude/nen-tang
aliases:
  - SDK TypeScript
  - Claude API TypeScript
up: "[[SDKs]]"
---

# SDK TypeScript/Node.js — Bắt đầu nhanh

Hướng dẫn thực hành gọi [[REST API|Claude API]] bằng gói `@anthropic-ai/sdk` (Node.js/TypeScript), từ setup đến ví dụ dùng thật.

## 1. Setup

1. **Lấy API Key** — tạo tại [[Console]] (`platform.claude.com`); cần nạp credit trước khi gọi được.
2. **Bảo mật key** — lưu vào file `.env.local`, không hardcode trong code. Tránh vô tình push key lên GitHub bị lộ/lợi dụng.
3. **Cài SDK**:

   ```bash
   npm install @anthropic-ai/sdk
   ```

4. **Khởi tạo client** — mặc định SDK tự đọc biến môi trường `ANTHROPIC_API_KEY`, không cần truyền key thủ công:

   ```typescript
   import Anthropic from "@anthropic-ai/sdk";
   const client = new Anthropic();
   ```

## 2. Giải phẫu một request

`client.messages.create()` có 3 trường bắt buộc:

- `model` — model xử lý yêu cầu (ví dụ `claude-opus-5`; model ID thay đổi theo thời gian, không hardcode "mãi mãi").
- `max_tokens` — giới hạn độ dài phản hồi tối đa.
- `messages` — mảng hội thoại, mỗi phần tử có `role` (`"user"`/`"assistant"`) và `content` (nội dung tin nhắn).

```typescript
const msg = await client.messages.create({
  model: "claude-opus-5",
  max_tokens: 1024,
  messages: [{ role: "user", content: "Hello, Claude" }],
});
```

## 3. Ví dụ thực hành: Code review

Dùng Claude rà lỗi code chỉ với ~20 dòng:

```typescript
const buggyCode = `
function add(a, b) {
  return a - b; // lỗi cố ý: cộng nhưng lại trừ
}
`;

const response = await client.messages.create({
  model: "claude-opus-5",
  max_tokens: 1024,
  system: "You are a terse senior code reviewer. Give feedback in one paragraph.",
  messages: [
    { role: "user", content: `Review this code:\n${buggyCode}` },
  ],
});

for (const block of response.content) {
  if (block.type === "text") {
    console.log(block.text);
  }
}
```

**2 điểm quan trọng:**

- **`system` prompt** — định hình vai trò/tính cách cho Claude (ở đây: reviewer cấp cao, trả lời cô đọng trong 1 đoạn), tách biệt với nội dung yêu cầu ở `messages`.
- **`response.content` là mảng các khối (blocks)**, không phải một chuỗi. Ngoài `text`, Claude còn có thể trả về `tool_use`, `thinking`... nên luôn cần lặp qua từng block và kiểm tra `block.type === "text"` trước khi đọc `.text`.

## Liên kết

- Thuộc nhóm: [[SDKs]]
- Xem thêm: [[REST API]], [[Console]]
