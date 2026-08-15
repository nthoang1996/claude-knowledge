---
tags:
  - claude/tich-hop
aliases:
  - Claude Code
up: "[[Bộ tiện ích tích hợp]]"
---

# Claude Code

Claude chạy dưới dạng CLI/agent lập trình — làm việc trực tiếp trong terminal, IDE (VS Code, JetBrains) hoặc app Desktop/Web, hỗ trợ đọc/sửa code, chạy lệnh, quản lý git...

## Trường hợp sử dụng chính

- **Lập trình theo agent** — đọc hiểu codebase, sửa lỗi, viết tính năng mới, chạy test.
- **Thao tác hệ thống** — chạy shell command, quản lý git (commit, PR...) khi được cho phép.
- **Tích hợp IDE** — dùng như extension trong VS Code/JetBrains, tham chiếu trực tiếp file đang mở trong editor.

## So với Claude.ai thông thường

- **Cách làm việc** — tự động truy cập trực tiếp file, thư mục, terminal trên máy, không cần copy/paste code qua lại.
- **Vai trò** — như đồng nghiệp lập trình ([[AI Agent]]) tự thực thi tác vụ, khác với vai trò tư vấn/giải thích của chat thông thường.
- **Phạm vi** — thao tác trên toàn bộ mã nguồn dự án, không giới hạn ở nội dung dán vào khung chat hoặc tải lên Projects.

## Nguyên tắc dùng hiệu quả

- **Context window có giới hạn** — không nạp toàn bộ codebase cùng lúc; hoạt động theo kiểu agent, tự truy vết/tra cứu đúng file, đúng hàm cần thiết thay vì đọc tràn lan.
- **Luôn xin phép trước khi hành động** — mặc định hỏi ý kiến trước khi chạy lệnh terminal hoặc ghi đè file; người dùng luôn kiểm soát, có thể chọn giám sát chặt từng thao tác hoặc duyệt nhanh.
- **Vẫn có thể mắc sai sót** — hiểu sai ý định, tạo bug mới, hoặc đưa ra giải pháp quá phức tạp (over-engineer); cần người dùng tiếp tục tham gia rà soát, định hướng lại kịp thời (stay in the loop).

## Liên kết

- Thuộc nhóm: [[Bộ tiện ích tích hợp]]
- Khái niệm nền: [[AI Agent]]
- Xem thêm: [[Claude for Slack]], [[Claude Design]], [[Claude in Chrome]]
