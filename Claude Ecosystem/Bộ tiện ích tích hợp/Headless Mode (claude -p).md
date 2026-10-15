---
tags:
  - claude/tich-hop
  - claude/automation
aliases:
  - Headless Mode
  - claude -p
  - Print mode
up: "[[Claude Code]]"
---

# Headless Mode (`claude -p`)

Chạy [[Claude Code]] như một **lệnh CLI một lần (one-shot)**, không giao diện tương tác — dùng khi tác vụ cần **môi trường của chính bạn** hoặc muốn nhúng Claude vào script Bash/Python.

## Khi nào dùng Headless thay vì Routines

| | [[Routines]] | Headless Mode |
| --- | --- | --- |
| Nơi chạy | Cloud Anthropic | **Máy/CI của bạn** |
| Phù hợp | Việc chạy trọn trên cloud, chỉ cần repo + connectors | Cần tool, mạng nội bộ, biến môi trường cục bộ; hoặc lồng vào script tự viết |
| Tần suất | Tối đa 1 giờ/lần | Tùy bạn (cron, CI, vòng lặp...) |

## Cú pháp cốt lõi: cờ `-p` / `--print`

- Hoạt động như lệnh Unix chuẩn: **đọc `stdin`, ghi `stdout`**, thoát khi xong → ghép **pipe** thoải mái.

```bash
claude -p "summarize the changes in this diff"

# Ghép ống
git diff | claude -p "Review đoạn diff này giúp tôi" > review.txt
```

### Cờ hay dùng kèm

| Cờ | Tác dụng |
| --- | --- |
| `--output-format json` / `stream-json` | Đầu ra có cấu trúc cho script khác xử lý |
| `--allowedTools` | Khai báo trước tool được phép (không ai bấm duyệt) |
| `--permission-mode` | Chọn chế độ quyền, xem [[Permission Modes]] |
| `--bare` | Bỏ auto-discovery (CLAUDE.md, hooks, MCP...) cho môi trường đồng nhất, xem [[#Cờ `--bare` — nhất quán cho CI/CD\|bên dưới]] |
| `--max-turns` | Giới hạn số lượt, tránh chạy vô hạn |
| `--resume` / `--continue` | Nối tiếp phiên trước |

## Structured Output (JSON có schema)

Khi đưa Claude vào script/pipeline, thường cần **dữ liệu chuẩn hóa** (array/object) thay vì đoạn văn để chương trình khác đọc được.

- **`--output-format json`** — trả toàn bộ kết quả dạng JSON.
- **`--json-schema '<schema>'`** — khuôn mẫu JSON Schema ép dạng dữ liệu mong muốn.
- Kết quả khớp schema nằm trong trường **`structured_output`** của JSON phản hồi.

```bash
claude -p "Extract the exported function names from src/core/style.js" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
  | jq '.structured_output.functions'
```

- **Schema** — object có trường `functions` là mảng chuỗi (`required` bắt buộc phải có).
- **`jq`** — lọc riêng `.structured_output.functions`; đừng parse trường văn bản `result`.
- **Giá trị** — Claude thành "hàm xử lý dữ liệu" tin cậy: output sạch để đẩy thẳng vào DB hoặc bước script kế tiếp.
- **Mẹo** — schema càng chặt (`required`, `enum`, `type`) thì càng ít phải xử lý ngoại lệ; vẫn nên kiểm tra `null`/thiếu trường ở script phía sau và exit code của lệnh.

## Multi-step: lưu & khôi phục `session_id`

Việc phức tạp nên chia giai đoạn (vd. **Plan → Execute**) thay vì nhồi vào một lệnh. Để bước sau nhớ bước trước, dùng `session_id`.

```mermaid
graph LR
    A["Bước 1: claude -p ... --output-format json"] --> B["Lưu JSON<br/>(chứa session_id)"]
    B --> C["Bước 2: claude --resume session_id -p ..."]
```

```bash
# Bước 1: lập kế hoạch, lưu kết quả
claude -p "Đánh giá module auth và đề xuất kế hoạch refactor" \
  --output-format json > /tmp/plan.json

# Bước 2: nối tiếp đúng phiên đó để thực thi
claude --resume "$(jq -r .session_id /tmp/plan.json)" \
  -p "Bây giờ hãy thực thi kế hoạch đã đề ra ở trên"
```

- **Giữ nguyên ngữ cảnh** — bước 2 nhớ suy luận, file đã đọc và kế hoạch của bước 1.
- **Script module hóa** — mỗi script một việc (lập kế hoạch / review / thực thi), nối nhau bằng `session_id` qua file hoặc biến.
- **Lưu ý** — kiểm tra `session_id` không rỗng trước khi `--resume`; ngữ cảnh dài vẫn bị giới hạn bởi context window (xem [[Claude Code#Auto-Compaction — khi context đầy|Auto-Compaction]]); bước 2 cần cờ quyền (`--allowedTools`/`--permission-mode`) riêng vì thực thi mới là bước thay đổi file.
- **Mẹo** — cho phép bước 1 chỉ đọc ([[Plan Mode]]), bước 2 mới mở quyền ghi: tách rõ "nghĩ" và "làm".

## Tóm tắt Headless Automation

| Tính năng | Cú pháp chính | Công dụng |
| --- | --- | --- |
| Headless base | `claude -p "prompt"` | Chạy không giao diện, nhanh, ghép pipeline |
| Structured output | `--output-format json --json-schema '...'` | Dữ liệu chuẩn cho DB/script khác |
| Session resume | `claude --resume "<session_id>"` | Nối ngữ cảnh cho quy trình nhiều bước |

## Cờ `--bare` — nhất quán cho CI/CD

**Vấn đề** — CI cần *cùng input → cùng kết quả* trên mọi máy/runner. Mặc định Claude Code tự **auto-discover** các thành phần cục bộ (`CLAUDE.md`, Hooks, Skills, Plugins, MCP Servers, memory...), nên mỗi máy có cấu hình khác nhau sẽ cho hành vi khác nhau → pipeline chập chờn (flaky).

**Giải pháp** — thêm **`--bare`**: bỏ qua toàn bộ auto-discovery trên, chỉ còn **model gốc + những gì bạn chủ động cấp**.

```bash
claude --bare -p "Review diff và liệt kê lỗi" \
  --allowedTools "Read" "Grep" \
  --output-format json
```

| Ưu điểm | Nhược điểm |
| --- | --- |
| Khởi động nhanh; hành vi **giống nhau trên mọi môi trường** (laptop, runner CI), không dính cấu hình cá nhân | Không có quy ước dự án, hook an toàn hay MCP nếu bạn không bật lại tường minh |

- **Cần thứ gì thì cấp rõ ràng** — `--allowedTools`, `--mcp-config`, `--append-system-prompt`... thay vì mong Claude tự nạp.
- **"Xác định" ở đây = môi trường cố định**, *không* biến LLM thành hàm thuần túy: cùng prompt vẫn có thể ra câu chữ khác nhau. Muốn đầu ra ổn định hơn nữa, kết hợp `--json-schema` (ép hình dạng) và prompt chặt.

> [!warning] Kiểm chứng theo phiên bản
> Chính xác `--bare` bỏ qua những gì (và `-p` thường có tự nạp `CLAUDE.md` hay không) có thể khác giữa các bản. Chạy `claude --help`, và nếu script cần quy tắc/hook nào thì kiểm tra nó có thực sự được nạp/không bị nạp, đừng giả định.

### 3 cờ cốt lõi cho tự động hóa

| Cờ | Mục đích | Ngữ cảnh |
| --- | --- | --- |
| `-p` | Chạy không giao diện, pipe stdin/stdout | Script local |
| `--output-format json` (+ `--json-schema`) | Đầu ra cấu trúc | Nạp DB/script khác |
| `--bare` | Bỏ auto-discovery, môi trường đồng nhất | CI/CD pipeline, test tự động |

## Tóm lại

> [!tip] Mental model
> `claude -p` biến Claude thành một **công cụ CLI thuần túy**: nhanh, nhẹ, dễ pipe vào script tự động hóa trên máy bạn.

## Liên kết

- Thuộc: [[Claude Code]]
- Phổ tự động hóa: [[Tự động hóa Claude Code (Routines & Headless)]]
- Liên quan: [[Routines]], [[Permission Modes]], [[Hooks]], [[SDKs]]
