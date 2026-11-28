---
tags:
  - claude/nen-tang
aliases:
  - Text Editor Tool
  - str_replace_based_edit_tool
up: "[[Claude Platform]]"
---

# Text Editor Tool

Tool **dựng sẵn (Anthropic-defined)** cho Claude xem và sửa file văn bản, thư mục. Thuộc nhóm [[Client Tools]].

> **Dựng sẵn schema, không dựng sẵn phần thực thi.** Bạn không phải viết `input_schema`, nhưng **vẫn phải tự viết code thao tác file**: Claude trả về `tool_use` → code của bạn thực hiện → gửi lại `tool_result`.

## Khi nào cần dùng

IDE đã có sẵn AI assistant, nhưng tool này dành cho việc **tự xây** khả năng sửa file:

| Tình huống | Ví dụ |
| --- | --- |
| Ứng dụng cần **sửa file bằng code** (programmatically), không có người thao tác trên IDE | Agent refactor, scaffolding code, bot auto-fix bug trong CI/CD |
| Môi trường **không có code editor đầy đủ** | Remote server, CLI, Docker container, web app |
| **Tích hợp** sửa file vào app chạy bằng Claude của riêng bạn | Công cụ nội bộ, không phụ thuộc extension bên thứ ba |

- Giá trị cốt lõi: **fine-grained control** — bạn quyết định thư mục nào được đọc/sửa, phân quyền, ghi log và logic thực thi; từ đó tái tạo được phần sửa file của một AI code editor ngay trong app của mình.
- Nếu không muốn tự viết phần thực thi: [[Agent SDK]] có sẵn tool đọc/ghi/sửa file.

## So với Custom Tool

| Thành phần | Custom Tool (vd `set_reminder`) | Text Editor Tool |
| --- | --- | --- |
| Schema (`name`, `description`, `input_schema`) | Bạn tự viết | Đã có sẵn |
| Hàm thực thi | Bạn tự viết | **Vẫn bạn tự viết** |

Giống mọi client tool: Claude chỉ **yêu cầu** (ask), không tự chạm vào file system; code của bạn mới **thực hiện** (perform).

## Cài đặt phía ứng dụng

1. **Ánh xạ** mỗi `command` (bảng bên dưới) tới một hàm xử lý file tương ứng.
2. **Kiểm tra đường dẫn** trước khi thao tác (xem [[#Bảo mật]]).
3. **Trả `tool_result` rõ ràng:** nội dung file, kết quả sửa, hoặc thông báo lỗi (file không tồn tại, `old_str` không khớp duy nhất...) để Claude làm bước tiếp theo.

## Khai báo

```json
{"type": "text_editor_20250728", "name": "str_replace_based_edit_tool"}
```

- Chỉ cần `type` + `name`, **không** truyền `input_schema` (schema đã nằm sẵn trong model, không sửa được).
- Tuỳ chọn: `max_characters` — giới hạn độ dài output của `view`.

## Phiên bản theo model

Khai báo ngắn gọn `type` + `name` này còn gọi là **schema stub**: phía Claude/API tự mở rộng thành định nghĩa đầy đủ (lệnh + tham số) đúng phiên bản. `type` và `name` phải **khớp model đang gọi**.

| `type` | `name` | Model | `undo_edit` |
| --- | --- | --- | --- |
| `text_editor_20250728` | `str_replace_based_edit_tool` | Claude 4 trở đi (bản dùng trong note) | Không |
| `text_editor_20250429` | `str_replace_based_edit_tool` | Claude 4 (bản trước) | Không |
| `text_editor_20250124` | `str_replace_editor` | Sonnet 3.7 *(đã retired 19/02/2026)* | Có |
| `text_editor_20241022` | `str_replace_editor` | Sonnet 3.5 *(đã retired 28/10/2025)* | Có |

> Code mẫu cũ hay có hàm `get_text_edit_schema(model)` chỉ `if / elif` cho 3.7 và 3.5 → cả hai đã retired; model mới không khớp nhánh nào, hàm trả `None` và lỗi khi đưa vào `tools`. Nên dùng bản mới, và nếu chọn theo model thì phải có `else` (trả mặc định hoặc raise lỗi rõ ràng).

- `name` chính là tên tool trong block `tool_use` Claude gửi về → bộ định tuyến tool phía bạn phải nhận đúng tên này (đổi phiên bản thì kiểm tra lại).

## Các lệnh (`tool_use.input.command`)

| `command` | Tham số kèm theo | Tác dụng |
| --- | --- | --- |
| `view` | `path`, `view_range` (tuỳ chọn) | Xem nội dung file **hoặc** liệt kê thư mục |
| `create` | `path`, `file_text` | Tạo mới / ghi đè file (nên backup nếu file đã tồn tại) |
| `str_replace` | `path`, `old_str`, `new_str` | Thay đúng **1** vị trí khớp; khớp 0 hoặc >1 lần → báo lỗi |
| `insert` | `path`, `insert_line`, `insert_text` | Chèn sau dòng `insert_line` (`0` = đầu file) |

- `view_range`: `[dòng đầu, dòng cuối]`, đếm từ `1`, dòng cuối `-1` = đến hết file; chỉ áp dụng cho file. Hữu ích với file lớn → tiết kiệm token/context.
- Khi lỗi: trả `tool_result` kèm `is_error: true` để Claude tự sửa và thử lại.

### Undo (`undo_edit`) — chỉ có ở bản cũ

Một số tài liệu liệt kê **6 khả năng**: xem file/thư mục, xem theo khoảng dòng, thay text, tạo file, chèn tại dòng, **undo**. Undo ứng với lệnh `undo_edit` của các phiên bản cũ, đã **bị bỏ từ `text_editor_20250429`** (Claude 4) → bản `text_editor_20250728` không có. Cần hoàn tác thì dựa vào backup hoặc Git.

## Ý nghĩa

Đọc → chọn vùng dòng → sửa / chèn / tạo cho phép Claude **tự khảo sát codebase và sửa nhiều file** như một software engineer, thay vì chỉ gợi ý code trong khung chat.

## Ví dụ

| Yêu cầu | Chuỗi thao tác của Claude |
| --- | --- |
| "Mở `./main.py` và tóm tắt nội dung" | `view` → trả lời tóm tắt (không sửa file) |
| "Viết vào `./main.py` hàm tính π tới 5 chữ số thập phân (`3.14159`), rồi tạo `./test.py` để test" | `view` `main.py` → `str_replace` / `insert` → `create` `test.py` |

- Claude **tự chuỗi nhiều thao tác** (đọc → sửa → tạo) cho một yêu cầu của người dùng.
- "Một yêu cầu" **không** phải một API request: mỗi lần Claude trả `tool_use`, code của bạn thực thi, gửi `tool_result` trong request tiếp theo — lặp tới khi xong (xem [[Vòng lặp Agent]]).
- Tool này **không chạy được code**: tạo `test.py` chưa có nghĩa là đã test. Muốn chạy cần thêm tool thực thi (Bash tool, xem [[Client Tools]]; hoặc code của bạn tự chạy).

## Lợi ích

- **Đỡ công thiết kế** — không phải tự đặt tên tool, viết mô tả và schema (khác custom tool như `get_current_datetime`, `set_reminder`).
- **Chuẩn hoá thao tác sửa code** — Claude được huấn luyện sẵn với giao thức này; `str_replace` sửa đúng đoạn cần sửa, không phải ghi lại cả file.

## Bảo mật

> `path` là **output của model, không đáng tin**. Phải giới hạn mọi thao tác trong 1 thư mục gốc: chuẩn hoá đường dẫn rồi kiểm tra không thoát ra ngoài (`..`, symlink, đường dẫn tuyệt đối, `%2e%2e%2f`) — ví dụ Python: `Path.resolve()` + `is_relative_to(root)`.

An toàn **không tự có sẵn**: việc chống ghi đè mất dữ liệu (backup, kiểm tra khớp duy nhất) và chặn truy cập trái phép là do code thực thi của bạn đảm nhiệm.

- Có thể chạy trong tài khoản / môi trường cô lập, tránh chạm file hệ thống và dữ liệu nhạy cảm.
- Dự án thật: dùng Git (`git diff` để rà soát, `git restore` để hoàn tác thay đổi chưa commit) trước khi áp dụng chính thức.

## Liên kết

- Thuộc nhóm: [[Claude Platform]]
- Là một dạng: [[Client Tools]] (schema dựng sẵn, thực thi trên máy bạn)
- Cơ chế `tool_use` / `tool_result`: [[Tool Use (Function Calling)]], [[Vòng lặp Agent]]
- Phân biệt với: [[Built-in Tools (Server Tools)]] (Anthropic tự thực thi)
