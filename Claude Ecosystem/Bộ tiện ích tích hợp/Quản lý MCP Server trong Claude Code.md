---
tags:
  - claude/tich-hop
aliases:
  - Quản lý MCP Server
  - MCP Server trong Claude Code
up: "[[Claude Code]]"
---

# Quản lý MCP Server trong Claude Code

Cách thêm, quản lý và giới hạn phạm vi (scope) của [[Connectors (MCP)|MCP Server]] khi dùng trong [[Claude Code]].

## Thêm & quản lý server

- Thêm server mới: `claude mcp add <tên-server> <thông-số-kết-nối>`
- Lệnh **`/mcp`** trong phiên làm việc: xem danh sách server đang kết nối, kiểm tra trạng thái (chạy/lỗi), bật/tắt server chưa cần dùng để tiết kiệm context — xem thêm [[Claude Code#Mẹo tiết kiệm context]].

## Phân loại theo kết nối

| Loại | Cách hoạt động | Trường hợp dùng |
| --- | --- | --- |
| **HTTP (Remote)** | Kết nối qua internet tới dịch vụ cloud của bên thứ ba | Linear, Slack, Notion, các SaaS khác |
| **Stdio (Local)** | Chạy như một tiến trình ngay trên máy | Đọc file hệ thống, database nội bộ, chạy lệnh terminal local |

## Phạm vi áp dụng (Scope)

- **Local** (cá nhân — theo dự án) — chỉ hoạt động trong thư mục dự án hiện tại, chỉ trên máy của bạn.
- **User** (cá nhân — toàn hệ thống) — cấu hình toàn cục (global), dùng được ở mọi dự án/thư mục trên máy.
- **Project** (cho cả team) — lưu vào file `.mcp.json` ngay trong dự án; push lên Git để mọi thành viên pull code về là tự động có cùng bộ MCP Server, không cần cài lại từ đầu.

## Chi phí ngữ cảnh (Context Cost)

Mỗi MCP Server khi bật sẽ chiếm một phần [[Claude Code#Hai trụ cột kỹ thuật: Context & Tools|Context window]] để chứa mô tả công cụ (tool definitions) — dù chưa dùng tới; bật càng nhiều server, bộ nhớ khả dụng cho code/câu trả lời càng bị thu hẹp.

**Giải pháp tối ưu:**

- **Tắt server chưa cần dùng** — qua lệnh `/mcp`.
- **Ưu tiên CLI có sẵn** — nếu công cụ đã có bản CLI (`gh`, `aws`...), để AI chạy lệnh trực tiếp qua terminal thay vì tải cố định mô tả tool vào bộ nhớ.
- **Dùng [[Skills]] thay vì MCP** — Skill chỉ tải tên & mô tả ngắn trước, chỉ tải toàn bộ nội dung chi tiết khi AI xác định thực sự cần; khác với MCP luôn tải sẵn toàn bộ tool definitions ngay từ đầu.

**Cơ chế tự động — Tool Search Mode:** khi tool definitions từ MCP chiếm vượt quá **10%** Context window, Claude Code tự chuyển sang tìm & nạp tool theo nhu cầu (on-demand) thay vì tải sẵn hết — tiết kiệm bộ nhớ nhưng đổi lại độ chính xác/tin cậy có thể giảm.

## Liên kết

- Thuộc: [[Claude Code]]
- Khái niệm nền: [[Connectors (MCP)]]
- Liên quan: [[Claude Code#Mẹo tiết kiệm context]]
