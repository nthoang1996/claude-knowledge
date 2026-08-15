---
tags:
  - claude/tich-hop
aliases:
  - Explore, Plan, Code, Commit
  - Quy trình 4 bước
up: "[[Claude Code]]"
---

# Explore, Plan, Code, Commit

Quy trình làm việc chuẩn được khuyến nghị khi dùng [[Claude Code]] cho tác vụ phức tạp — tách rõ giai đoạn *tìm hiểu bối cảnh* và *lập kế hoạch* trước khi để Claude viết code, tránh phải sửa lỗi dây chuyền do code vội vã.

## Sơ đồ 4 bước

```mermaid
graph LR
    A["1. Explore<br/>Khảo sát"] --> B["2. Plan<br/>Lập kế hoạch"]
    B --> C["3. Code<br/>Viết mã"]
    C --> D["4. Commit<br/>Lưu trữ"]
```

## 1. Explore & 2. Plan

Cách nhanh nhất để thực thi 2 bước đầu: bật [[Plan Mode]] (`Shift+Tab` cho tới khi hiện "Plan Mode").

- **Read-only** — Claude chỉ đọc file liên quan trong dự án, tra cứu web nếu cần tài liệu thư viện mới; chưa sửa gì lên mã nguồn.
- **Đầu ra** — một bản kế hoạch hành động chi tiết (plan of action) để người dùng duyệt trước khi cho phép chỉnh sửa thật.
- Nếu chỉ cần khảo sát/tóm tắt tổng quan mã nguồn mà không có ý định sửa code, có thể chạy **Explore subagent** độc lập mà không cần bật cả Plan Mode.

> Xem cơ chế hoạt động chi tiết và ví dụ thực hành ở [[Plan Mode]].

## 3. Code (Viết mã)

Sau khi duyệt xong Plan, chọn **"Approve"** để Claude lần lượt thực hiện từng hạng mục theo đúng [[Claude Code#Cơ chế hoạt động Vòng lặp Agentic|vòng lặp Agentic]]: sửa file/chạy lệnh từng bước, tự kiểm tra và xác minh kết quả (build, test) trước khi báo hoàn thành.

- **Tự xử lý sự cố** — Claude tự tìm cách khắc phục lỗi phát sinh trong lúc code, nhưng đôi khi vẫn cần người dùng can thiệp để định hướng lại.
- **Lợi ích từ Explore & Plan trước đó** — đã khảo sát kỹ nên người dùng hiểu rõ bối cảnh và lý do Claude ra từng quyết định kỹ thuật, việc chỉ dẫn ở bước Code vì vậy dễ dàng hơn.

### Mẹo giúp giai đoạn Code diễn ra mượt mà

- **Xác định tiêu chí thành công rõ ràng** ngay từ lúc lập Plan — Claude cần biết chính xác kết quả "đúng" trông như thế nào.
- **Trang bị thêm công cụ hỗ trợ** để giảm số vòng giao tiếp qua lại (ví dụ: cài Chrome extension để Claude tự mở browser tab, kiểm thử UI trực tiếp — xem [[Claude in Chrome]]).
- **Cung cấp bộ test suite** (unit/integration test) để Claude liên tục tự kiểm tra lại code; Claude cũng có thể tự viết bộ test này.

> [!tip] Tối ưu bộ nhớ với CLAUDE.md
> Nếu Claude liên tục mắc cùng một lỗi hoặc quên quy tắc dự án, yêu cầu Claude ghi giải pháp vào file `CLAUDE.md` — file này đóng vai trò bộ nhớ dài hạn, lưu quy ước cấu trúc và kinh nghiệm xử lý lỗi cho các phiên làm việc sau.

## 4. Commit (Lưu trữ)

Sau khi tự kiểm thử (self-testing) và hài lòng với kết quả, tác vụ mới sẵn sàng để commit/push:

- **Review độc lập bằng Subagent** — trước khi tạo commit, chạy một Subagent riêng để rà soát lại toàn bộ code vừa sửa. Subagent đóng vai trò "cặp mắt mới" (fresh pair of eyes), không mang theo thành kiến hay giả định lệch mà Agent chính có thể đã tích tụ trong suốt phiên làm việc.
- Để Claude tự viết commit message mô tả đúng thay đổi, theo đúng văn phong/chuẩn của dự án, thay vì tự soạn tay.
- Luôn xem lại diff trước khi xác nhận — tránh để Claude tự ý push hoặc thao tác git nguy hiểm khi chưa được cho phép.

## Vì sao Explore & Plan là bước quan trọng nhất

- **Định hướng đúng trước khi viết code** — đổi hướng lúc lập kế hoạch (chưa có dòng code nào) luôn nhanh và ít tốn công hơn nhiều so với sửa một đống code đã viết sai cấu trúc.
- **Quyền xem xét của người dùng** — đánh giá bản kế hoạch, yêu cầu Claude điều chỉnh trực tiếp trên đó nếu chưa ưng ý, thay vì để Claude code sai rồi mới sửa.

## Ví dụ prompt

> "Tôi cần thêm tính năng chuyển đổi định dạng WebP vào pipeline tải ảnh lên của hệ thống. Hãy tìm hiểu xem đoạn chuyển đổi này nên nằm ở đâu trong pipeline, liệu chúng ta có cần thêm thư viện phụ thuộc (dependencies) mới hay không, và đề xuất hướng tiếp cận."

## Tóm tắt chế độ/công cụ theo từng bước

| Bước | Hành động chính | Chế độ / Công cụ khuyên dùng |
| --- | --- | --- |
| 1. Explore | Đọc file, hiểu luồng dữ liệu | [[Plan Mode]] hoặc Explore subagent |
| 2. Plan | Duyệt bản kế hoạch | [[Plan Mode]] (read-only) |
| 3. Code | Sửa file, chạy lệnh, tự kiểm tra | Approval / Auto-accept Mode (`Shift+Tab`) |
| 4. Commit | Review bằng Subagent, viết commit message | Main Agent + Code Review Subagent |

## Liên kết

- Thuộc: [[Claude Code]]
- Xem thêm: [[Plan Mode]]
