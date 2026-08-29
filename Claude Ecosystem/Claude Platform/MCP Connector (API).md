---
tags:
  - claude/nen-tang
aliases:
  - MCP Connector
  - MCP Client API
up: "[[Claude Platform]]"
---

# MCP Connector (API)

Cách kết nối Claude **trực tiếp** tới một MCP Server (ví dụ Linear) ngay trong `client.beta.messages.create` — **không cần tự viết JSON Schema** như [[Tool Use (Function Calling)|custom tool]] thông thường, cũng không cần cài SDK/client riêng cho dịch vụ đó (ví dụ `linear-sdk`).

Cơ chế đứng sau: **Introspection (tự khám phá)** — Claude tự hỏi MCP Server "ở đây có công cụ gì, dùng ra sao" thay vì bạn phải khai báo trước.

## 2 tham số cốt lõi trong request

- **`mcp_servers`** — khai báo kết nối tới server: loại kết nối (`type: "url"`), địa chỉ URL, tên đại diện (`name`), và token xác thực (`authorization_token`).
- **`tools`** dạng **`mcp_toolset`** — cho phép Claude dùng công cụ từ server đã khai báo ở trên, tham chiếu qua `mcp_server_name`. Mặc định (không giới hạn thêm) Claude được dùng **toàn bộ** tool mà server đó cung cấp; muốn giới hạn (scope down) thì cấu hình thêm ở đây.

```python
response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=4096,
    betas=["mcp-client-2025-04-04"],          # Bắt buộc vì còn Beta
    mcp_servers=[
        {
            "type": "url",
            "url": "https://mcp.linear.app/mcp",
            "name": "linear",
            "authorization_token": os.environ["LINEAR_TOKEN"],
        }
    ],
    tools=[
        {
            "type": "mcp_toolset",
            "mcp_server_name": "linear",       # Tham chiếu tới name ở trên
        }
    ],
    messages=[
        {"role": "user", "content": "Liệt kê các issue đang mở của tôi trên Linear"}
    ],
)
```

## Điều khác biệt so với Function Calling truyền thống

| | Function Calling truyền thống | MCP Connector |
| --- | --- | --- |
| Khai báo `tools` | Tự viết `name`/`description`/`input_schema` cho từng tool | Mảng `tools` chỉ ghi `mcp_toolset` + tên server — không có schema nào |
| Biết tool nào tồn tại | Do bạn liệt kê sẵn | Claude tự hỏi server lúc gửi request (introspection) |
| Cài đặt tích hợp | Tự viết/cài SDK riêng cho dịch vụ (`linear-sdk`...) | Không cần — server tự lo, xem [[Connectors (MCP)#Vì sao cần MCP, đã có Tool và Skills rồi?\|lý do MCP tồn tại]] |

## Lọc công cụ (Tool filtering) — Least Privilege Pattern

Một MCP Server (Slack, GitHub...) thường lộ ra **rất nhiều** tool, gồm cả quyền Đọc lẫn quyền Ghi/Xóa. Mở hết cho Claude mang 2 rủi ro:

- **Lãng phí context window** — càng nhiều định nghĩa tool, càng tốn token đầu vào không cần thiết.
- **Rủi ro bảo mật** — Claude có thể lỡ gửi tin nhắn, xóa tài liệu, sửa dữ liệu ngoài ý muốn.

Cách khắc phục: đặt `mcp_toolset` ở **chế độ tắt mặc định**, rồi chỉ bật đúng những tool cần dùng:

```python
tools=[
    {
        "type": "mcp_toolset",
        "mcp_server_name": "slack",
        "default_config": {"enabled": False},   # 1. Tắt hết mặc định
        "configs": {                             # 2. Chỉ bật đúng tool cần
            "search_messages": {"enabled": True},
            "list_channels": {"enabled": True},
        },
    }
]
```

Kết quả: kết nối trở thành **read-only theo đúng nghĩa** — Claude chỉ thấy và dùng được `search_messages`/`list_channels`, không đụng tới các tool ghi/xóa khác mà server có sẵn.

## Lưu ý kỹ thuật

- **Beta feature** — cần truyền header `betas=["mcp-client-..."]` (cộng dồn với các beta khác nếu có).
- **Tái sử dụng cho dịch vụ khác** — cùng một cấu trúc code, chỉ đổi `url`/`authorization_token`/`name` trong `mcp_servers` là dùng ngay được Slack, GitHub, Notion... không cần viết lại logic.

## Recap

| Nội dung | Chi tiết cần nhớ |
| --- | --- |
| Bản chất | Kết nối MCP Server ngay trong Messages API — không tự viết schema, không cài SDK riêng cho dịch vụ. |
| Cơ chế | Introspection — Claude tự hỏi server có tool gì, dùng ra sao, ngay lúc gửi request. |
| 2 tham số | `mcp_servers` (khai báo kết nối: url, name, token) + `tools: mcp_toolset` (tham chiếu server, phân quyền tool). |
| Phân quyền tool | `default_config: {"enabled": False}` tắt hết, `configs` bật đúng tool cần — theo nguyên tắc Least Privilege. |
| Vì sao cần phân quyền | Tránh tốn context window vô ích và tránh AI lỡ tay Ghi/Xóa khi không được phép. |
| Trạng thái | Beta — bắt buộc header `betas=["mcp-client-..."]`. |

## Liên kết

- Thuộc nhóm: [[Claude Platform]]
- Là một dạng: [[Tool Use (Function Calling)]] — nhưng không tự viết schema
- Khái niệm/sản phẩm nền: [[Connectors (MCP)]]
- Ứng dụng trong Claude Code (CLI): [[Quản lý MCP Server trong Claude Code]]
