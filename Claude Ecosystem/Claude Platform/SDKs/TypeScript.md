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

- `model` — model xử lý yêu cầu (ví dụ `claude-opus-5`; model ID thay đổi theo thời gian, không hardcode "mãi mãi"). Cách chọn giữa các cấp model: xem [[Model Tiers]].
- `max_tokens` — giới hạn độ dài phản hồi tối đa.
- `messages` — mảng hội thoại, mỗi phần tử có `role` (`"user"`/`"assistant"`) và `content` (nội dung tin nhắn).

```typescript
const msg = await client.messages.create({
  model: "claude-opus-5",
  max_tokens: 1024,
  messages: [{ role: "user", content: "Hello, Claude" }],
});
```

### `max_tokens` — trần an toàn, không phải mục tiêu

- Đặt `max_tokens: 1000` **không** có nghĩa Claude cố viết đủ 1000 token. Câu hỏi đơn giản (ví dụ "Thủ đô của Việt Nam?") chỉ tốn vài chục token rồi dừng tự nhiên, `stop_reason` = `end_turn`.
- Vai trò thật: "phanh khẩn cấp" — chặn câu trả lời quá dài hoặc model bị lặp vô tận, **giới hạn chi phí tối đa** mỗi request. Chạm trần thì ngắt cứng, `stop_reason` = `max_tokens`, câu trả lời **bị cụt**.
- Hệ quả khi code:
  - Đặt quá thấp → output bị cắt giữa câu/JSON/code. Luôn kiểm tra `stop_reason` trước khi dùng kết quả (xem bảng `stop_reason` ở [[Vòng đời Request (Request Lifecycle)]]).
  - Chỉ giới hạn **output**; input bị giới hạn bởi context window. Với [[Extended Thinking]], token suy nghĩ cũng tính vào `max_tokens`.
  - Muốn câu trả lời **ngắn hơn** → chỉ dẫn trong prompt/`system`; `max_tokens` chỉ cắt chứ không làm câu trả lời gọn hơn.
- Tương đương Python: `client = anthropic.Anthropic()` rồi `client.messages.create(model=..., max_tokens=..., messages=[...])`, đọc kết quả bằng `response.content[0].text` (chỉ an toàn khi biết chắc block đầu là `text`).

### `messages` — hội thoại nhiều lượt (stateless)

- Mỗi phần tử là `{ role, content }`; `role` chỉ có `"user"` (người dùng/ứng dụng của bạn gửi) hoặc `"assistant"` (lượt Claude đã trả lời trước đó). Mảng phải **bắt đầu bằng `user`**, thường xen kẽ `user` → `assistant` → `user`.
- API **không lưu lịch sử** → server của bạn phải giữ hội thoại (RAM/DB) và gửi lại **toàn bộ** mảng mỗi lượt để Claude "nhớ":

```typescript
const history: Anthropic.MessageParam[] = [
  { role: "user", content: "Chào bạn, tôi tên là Nam." },
  { role: "assistant", content: "Chào Nam! Rất vui được hỗ trợ bạn hôm nay." },
  { role: "user", content: "Tên tôi là gì?" },
];
const reply = await client.messages.create({ model: "claude-opus-5", max_tokens: 1024, messages: history });
// Lượt sau: push reply (role "assistant") + câu hỏi mới (role "user") vào history rồi gọi lại
```

- Hệ quả thực tế:
  - Hội thoại càng dài, **input token mỗi lượt càng tăng** (tốn tiền, chậm hơn, có thể chạm context window) → cắt/tóm tắt lịch sử cũ, xem [[Context Management]].
  - `system` prompt **không** nằm trong `messages` mà ở tham số `system` riêng.
  - Lượt `assistant` do Claude sinh ra phải được lưu lại **nguyên vẹn** (đặc biệt khi có `tool_use`/`thinking` block) trước khi gửi tiếp. Chi tiết trường `messages`/`content` block: xem [[REST API]].

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
- **`response.content` là mảng các khối (blocks)**, không phải một chuỗi. Ngoài `text`, Claude còn có thể trả về `tool_use`, `thinking`... nên luôn cần lặp qua từng block và kiểm tra `block.type === "text"` trước khi đọc `.text`. Khi có block `tool_use`, đó là dấu hiệu Claude đang tham gia [[Vòng lặp Agent]] chứ không chỉ trả lời đơn thuần.

## Liên kết

- Thuộc nhóm: [[SDKs]]
- Xem thêm: [[REST API]], [[Console]], [[Model Tiers]], [[Vòng lặp Agent]]
