---
tags:
  - claude/nen-tang
aliases:
  - Tool Runner
  - toolRunner
  - tool_runner
up: "[[Claude Platform]]"
---

# Tool Runner

Tính năng có sẵn trong [[SDKs]] (TypeScript, Python, Ruby) giúp tự động hóa toàn bộ phần boilerplate khi tự viết [[Vòng lặp Agent]] bằng tay.

## 2 vấn đề khi tự viết tay

1. **Mã lặp lại (boilerplate)** — phải tự quản lý vòng lặp `while`, tự kiểm tra `stop_reason`, tự `push` `tool_result` vào mảng `messages`.
2. **Khai báo trùng lặp** — vừa viết hàm thực thi bằng code, vừa phải tự gõ lại `input_schema` (JSON Schema) mô tả hàm đó cho Claude (xem [[Tool Use (Function Calling)#Cấu trúc khai báo tool|cấu trúc khai báo tool]]).

## Cách hoạt động

Tool Runner tự trích xuất kiểu dữ liệu (types) và tài liệu (JSDoc/docstring) từ hàm code thật để sinh `JSON Schema`, đồng thời tự điều phối vòng lặp bên dưới — chỉ cần truyền thẳng các hàm vào `tools`.

```typescript
function getWeather(city: string) { /* ... */ }
function getForecast(city: string) { /* ... */ }

const runner = client.beta.messages.toolRunner({
  model: "claude-sonnet-4-6",
  max_tokens: 1024,
  messages: [
    { role: "user", content: "I'm packing for a three-day trip to Denver. What's the weather today and over the next few days?" },
  ],
  tools: [getWeather, getForecast], // truyền trực tiếp hàm, không cần tự viết JSON Schema
});

const finalMessage = await runner.untilDone();
```

## So sánh: tự viết tay vs. Tool Runner

| Tiêu chí | Tự triển khai (Manual) | Tool Runner |
| --- | --- | --- |
| Vòng lặp `while` | Tự viết, tự kiểm soát điều kiện dừng | SDK tự xử lý ẩn bên dưới |
| JSON Schema | Gõ thủ công `input_schema` | Tự sinh từ kiểu dữ liệu của hàm |
| Cập nhật `messages` | Tự `push` tin nhắn `assistant` + `tool_result` | SDK tự quản lý lịch sử |
| Lấy kết quả cuối | Tự bắt `stop_reason == "end_turn"` | `await runner.untilDone()` |

## Liên kết

- Thuộc nhóm: [[Claude Platform]]
- Tự động hóa cơ chế lặp thủ công mô tả ở: [[Vòng lặp Agent]]
- Tự sinh schema cho: [[Tool Use (Function Calling)]]
- Có sẵn trong: [[SDKs]]
- Ví dụ thực hành từng bước bằng code: [[Weather Agent (Ví dụ thực hành)]]
