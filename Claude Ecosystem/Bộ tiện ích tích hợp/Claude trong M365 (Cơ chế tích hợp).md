---
tags:
  - claude/tich-hop
aliases:
  - Claude trong M365
  - Claude inside M365
  - M365
up: "[[Bộ tiện ích tích hợp]]"
---

# Claude trong M365 (Cơ chế tích hợp)

Mô tả cách Claude vận hành *xuyên suốt* các add-in Word/Excel/PowerPoint/Outlook, thay vì lặp lại use-case riêng từng app (xem [[Claude for Word]], [[Claude for Excel]], [[Claude for PowerPoint]], [[Claude for Outlook]]).

## Claude hiện diện ngay trong tài liệu

- Xuất hiện dưới dạng **add-in** ngay trong giao diện Word/Excel/PowerPoint/Outlook — thao tác trực tiếp trên file đang mở, không cần chuyển sang app/tab khác.

## Ngữ cảnh xuyên ứng dụng (cross-app context)

Điểm đột phá: ngữ cảnh dữ liệu được **truyền xuyên suốt** từ app này sang app khác trong cùng một cuộc trò chuyện — không phải thao tác trên từng tài liệu đơn lẻ, tách biệt.

```mermaid
graph LR
    A["Outlook<br/>(email yêu cầu)"] --> B["Word<br/>(bản ghi nhớ)"]
    B --> C["Excel<br/>(mô hình dữ liệu)"]
    C --> D["PowerPoint<br/>(slide báo cáo)"]
    D --> E["Outlook<br/>(lên lịch họp)"]
```

**Ví dụ luồng việc trọn vẹn:**

1. **Outlook → Word:** *"Mở bản yêu cầu này trong Word và tạo memo từ mẫu công ty."* — Word tự mở, tải file đính kèm, giữ nguyên nội dung thread email ở thanh bên; Claude đã hiểu yêu cầu khách hàng mà không cần copy-paste lại.
2. **Word → Excel:** *"Tạo mô hình định giá thị trường cho Phương án 2."* — Excel tự mở, Claude lấy assumptions từ chính bản Word vừa đọc để dựng file nhiều tab với công thức kiểm tra được.
3. **Excel → PowerPoint:** *"Chuyển dữ liệu này thành slide báo cáo Ban điều hành theo mẫu khách hàng."* — PowerPoint tạo slide theo đúng slide master, chèn chart dạng gốc (native/editable) cập nhật đúng số liệu vừa tính ở Excel.
4. **PowerPoint → Outlook:** *"Tìm khoảng trống 30 phút với cả đội trước thứ Tám."* — Outlook tự tạo lời mời họp, điền sẵn người tham gia và nội dung, chỉ chờ bấm Gửi.

**Takeaway:** thay vì làm thủ công Đọc email → Mở Word viết nháp → Mở Excel tính toán → Copy chart sang PowerPoint → Mở Outlook đặt lịch, Claude đóng vai người trợ lý nối liền cả chuỗi thao tác đó trong một trải nghiệm liền mạch, không phải copy-paste hay giải thích lại ngữ cảnh ở mỗi bước.

## Phân biệt với [[Claude Cowork]]

| Tiêu chí | Claude trong M365 | [[Claude Cowork]] |
| --- | --- | --- |
| Vai trò chính | Chỉnh sửa/hoàn thiện/debug trên **file đang làm việc trực tiếp** | Xây sản phẩm hoàn chỉnh (finished deliverables) tổng hợp từ **nhiều nguồn** |
| Ngữ cảnh | Trích xuất thông tin từ file hiện tại, chuyển đổi sang tài liệu Office khác | Kết nối đa nền tảng (Web, Slack, Drive, CRM...) để tạo đầu ra mới từ đầu |
| Trải nghiệm | Làm việc ngay trong giao diện Word/Excel/PPT/Outlook | Không gian làm việc riêng để giao việc điều phối diện rộng |

**Ghi nhớ:** M365 = *tinh chỉnh tại chỗ* trên file đang mở; Cowork = *điều phối, tổng hợp* từ nhiều nguồn thành sản phẩm mới.

### Quy tắc chọn dùng cái nào

| | [[Claude Cowork]] | Claude trong M365 |
| --- | --- | --- |
| **Khi nào dùng** | Cần thu thập dữ liệu từ **rất nhiều nguồn** (Drive, Slack, CRM, Web...) để tạo sản phẩm hoàn chỉnh từ đầu | Đang **thao tác trực tiếp trên tệp Office** — chỉnh sửa tại chỗ hoặc chuyển ngữ cảnh qua lại giữa các tệp |
| **Ví dụ** | Tổng hợp báo cáo từ 20 file nguồn; gom dữ liệu CRM + 3 kênh Slack; chạy quy trình tự động theo lịch | Tinh chỉnh bảng tính Excel đang mở; viết lại một đoạn trong Word; chuyển số liệu Excel sang slide PowerPoint |

### Phối hợp trong thực tế (handoff)

Phần lớn công việc thực tế dùng **cả hai**, bàn giao (handoff) mượt mà theo 2 giai đoạn:

1. **Tạo bản thảo — dùng Cowork:** giao Cowork gom dữ liệu từ nhiều nguồn để dựng bản nháp đầu tiên (ví dụ bộ slide báo cáo).
2. **Tinh chỉnh & hoàn thiện — dùng M365:** mở file vừa tạo ngay trong PowerPoint/Excel, dùng Claude inside M365 để sửa từng trang, đổi bố cục, hoặc rà lại số liệu bất thường tại chỗ.

**Takeaway:** tệp dữ liệu vẫn là một ("the file is the file") — Cowork là "xưởng sản xuất" gom dữ liệu từ bên ngoài, M365 là "bàn làm việc" để gọt giũa, hoàn thiện trực tiếp trên chính tệp đó.

## Liên kết

- Thuộc nhóm: [[Bộ tiện ích tích hợp]]
- Các add-in cụ thể: [[Claude for Word]], [[Claude for Excel]], [[Claude for PowerPoint]], [[Claude for Outlook]]
- Phân biệt: [[Claude Cowork]], [[So sánh 3 chế độ làm việc (Chat, Cowork, Code)]]
