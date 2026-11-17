---
tags:
  - claude/nen-tang
aliases:
  - Tool Use
  - Function Calling
  - Sử dụng công cụ
up: "[[Claude Platform]]"
---

# Tool Use (Function Calling)

**Tool** là một hàm do lập trình viên tự viết (query CSDL, gọi API ngoài, đọc file...), được mô tả cho Claude qua tên + công dụng + tham số đầu vào. Claude tự phân tích ngữ cảnh và quyết định *khi nào* cần gọi tool nào — đây là cách Claude vượt qua giới hạn không tự truy cập được hệ thống dữ liệu nội bộ của bạn.

## Vì sao cần Tool Use

LLM thuần có 2 giới hạn:

- **Knowledge cutoff** — dữ liệu huấn luyện dừng ở một mốc thời gian, không biết sự kiện/thông tin thời gian thực (thời tiết, giá cổ phiếu...).
- **Cô lập với thế giới ngoài** — mặc định không tự lướt web, đọc email, tra số dư tài khoản... nếu không có "cầu nối".

Hệ quả với trải nghiệm người dùng: khi bị hỏi thông tin thời gian thực (vd: "Thời tiết ở San Francisco?"), AI thuần buộc phải từ chối kiểu *"Tôi không có truy cập thông tin thời tiết cập nhật"* — nghịch lý vì người dùng tìm đến AI để có thông tin nhanh, nhưng lại bị từ chối cả những nhu cầu cơ bản hằng ngày. Đây chính là "bức tường" mà Tool Use ra đời để phá bỏ.

Tool Use = trang bị "công cụ ngoại vi" cho AI (giống người dùng Google, máy tính, Excel). Ví dụ: hỏi giá cổ phiếu → tool tài chính; tìm quán cà phê gần đây → tool Maps.

### Ví dụ thực hành: Đặt lịch nhắc (Reminder)

Tool Use không chỉ để *tra cứu* dữ liệu (như thời tiết) mà còn **bù đắp giới hạn nhận thức thời gian** và **cho phép Claude thực hiện hành động (side-effect)** lên hệ thống ngoài. Bài toán: user nói *"Đặt nhắc lịch khám bác sĩ, đó là Thứ Năm tuần sau nữa"* → mong đợi Claude xác nhận *"OK, I will remind you."*

3 thách thức ẩn bên dưới:

- **Limited time awareness** — system prompt có thể cho Claude biết *ngày* hiện tại, nhưng Claude không có real-time clock → không biết giờ/phút chính xác nếu không được cấp tool kiểu `get_current_time`.
- **Date calculation issues** — LLM là token prediction, không phải máy tính/lịch → dễ tính sai mốc tương lai phức tạp ("Thứ Năm tuần sau nữa", +45 ngày: sai ngày, năm nhuận, số ngày trong tháng).
- **No reminder capability** — mô hình chỉ "nhận lệnh → trả lời ngay", không có background process, DB lưu trữ hay push notification → không thể tự chờ đến đúng ngày để nhắc.

**Tools lấp khoảng trống (bridge):**

- Bài toán thời gian & tính toán → tool xử lý date/time phía server, đổi cụm tương đối sang mốc tuyệt đối chính xác (vd `2026-11-26`), không để AI tự tính.
- Bài toán đặt lịch → tool nối hệ thống nhắc ngoài (Google Tasks, Apple Reminders, cron job, DB); Claude chỉ phát lệnh `set_reminder(title, date_time)`, server mới thực sự lưu lịch.

**Bộ 3 tool giải quyết bài toán** (mỗi tool một việc, độc lập):

| Tool | Nhiệm vụ | Khắc phục |
| --- | --- | --- |
| `get_current_datetime` | Trả ngày giờ hiện tại từ đồng hồ server — mốc xuất phát cho mọi tính toán | Limited time awareness |
| `add_duration_to_datetime` | Nhận mốc thời gian + khoảng tăng (+7 ngày, +2 giờ), tính bằng thư viện tin cậy (`datetime` Python, `Day.js` JS) | Date calculation issues |
| `set_reminder` | Nhận thời điểm chính xác + nội dung, ghi vào DB / Google Calendar / hệ thống thông báo | No reminder capability |

Claude tự xâu chuỗi 3 tool qua [[Vòng lặp Agent]]. Ví dụ *"Remind me in a week to buy groceries"*:

```mermaid
graph TD
    A["1. get_current_datetime()<br/>→ 2026-11-17 20:18"] --> B["2. add_duration_to_datetime(start, days=7)<br/>→ 2026-11-24 20:18"]
    B --> C["3. set_reminder(title='Buy groceries', time='2026-11-24 20:18')<br/>→ OK"]
```

> **Nguyên tắc vàng:** khi mô hình có giới hạn, hãy *mở rộng năng lực bằng tool* thay vì tìm cách lách giới hạn đó trong prompt. Ép AI tính lịch bằng prompt dài dòng dễ gây hallucination và tốn token; việc chính xác/có tác động thực tế nên **ủy quyền (delegate)** cho code phía server.
>
> Bài học: Tool vừa là "mắt" (đọc dữ liệu thời gian thực) vừa là "tay" (ghi/kích hoạt hành động) của Claude.

### Flow 4 bước (Application ↔ Claude)

Claude **không** tự truy cập internet hay API của bạn — server của bạn là "người thực thi". Ví dụ với câu hỏi *"Thời tiết San Francisco thế nào?"*:

1. **Initial Request** — app gửi câu hỏi của user + danh sách tool (kèm mô tả), vd `get_weather(location)`.
2. **Tool Request** — Claude nhận ra thiếu dữ liệu thời gian thực, thấy có tool phù hợp → trả về JSON có cấu trúc yêu cầu chạy `get_weather(location="San Francisco, CA")`.
3. **Data Retrieval** — server chạy code thật (gọi API thời tiết / truy vấn DB), nhận kết quả vd `20°C, có mây`.
4. **Final Response** — server gửi kết quả lại cho Claude → Claude kết hợp câu hỏi gốc + dữ liệu thật thành câu trả lời tự nhiên: *"Thời tiết hiện tại ở San Francisco là 20°C và có mây."*

```mermaid
sequenceDiagram
    participant U as Người dùng
    participant S as Server của bạn
    participant C as Claude
    U->>S: Hỏi
    S->>C: 1. Câu hỏi + danh sách tools
    C->>S: 2. Yêu cầu chạy tool X với tham số Y
    S->>S: 3. Chạy API/DB lấy dữ liệu thật
    S->>C: 4. Gửi kết quả tool
    C->>S: Câu trả lời hoàn chỉnh
    S->>U: Trả lời
```

Phân vai: **Claude = "bộ não"** (phân tích ý định, quyết định *khi nào* và *dùng tool nào, thế nào*); **server của bạn = "tay chân"** (thực sự đi lấy dữ liệu từ API/DB rồi đưa lại cho Claude).

### Lợi ích cốt lõi

- **Real-time information** — truy cập dữ liệu mới hơn dữ liệu huấn luyện (thời tiết, tin tức, giá chứng khoán, chuyến bay).
- **External system integration** — nối AI với CSDL nội bộ, REST API, CRM (Salesforce), dịch vụ bên thứ ba.
- **Dynamic responses** — câu trả lời dựa trên dữ liệu thực tại thời điểm hỏi, không cố định/lý thuyết suông.
- **Structured interaction** — Claude biết rõ thiếu dữ liệu gì, gọi hàm nào, tham số ra sao, ở dạng JSON để server xử lý dễ.

> Tool use biến Claude từ **kho tri thức tĩnh** (như bách khoa toàn thư ngừng xuất bản) thành **trợ lý năng động** làm việc được với dữ liệu sống.

## Nguyên tắc cốt lõi

> **Claude KHÔNG trực tiếp chạy công cụ — code của bạn mới là nơi thực thi nó.**

```mermaid
graph LR
    A["Claude"] -->|"1. Yêu cầu gọi tool X<br/>với tham số Y"| B["Code của bạn"]
    B -->|"2. Chạy hàm X thật"| B
    B -->|"3. Trả tool_result"| A
```

Claude chỉ trả về một yêu cầu dạng JSON ("hãy chạy tool này với input này"); ứng dụng nhận yêu cầu, tự thực thi, rồi gửi kết quả ngược lại để Claude tiếp tục suy luận.

## Cấu trúc khai báo tool

Mỗi tool là một JSON Schema gồm 3 phần:

- **`name`** — tên định danh tool.
- **`description`** — mô tả công dụng, quan trọng nhất vì Claude dựa vào đây để quyết định *có nên* và *khi nào* gọi tool này (nên viết rõ ràng, cụ thể, như hướng dẫn cho người mới — không chỉ đặt tên hàm).
- **`input_schema`** — JSON Schema mô tả tham số đầu vào cần cung cấp.

```json
{
  "name": "lookup_building_code",
  "description": "Tra cứu nội dung một điều khoản quy chuẩn xây dựng theo mã định danh. Trả về toàn bộ văn bản của điều khoản đó.",
  "input_schema": {
    "type": "object",
    "properties": {
      "section": {
        "type": "string",
        "description": "Mã điều khoản quy chuẩn xây dựng cần tra cứu (ví dụ: 'Sec-102')"
      }
    },
    "required": ["section"]
  }
}
```

> **Nguyên nhân số 1 khiến Agent gọi sai tool (hoặc không nhận ra tool khả dụng): `description` quá mơ hồ.** Hãy mô tả cụ thể mục đích và dữ liệu trả về, không viết chung chung.

## Vòng đời một lượt gọi (tool_use_id)

1. Claude trả về `stop_reason: "tool_use"` kèm block `type: "tool_use"` gồm `id` (mã định danh riêng cho lượt gọi này, ví dụ `toolu_01ABC...`), `name`, `input`.
2. Code chạy hàm tương ứng với `input` đó.
3. Gửi lại block `type: "tool_result"` với `tool_use_id` **khớp chính xác** `id` ở bước 1, mang `content` là kết quả.

```mermaid
graph TD
    A["Claude: stop_reason = tool_use<br/>gọi lookup_building_code(section='Sec-102')<br/>id: toolu_01ABC..."] --> B["Code: thực thi tra cứu CSDL<br/>với tham số 'Sec-102'"]
    B --> C["Gửi role: user, block tool_result<br/>tool_use_id: toolu_01ABC... (phải khớp)<br/>content: nội dung điều khoản"]
    C --> D["Claude nhận diện đúng<br/>kết quả này thuộc yêu cầu nào<br/>→ tiếp tục suy luận"]
```

Nhờ `tool_use_id`, Claude biết chính xác `tool_result` vừa nhận thuộc về lượt gọi tool nào — quan trọng khi có nhiều tool được gọi song song (xem [[#Gọi song song parallel tool use]]) vì lúc đó nhiều `tool_result` có thể trả về không theo đúng thứ tự gọi.

Chi tiết cách nối `messages` qua nhiều lượt để tạo thành agent hoàn chỉnh: xem [[Vòng lặp Agent]].

## Các loại tool

- **Custom tools** — tự định nghĩa và tự thực thi trong code của bạn; phổ biến nhất.
- **Server-side/built-in tools** — Anthropic host & thực thi sẵn (web search, code execution, bash, text editor, computer use...): Claude gọi nhưng hạ tầng Anthropic chạy, không phải code của bạn. Chi tiết: [[Built-in Tools (Server Tools)]].
- **Client tools** — vẫn thực thi trên máy/hạ tầng của bạn như custom tool, nhưng dùng schema dựng sẵn từ SDK (ví dụ Memory, Bash) thay vì tự viết `input_schema` từ đầu. Chi tiết: [[Client Tools]].
- **MCP connectors** — tool lấy từ MCP server bên ngoài, không cần tự viết schema (xem [[MCP Connector (API)]]); khái niệm/sản phẩm nền xem [[Connectors (MCP)]].

## tool_choice — điều khiển việc chọn tool

- `auto` (mặc định) — Claude tự quyết có dùng tool hay trả lời trực tiếp.
- `any` — bắt buộc gọi một tool nào đó, không được trả lời trực tiếp.
- `tool: {name}` — ép dùng đúng một tool cụ thể.
- `none` — cấm dùng tool.

## Khai báo nhiều tool cùng lúc

Mảng `tools` không giới hạn ở 1 phần tử — khai báo càng nhiều tool, Claude càng có nhiều lựa chọn để tự đối chiếu với yêu cầu người dùng và quyết định *tool nào, theo thứ tự nào*, dựa hoàn toàn vào phần `description` của từng tool.

Cấu trúc [[Vòng lặp Agent|vòng lặp]] không đổi — chỉ cần mở rộng 2 chỗ: thêm khai báo JSON vào mảng `tools`, và thêm một nhánh vào hàm điều hướng thực thi (`runTool`/`run_tool`) để `switch`/rẽ nhánh theo `name`:

```javascript
function runTool(name, input) {
  switch (name) {
    case "get_weather":
      return getWeather(input.city);   // thời tiết hôm nay
    case "get_forecast":
      return getForecast(input.city);  // dự báo nhiều ngày tới
  }
}
```

Muốn thêm tool thứ 3? Chỉ cần thêm 1 khai báo JSON + 1 nhánh `case` mới — toàn bộ logic vòng lặp bên dưới giữ nguyên, không cần sửa.

**Ví dụ:** yêu cầu *"Chuẩn bị hành lý đi Denver 3 ngày"* — Claude tự nhận ra cần cả thời tiết hôm nay lẫn dự báo 3 ngày tới, nên gọi cả `get_weather` lẫn `get_forecast` rồi tổng hợp thành lời khuyên cụ thể (ví dụ: mặc nhiều lớp áo vì có tuyết nhẹ, sẽ ấm dần). Việc Claude nhận diện đúng cả 2 tool cần dùng phụ thuộc hoàn toàn vào `description` của mỗi tool có đủ rõ ràng hay không — một lần nữa khẳng định `description` là yếu tố quyết định độ chính xác của Agent.

## Gọi song song (parallel tool use)

Nếu nhiều tool độc lập nhau (như `get_weather` và `get_forecast` ở trên), Claude có thể trả về nhiều block `tool_use` trong cùng một response — giảm số vòng lặp cần thiết thay vì gọi tuần tự từng cái. Khi đó mỗi block mang `id` riêng, và từng `tool_result` gửi lại phải khớp đúng `tool_use_id` tương ứng (xem [[#Vòng đời một lượt gọi tool_use_id|phần tool_use_id]]).

## Xử lý lỗi

Khi tool thực thi lỗi, vẫn trả `tool_result` như bình thường nhưng đánh dấu `is_error: true` kèm mô tả lỗi, để Claude tự điều chỉnh (thử cách khác, hỏi lại người dùng) thay vì làm gãy luồng hội thoại.

## Tích hợp vào hệ thống thực tế (Production)

Không cần viết mã nguồn mới riêng cho AI — Tool thường chỉ là **lớp bọc mỏng (thin wrapper)** quanh các hàm/dịch vụ đã có sẵn trong ứng dụng (hàm truy vấn CSDL, gọi service nội bộ...).

Ví dụ: một **Compliance Agent** (kiểm định tuân thủ pháp lý) dùng thẳng các hàm `lookup_building_code`/`search_building_code` vốn đã tồn tại trong dự án — khi dùng [[Tool Runner]], chỉ cần truyền thẳng các hàm này vào là Agent tự tra cứu và dẫn chứng đúng điều khoản pháp lý.

## Recap

| Nội dung | Chi tiết cần nhớ |
| --- | --- |
| Bản chất của Tool | Cho Claude kết nối hệ thống bên ngoài: bạn định nghĩa hàm → Claude chọn khi nào gọi → code của bạn trực tiếp thực thi. |
| Cấu trúc Tool | JSON Schema gồm `name`, `description`, `input_schema`, truyền qua mảng `tools`. |
| Tầm quan trọng của `description` | Mô tả chung chung/mơ hồ là nguyên nhân chính khiến Agent gọi sai hoặc bỏ sót tool. |
| Dấu hiệu thực thi | `stop_reason: "tool_use"` → chạy hàm tương ứng → phản hồi bằng `tool_result` khớp đúng `tool_use_id`. |
| Tối ưu với [[Tool Runner]] | SDK (TypeScript, Python, Ruby) tự sinh Schema từ hàm code thật + tự quản lý toàn bộ vòng lặp. |
| Lựa chọn triển khai | Tự quản lý [[Vòng lặp Agent\|vòng lặp]], dùng [[Tool Runner]] của SDK, hoặc giao hẳn cho Managed Agents của Anthropic vận hành. |

## Liên kết

- Thuộc nhóm: [[Claude Platform]]
- Phân biệt với [[Skills]]: Tool = khả năng hành động (what Claude can do), Skill = quy trình/quy chuẩn thực hiện (how you want it done)
- Cơ chế lắp ráp nhiều lượt gọi tool thành hành vi agent: [[Vòng lặp Agent]]
- Kết nối tool từ server ngoài: [[Connectors (MCP)]]
- Tool Anthropic tự host & thực thi sẵn: [[Built-in Tools (Server Tools)]]
- Tool chạy trên máy bạn nhưng dùng schema dựng sẵn từ SDK: [[Client Tools]]
- SDK tự sinh `input_schema` từ code, khỏi gõ tay: [[Tool Runner]]
