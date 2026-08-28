---
tags:
  - claude/nen-tang
aliases:
  - Client Tools
  - Công cụ phía máy khách
up: "[[Claude Platform]]"
---

# Client Tools (Công cụ phía máy khách)

Nhóm tool do **SDK của Anthropic đóng gói sẵn schema + runner**, nhưng **thực thi ngay trên máy/hạ tầng của bạn** — trái ngược với [[Built-in Tools (Server Tools)|Server tools]] (Anthropic tự lo hạ tầng, bạn chỉ nhận kết quả).

## Khác gì với Custom Tools thông thường

Về bản chất, Client tools vẫn là [[Tool Use (Function Calling)|custom tools]] (chạy trên máy bạn, bạn kiểm soát toàn quyền môi trường/dữ liệu) — nhưng **không cần tự viết `input_schema` từ đầu** như custom tool thông thường: SDK đã định nghĩa sẵn cấu trúc chuẩn cho một số tool phổ biến, bạn chỉ cần khai báo dùng và tự cài đặt phần thực thi theo giao thức có sẵn.

> Khác với [[Tool Runner]] (tự sinh schema từ *hàm code tự viết* của bạn), Client tools là schema **đã được Anthropic định nghĩa sẵn** cho một nhóm use case cụ thể — bạn không tự đặt tên/tham số.

## 2 ví dụ tiêu biểu

| Tool | Công dụng |
| --- | --- |
| **Memory** | Cho Claude đọc/ghi thông tin để nhớ xuyên suốt nhiều phiên hội thoại (session) khác nhau. |
| **Bash** | Cấp một shell Bash **liên tục (persistent)** để Claude tự chạy lệnh trực tiếp trên hệ thống của bạn. |

## So sánh nhanh: Server tools vs Client tools

| | Server tools | Client tools |
| --- | --- | --- |
| Nơi thực thi | Hạ tầng Anthropic | Máy/hạ tầng của bạn |
| Ai lo sandbox/hạ tầng | Anthropic | Bạn |
| Schema | Anthropic định nghĩa & tự chạy luôn | Anthropic định nghĩa sẵn, bạn tự cài phần thực thi |
| Kiểm soát dữ liệu/môi trường | Không (dữ liệu đi qua server Anthropic) | Toàn quyền (chạy nội bộ) |
| Ví dụ | Web search, Code execution, Web fetch | Memory, Bash |

## Liên kết

- Thuộc nhóm: [[Claude Platform]]
- Là một dạng: [[Tool Use (Function Calling)]] (custom tool có schema dựng sẵn)
- Phân biệt với: [[Built-in Tools (Server Tools)]] (thực thi phía server)
- Khác với: [[Tool Runner]] (tự sinh schema từ hàm code tự viết, không phải schema dựng sẵn)
