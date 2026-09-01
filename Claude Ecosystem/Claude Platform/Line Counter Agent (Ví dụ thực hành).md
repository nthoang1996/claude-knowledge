---
tags:
  - claude/nen-tang
aliases:
  - Line Counter Agent
  - Smallest Possible Managed Agent
up: "[[Managed Agents]]"
---

# Line Counter Agent (Ví dụ thực hành)

Ví dụ tối giản nhất có thể (*the smallest possible managed agent*) — minh hoạ từng bước quy trình dựng một [[Managed Agents|Managed Agent]] hoàn chỉnh: tạo một file tạm, đếm số dòng trong file, và báo cáo lại kết quả.

## Bước 1: Tạo Agent

Thay vì tự viết schema cho từng tool (tạo file, đọc file, chạy lệnh terminal...), dùng bộ tool đóng gói sẵn do Anthropic cung cấp:

```python
agent = client.beta.agents.create(
    name="Line Counter",           # Tên định danh của Agent
    model="claude-opus-4-8",       # Mô hình LLM đảm nhiệm suy luận
    system="You are...",           # System prompt quy định vai trò/persona
    tools=[                        # Khai báo bộ công cụ tích hợp sẵn
        {"type": "agent_toolset_20260401", "default_config": {"enabled": True}}
    ],
)
```

- **`agent_toolset_20260401`** — bundled toolset có sẵn của Anthropic: thao tác file system, chạy lệnh Bash, tìm kiếm web. Bật toàn bộ bằng `"default_config": {"enabled": True}`, không cần tự định nghĩa schema cho từng tool.
- **`agent` là bản thiết kế tĩnh (state-less blueprint)** — tạo **một lần duy nhất**, sau đó dùng ID của nó để khởi chạy hàng ngàn [[Managed Agents#4 Thành phần cốt lõi (The Four Primitives)|Session]] khác nhau; bản thân đối tượng `agent` chưa trực tiếp chạy nhiệm vụ nào.

| Thuộc tính | Ý nghĩa |
| --- | --- |
| `name` | Tên gợi nhớ, hiển thị trên [[Console]]/Dashboard |
| `model` | Model đảm nhiệm suy luận & gọi tool |
| `system` | System prompt quy định vai trò/persona |
| `tools` | Nạp bộ tool tích hợp sẵn, không cần tự code schema |

## Bước 2: Tạo Environment (khởi tạo Sandbox)

```python
environment = client.beta.environments.create(
    name="line-counter-env",
    config={
        "type": "cloud",
        "networking": {"type": "unrestricted"},
    },
)
```

- **Bản chất** — yêu cầu hạ tầng Anthropic dựng một container cô lập (sandbox) trên cloud; đây là không gian lưu trữ và thực thi thực tế cho các file tạm.
- **`networking: {"type": "unrestricted"}`** — cho container quyền truy cập mạng không giới hạn (tải thư viện, truy cập web khi cần).

## Bước 3: Tạo Session (khởi tạo phiên làm việc)

```python
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
    title="Count lines demo",
)
```

- **Bản chất** — Session là **đơn vị công việc (unit of work)**: nối bản thiết kế Agent (`agent.id` ở Bước 1) vào Environment thực thi (`environment.id` ở Bước 2) để sẵn sàng chạy nhiệm vụ.

## Bước 4: Mở Event Stream và gửi tín hiệu kích hoạt (Kickoff)

Bước quan trọng nhất, thể hiện rõ nhất sự chuyển dịch tư duy sang [[Managed Agents#Sự chuyển dịch tư duy: từ vòng lặp sang Event Stream|Event Stream]]:

```python
# 1. Mở luồng lắng nghe Event TRƯỚC
with client.beta.sessions.events.stream(session_id=session.id) as stream:
    # 2. Sau khi luồng đã mở, mới gửi tin nhắn của User vào
    client.beta.sessions.events.send(
        session_id=session.id,
        events=[
            {
                "type": "user.message",
                "content": [
                    {
                        "type": "text",
                        "text": "Create a file in the temp directory, count its lines, and report back.",
                    }
                ],
            }
        ],
    )
```

**Quy tắc vàng (Golden Rule):**

- **Phải mở stream TRƯỚC khi gửi event** — stream chỉ phân phối các event xảy ra *sau khi* kết nối đã thiết lập; gửi tin nhắn trước rồi mới mở stream có thể làm mất các event phản hồi đầu tiên của Agent.
- **Luôn gửi dạng danh sách (`events`, số nhiều)** — mọi dữ liệu vào/ra API này đều đóng gói dưới dạng mảng event, cho phép gửi nhiều thông điệp/dữ liệu đầu vào cùng lúc.

## Bước 5: Đọc và xử lý Event Stream

Sau khi kích hoạt Session ở Bước 4, ứng dụng của bạn chỉ cần lắng nghe dòng event đổ về để cập nhật trạng thái theo thời gian thực — không tự chạy logic gì thêm:

```python
for event in stream:
    # 1. Văn bản phản hồi -> in ra màn hình ngay lập tức
    if event.type == "agent.message":
        for block in event.content:
            if block.type == "text":
                print(block.text, end="", flush=True)

    # 2. Sự kiện gọi tool -> in tên tool đang dùng
    elif event.type == "agent.tool_use":
        print(f"\n[tool] {event.name}")

    # 3. Agent làm xong -> in thông báo và ngắt vòng lặp
    elif event.type == "session.status_idle":
        print("\n--- Agent done ---")
        break
```

3 loại event quan trọng nhất cần lọc:

- **`agent.message`** — các đoạn text Claude sinh ra (suy luận, câu trả lời cuối); dùng để in kết quả.
- **`agent.tool_use`** — phát ra khi Claude kích hoạt một tool (tạo file, chạy Bash...); cho biết Agent đang thao tác gì trong Container.
- **`session.status_idle`** — báo Agent đã xử lý xong, quay về trạng thái nghỉ; là tín hiệu để `break` khỏi vòng lặp đọc stream.

**Ý nghĩa kiến trúc** — toàn bộ việc tạo file tạm, chạy lệnh đếm dòng, xử lý logic đều diễn ra bên trong Container của Anthropic; server của bạn **chỉ hiển thị kết quả** từ event đổ về, không tốn tài nguyên tính toán/lưu trữ cho phần việc đó.

## Tổng hợp quy trình 5 bước

| Bước | Thành phần | Hành động chính | Tương đương thực tế |
| --- | --- | --- | --- |
| 1 | Agent | Định nghĩa model, prompt, tools | Lập bản thiết kế (blueprint) |
| 2 | Environment | Dựng container cloud sandbox | Chuẩn bị máy chủ / Docker container |
| 3 | Session | Gắn Agent vào Environment | Tạo một tiến trình / job mới |
| 4 | Events (gửi) | Mở stream kết nối, gửi yêu cầu công việc | Kích hoạt và lắng nghe kết quả real-time |
| 5 | Events (đọc) | Lọc `agent.message`/`agent.tool_use`/`session.status_idle`, in kết quả, `break` khi xong | Consumer đọc queue/WebSocket |

## Liên kết

- Ví dụ minh hoạ cho: [[Managed Agents]]
- Primitive liên quan: [[Managed Agents#4 Thành phần cốt lõi (The Four Primitives)|Agent, Session, Environment, Events]]
