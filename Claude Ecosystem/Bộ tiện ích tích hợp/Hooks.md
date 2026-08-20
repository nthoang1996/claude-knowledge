---
tags:
  - claude/tich-hop
aliases:
  - Hooks
  - Hook
up: "[[Claude Code]]"
---

# Hooks

Điểm can thiệp tự động trong [[Claude Code]] — cho phép kích hoạt lệnh script/hệ thống tại các thời điểm cụ thể trong vòng lặp hoạt động (xem [[Claude Code#Cơ chế hoạt động: Vòng lặp Agentic|Vòng lặp Agentic]]).

## Đặc trưng: Tính xác định (Deterministic)

- Dặn qua [[CLAUDE.md Bộ nhớ dự án|CLAUDE.md]] (vd. "chạy Prettier sau mỗi lần sửa file") — Claude vẫn là AI, thỉnh thoảng có thể quên hoặc bỏ qua.
- Dùng **Hook** — lệnh bắt buộc chạy 100%, không có ngoại lệ, vì do hệ thống kích hoạt chứ không phụ thuộc AI tự nhớ.

## Tác dụng chính

- **Tự động định dạng** — chạy linter/formatter (Prettier, ESLint...) ngay khi file bị chỉnh sửa.
- **Ghi log** — lưu lịch sử toàn bộ lệnh Claude đã thực thi, phục vụ kiểm duyệt/tuân thủ quy trình.
- **Chặn hành động nguy hiểm** — ngăn AI tự ý sửa file quan trọng hoặc file cấu hình production.
- **Thông báo** — tự gửi thông báo khi Claude hoàn thành một tác vụ dài.

## Cách thiết lập

- **Vị trí cấu hình** — lưu trong file `settings.json`.
- **Cách thao tác** — dùng lệnh **`/hooks`** ngay trong giao diện Claude Code, hoặc tự sửa trực tiếp `settings.json`.
- **Thành phần một Hook** — **Event** (thời điểm kích hoạt) + **Matcher** (điều kiện lọc, áp dụng cho công cụ nào) + **Command** (lệnh thực thi).

## 5 sự kiện (Events)

| Event | Thời điểm chạy | Phù hợp cho |
| --- | --- | --- |
| **PreToolUse** | Ngay trước khi Claude thực thi một tool | Chặn/kiểm tra an toàn trước khi sửa file, chạy lệnh |
| **PostToolUse** | Ngay sau khi tool thực thi xong | Tự động format code (Prettier), kiểm tra lỗi |
| **UserPromptSubmit** | Ngay khi người dùng nhấn gửi, trước khi Claude xử lý | Thêm ngữ cảnh, kiểm tra nội dung đầu vào |
| **Stop** | Khi Claude hoàn thành phản hồi/tác vụ | Dọn file tạm, phát âm thanh báo xong |
| **Notification** | Khi Claude gửi một thông báo | Bắn thông báo về Slack/Email |

### Ví dụ điển hình: Auto-format với PostToolUse

Hook ở event `PostToolUse` + matcher `Edit | MultiEdit | Write` → mỗi khi Claude sửa/tạo file, hook tự kích hoạt, kiểm tra đuôi file và chạy công cụ căn chỉnh tương ứng (Prettier cho TypeScript, `gofmt` cho Go...).

## Cơ chế chặn (Blocking) với PreToolUse

`PreToolUse` cho phép kiểm tra và **ngăn chặn** lệnh của Claude trước khi kịp thực thi.

- **Cách hoạt động** — Claude Code truyền thông tin công cụ + dữ liệu đầu vào (JSON) vào `stdin` của script Hook; Hook xử lý và trả về **Exit Code** để quyết định:

| Exit Code | Hành động |
| --- | --- |
| **0** | Cho phép lệnh chạy bình thường |
| **2** (chặn) | Dừng lệnh ngay; nội dung lỗi từ `stderr` gửi ngược lại cho Claude để nó biết lý do bị chặn và tự điều chỉnh |
| Khác (1, 3...) | Báo lỗi hệ thống cho người dùng, nhưng không dừng thao tác của Claude |

- **Dùng để áp đặt quy tắc cứng** mà AI không được phép vi phạm — vd. chặn ghi/sửa thư mục cấu hình production, chặn lệnh Bash nguy hiểm (`rm -rf`), chặn commit trực tiếp vào nhánh `main`.

## Chia sẻ Hooks với team

- **Vị trí cấp dự án** — cấu hình tại `.claude/settings.json` trong thư mục dự án (khác cấu hình cá nhân/toàn cục) mang tính project-level.
- **Đồng bộ qua Git** — commit file này lên repo; ai clone dự án về cũng tự động áp dụng cùng tập quy tắc Hook, không cần cấu hình lại thủ công.
- **Biến môi trường `CLAUDE_PROJECT_DIR`** — dùng trong câu lệnh của Hook để trỏ đúng đường dẫn file trong dự án, đảm bảo script chạy đúng dù Claude đang đứng ở thư mục con nào.

> [!tip] Triết lý cốt lõi
> Nếu một việc bắt buộc phải xảy ra, không được phép có ngoại lệ — đừng viết vào prompt/CLAUDE.md, hãy đưa vào Hook.

## Liên kết

- Thuộc: [[Claude Code]]
- Liên quan: [[CLAUDE.md Bộ nhớ dự án]], [[Claude Code#Cơ chế phân quyền (Permissions)|Permissions]]
