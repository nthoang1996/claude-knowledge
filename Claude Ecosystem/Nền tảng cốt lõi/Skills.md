---
tags:
  - claude/nen-tang
aliases:
  - Skills
  - Quy trình
up: "[[Nền tảng cốt lõi]]"
---

# Skills (Quy trình)

Các quy trình/hướng dẫn được đóng gói sẵn (packaged instructions) mà Claude có thể **gọi ra khi cần** để thực hiện một tác vụ lặp lại theo đúng chuẩn đã định nghĩa trước.

- Thay vì giải thích lại quy trình từ đầu mỗi lần, người dùng định nghĩa Skill một lần rồi tái sử dụng.
- Ví dụ: quy trình review code, quy trình tạo báo cáo theo template cố định, checklist triển khai...
- Có thể được kích hoạt tự động khi Claude nhận thấy tác vụ phù hợp, hoặc gọi thủ công qua lệnh.

## Cấu trúc: thư mục Skill + SKILL.md

Một Skill là **một thư mục đóng gói sẵn**, gồm hướng dẫn (instructions), script và tài nguyên (resources) liên quan. Trung tâm của thư mục là file **`SKILL.md`** — nơi định nghĩa quy trình, tiêu chuẩn và định dạng đầu ra mong muốn.

- **Ở tầng API/developer:** upload thư mục Skill lên hệ thống một lần, sau đó **attach** vào bất kỳ lệnh gọi `messages.create` nào cần dùng — không phải khai báo lại mỗi lần gọi.
- **Bản chất:** dạy Claude *cách bạn làm việc* — Claude đọc Skill, làm theo đúng từng bước và trả kết quả theo đúng định dạng/phong cách đã định nghĩa.
- Ví dụ ứng dụng: mẫu Status Report (mục cần có, cách phân loại công việc, bảng biểu cố định), Review Checklist (các bước bắt buộc khi review code/tài liệu), Release Notes (quy tắc trích log Git/GitHub rồi tổng hợp theo văn phong công ty).

> Ví von: Skill giống một **SOP (Standard Operating Procedure)** dành riêng cho Claude — chuẩn hóa đầu ra AI trên toàn ứng dụng mà không cần lặp lại prompt dài dòng mỗi lần gọi.

## Phân biệt với Tool

Dễ nhầm vì cả hai đều là thứ Claude "gọi ra khi cần", nhưng phục vụ hai mục đích khác nhau:

| Đặc điểm | [[Tool Use (Function Calling)\|Tool]] | Skill |
| --- | --- | --- |
| Mục đích | Kết nối Claude với **dữ liệu/hành động** bên ngoài (query CSDL, gửi email, tìm code...) | Dạy Claude **quy trình/quy chuẩn** thực hiện công việc |
| Câu hỏi trả lời | Claude **CÓ THỂ LÀM** gì (*what Claude can do*) | **CÁCH** bạn muốn nó làm (*how you want it done*) |
| Ví dụ | "Tìm đoạn code này", "Gửi email này" | "Viết báo cáo hàng ngày đúng theo template này" |

Thực tế hai thứ thường đi cùng nhau: Skill mô tả *quy trình*, còn bên trong quy trình đó Claude vẫn có thể gọi Tool để lấy dữ liệu/thực thi hành động.

## Lazy loading — tối ưu context window

Khi có hàng chục Skill được cấp cùng lúc, Claude không nạp toàn bộ nội dung ngay từ đầu:

1. **Lúc khởi chạy:** chỉ nạp **tên + mô tả (name/description)** của tất cả Skill khả dụng vào context — rất nhẹ.
2. **Khi xác định cần dùng:** Claude mới đọc *toàn bộ* `SKILL.md` (và file liên quan) của đúng Skill đó, nạp vào context ngay lúc đó (dynamic loading).

Nhờ vậy context window luôn gọn nhẹ dù có rất nhiều Skill được đăng ký — tiết kiệm token, giảm chi phí, và giữ độ chính xác phản hồi (không bị nhiễu bởi hàng chục hướng dẫn không liên quan).

## Upload & tái sử dụng Skill (API)

Ở tầng developer, Skill được quản lý qua Beta SDK — upload một lần, gọi lại nhiều lần bằng ID:

```python
skill = client.beta.skills.create(
    display_title="Status Report Generator",
    files=files_from_dir("status-report-skill"),  # Thư mục chứa SKILL.md
)

print(skill.id)  # Lưu ID để dùng cho các lệnh messages.create sau này
```

- **`client.beta.skills.create`** — endpoint khởi tạo Skill mới (cũng có thể upload qua giao diện web Claude Platform thay vì code).
- **`display_title`** — tên hiển thị để quản lý Skill trên hệ thống.
- **`files_from_dir(...)`** — đóng gói toàn bộ nội dung thư mục (gồm `SKILL.md` và tài nguyên kèm theo) để gửi lên.
- Kết quả trả về **`skill.id`** duy nhất — lưu lại và truyền ID này vào các lệnh `messages.create` sau, không cần nạp lại toàn bộ file mỗi lần gọi.

### Tách biệt quy chuẩn (Skill) và dữ liệu (input)

Skill chỉ đóng gói **bộ khung/quy chuẩn cố định** (mục cần có, văn phong, cách tóm tắt, cách xử lý blocker...) — còn **dữ liệu thực tế** vẫn truyền riêng ở mỗi lần gọi, dưới dạng input đơn giản (ví dụ: activity log dạng text). Claude tự lấy khung Skill đã lưu để xử lý dữ liệu mới truyền vào — quy chuẩn không đổi, chỉ dữ liệu thay đổi theo từng lần gọi.

## Gắn Skill vào request (`container.skills`)

Skill **không** truyền qua tham số `tools` hay `messages` thông thường — mà cấu hình riêng trong đối tượng `container.skills`:

```python
response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=4096,
    betas=["skills-2025-10-02", "code-execution-2025-08-25"],  # 1. Beta header
    container={
        "skills": [                                            # 2. container.skills
            {
                "type": "custom",
                "skill_id": skill.id,
                "version": "latest",
            }
        ]
    },
    tools=[                                                    # 3. Kết hợp với Tool
        {
            "type": "code_execution_20250825",
            "name": "code_execution",
        }
    ],
    messages=[
        {
            "role": "user",
            "content": f"Generate the daily status report from this activity log:\n\n{activity_log}",
        }
    ],
)
```

- **Beta header bắt buộc** — Skills còn ở dạng Beta nên phải gọi qua `client.beta.messages.create` kèm cờ tương ứng trong `betas` (ví dụ `"skills-2025-10-02"`); dùng tool nào cũng ở dạng beta thì cộng dồn cờ đó vào cùng mảng (ví dụ `"code-execution-2025-08-25"`).
- **`container.skills`** — mảng, cho phép gắn **nhiều Skill cùng lúc** vào một request. Mỗi phần tử gồm `skill_id` (lấy từ bước upload) và `version` (ví dụ `"latest"`).
- **Kết hợp Skill + Tool:** Skill cung cấp *quy trình* (`SKILL.md`), còn Tool (`code_execution`) là *công cụ thực thi* — Claude dùng Code Execution để chạy script mà quy trình trong Skill mô tả, xử lý dữ liệu thực tế (activity log) truyền vào `messages`. Đây là minh hoạ rõ nhất cho phần [[#Phân biệt với Tool|phân biệt Skill/Tool]] ở trên: một bên định nghĩa cách làm, một bên thực thi hành động.

## Giá trị trong Production

- **Chuẩn hóa đầu ra:** user prompt chỉ cần một dòng (truyền dữ liệu thô), Claude tự bám khung `SKILL.md` để trả về kết quả đủ mục, đúng văn phong, đúng cách xử lý các trường hợp đặc biệt (ví dụ blocker trong báo cáo) — không lệch dù ai gọi, gọi bao nhiêu lần.
- **Tự động hóa, bỏ thao tác copy-paste prompt mẫu:** cả team dùng chung một Skill nên nhận kết quả cùng cấu trúc/thứ tự mục mà không ai phải tự nhớ và dán lại prompt chuẩn mỗi lần.

## Khi nào nên dùng Skill

Cân nhắc dùng Skill khi **cách thức thực hiện quan trọng ngang với kết quả đầu ra** — tức là có một quy chuẩn/định dạng cố định cần lặp lại nhất quán (báo cáo định kỳ, checklist review, release notes...). Nếu chỉ cần Claude *làm được* một việc (truy vấn, gọi API, thực thi hành động) mà không đòi hỏi quy chuẩn trình bày cố định, đó là việc của [[Tool Use (Function Calling)|Tool]] chứ không phải Skill.

## Recap

| Nội dung | Chi tiết cần nhớ |
| --- | --- |
| Bản chất | Đóng gói *cách thức* thực hiện (`SKILL.md` + resources) — dạy Claude quy trình/quy chuẩn, không phải khả năng hành động. |
| Khác Tool | Tool = *what Claude can do*, Skill = *how you want it done*. |
| Nạp context | Lazy loading — chỉ nạp tên/mô tả lúc đầu, nạp toàn bộ nội dung khi Claude xác định cần dùng. |
| Vòng đời API | `client.beta.skills.create` (upload 1 lần, nhận `skill.id`) → gắn vào `container.skills` (kèm `version`) trong các `messages.create` sau, cần cờ `betas`. |
| Kết hợp Tool | Skill mô tả quy trình, Tool (ví dụ `code_execution`) thực thi hành động cụ thể trong quy trình đó. |
| Khi nào dùng | Cách làm quan trọng ngang kết quả — cần chuẩn hóa lặp lại nhất quán trên toàn hệ thống. |

## Liên kết

- Thuộc nhóm: [[Nền tảng cốt lõi]]
- Xem thêm: [[Projects]], [[Artifacts]], [[Connectors (MCP)]]
- Ở góc nhìn hạ tầng dev, Skill là một trong các *Primitives* của [[Claude Platform]]
- Phân biệt với [[Tool Use (Function Calling)|Tool]]: Tool = khả năng hành động, Skill = quy trình/quy chuẩn
