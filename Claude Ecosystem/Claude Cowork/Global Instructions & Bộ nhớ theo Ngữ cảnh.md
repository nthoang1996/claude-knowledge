---
tags:
  - claude/cowork
aliases:
  - Global Instructions
  - Standing Brief
up: "[[Claude Cowork]]"
---

# Global Instructions & Bộ nhớ theo Ngữ cảnh

Trong **Cowork**, bộ nhớ **không** tự tích lũy từ mọi cuộc trò chuyện như [[So sánh 3 chế độ làm việc (Chat, Cowork, Code)|Claude Chat thường]] — Cowork chỉ "nhớ" những gì được chủ động thiết lập qua **Global instructions** và **Projects**. Đây là lý do đầu tư ngữ cảnh ban đầu tạo ra [[Tích lũy Giá trị theo Thời gian (Compounding Value)|giá trị tích lũy]] về sau.

## Chat vs. Cowork — cơ chế nhớ khác nhau

| | Claude Chat | Cowork |
| --- | --- | --- |
| Nguồn bối cảnh | Tự động học từ lịch sử chat | Chủ động thiết lập: Global instructions + Projects |
| Phạm vi áp dụng | Không rõ ràng, ngầm định | Global instructions → mọi phiên; Project → riêng theo dự án |
| Vai trò người dùng | Thụ động (Claude tự suy luận) | Chủ động (người dùng khai báo trước) |

## Global Instructions là gì

Một **"bản tóm tắt chỉ thị cố định" (standing brief)** — viết một lần trong Settings, Claude áp dụng cho **mọi** phiên Cowork: mọi đoạn chat mới và mọi tác vụ [[Lên lịch Tác vụ (Scheduled Tasks)|đã lên lịch]].

- Khác với [[Projects trong Cowork (4 Thành phần & Bộ nhớ)|Projects]]: Project gom ngữ cảnh cho **một luồng việc cụ thể**, còn Global instructions áp dụng **xuyên suốt mọi luồng việc**.
- Tương tự vai trò `CLAUDE.md` cấp **User-level** bên [[CLAUDE.md Bộ nhớ dự án|Claude Code]] (cấu hình cá nhân, không gắn với 1 dự án) — khác `CLAUDE.md` cấp Project-level (gắn 1 repo, commit vào Git).

## Cách thiết lập

1. Mở Claude Desktop app.
2. **Settings → Cowork**.
3. Mục **Global instructions** → **Edit**.
4. Soạn nội dung → **Save**.

## Nội dung nên viết — 3 nhóm

- **Thông tin cá nhân & vai trò** — bạn là ai, công việc chính, phòng ban.
- **Thuật ngữ & viết tắt nội bộ** — từ viết tắt hay dùng trong công ty/team (vd: "QBR deck" nghĩa là gì) để Claude hiểu ngay, không phải hỏi lại.
- **Phong cách output mong muốn** — định dạng, độ dài, văn phong áp dụng mặc định cho mọi sản phẩm bàn giao.

> [!tip] Mẹo viết hiệu quả
> Áp dụng nguyên tắc súc tích như `CLAUDE.md`: chỉ ghi thông tin **thực sự lặp lại ở nhiều tác vụ** — nếu một chi tiết chỉ dùng cho một dự án riêng lẻ, đưa vào [[Projects]] thay vì Global instructions để tránh phình to và loãng trọng tâm.

## Ví dụ tùy chỉnh theo vai trò (Product Manager)

Khai báo đúng vai trò giúp Claude tự điều chỉnh hành vi mặc định, không cần nhắc lại mỗi câu lệnh:

- **Định dạng chuẩn** — mặc định trình bày theo PRD hoặc Brief.
- **Tư duy theo bài toán khách hàng** — luôn nêu vấn đề/nhu cầu người dùng trước khi đề xuất giải pháp.
- **Minh bạch dữ liệu** — luôn dẫn nguồn tài liệu gốc khi trích số liệu/metrics.

Áp dụng tương tự cho vai trò khác (Tài chính, Pháp lý...) — chỉ cần khai báo đúng vai trò và cách trình bày đặc thù của ngành.

## Nguyên tắc hoàn thiện dần (Iterative Refinement)

Không cần viết hoàn hảo ngay từ đầu — Global Instructions nên được bổ sung dần theo thời gian sử dụng:

- **Tín hiệu cần bổ sung**: bất kỳ lúc nào phải nhắc/sửa Claude lặp lại cùng một điều → đó là ứng viên nên đưa vào Global Instructions.
- *Ví dụ*: "Đưa kết luận lên đầu câu trả lời" (bottom line up front), "không dùng dấu phẩy Oxford", "trả lời ngắn gọn dưới 3 dòng".

> [!tip] Liên hệ
> Cùng tư duy với mẹo **"Lưu câu sửa lỗi vào bộ nhớ"** của [[CLAUDE.md Bộ nhớ dự án]] — chỉ khác chỗ áp dụng: `CLAUDE.md` sửa quy chuẩn kỹ thuật của dự án, Global Instructions sửa hành vi/văn phong chung của Claude với bạn.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Thực hành Thiết lập Global Instructions & Project]], [[Tích lũy Giá trị theo Thời gian (Compounding Value)]], [[Projects trong Cowork (4 Thành phần & Bộ nhớ)]], [[Projects]], [[CLAUDE.md Bộ nhớ dự án]], [[Bản chất & Triết lý]]
