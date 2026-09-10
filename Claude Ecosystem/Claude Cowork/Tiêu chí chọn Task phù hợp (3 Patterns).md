---
tags:
  - claude/cowork
aliases:
  - Tiêu chí chọn Task phù hợp cho Cowork
  - 3 Patterns Cowork
up: "[[Claude Cowork]]"
---

# Tiêu chí chọn Task phù hợp cho Cowork

Cowork phát huy tối đa khi task khớp một hoặc nhiều trong **3 mẫu (patterns)** sau — dùng để quyết định nhanh có nên giao việc cho Cowork thay vì Chat hay không.

## 3 mẫu công việc chuẩn

1. **Task đa bước** — chuỗi xử lý liên hoàn (thu thập bối cảnh → so sánh nguồn → nghiên cứu thêm → viết bản thảo → định dạng) gói gọn trong 1 prompt duy nhất. Người dùng chuyển từ vai "thợ" (tự tay chuyển đổi giữa từng công cụ, tự nhớ từng bước) sang vai "quản lý" (giao trọn cả chuỗi việc — *the whole arc*). Xem cơ chế theo dõi thực thi tại [[Progress Panel (Bảng tiến độ)]].
   - VD: phân loại email phản hồi khách hàng theo chủ đề kèm trích dẫn minh họa; trích số liệu từ 3 báo cáo + 1 spreadsheet để dựng dashboard tương tác.
2. **Task gắn với file thực tế trên máy** — đầu vào là file có sẵn (Word/Excel/PDF/PPT), đầu ra là artifact thực lưu đúng định dạng vào thư mục. Khác Chat ở chỗ Cowork **đọc, chỉnh sửa và ghi đè/lưu mới trực tiếp** trên file hiện có, chứ không chỉ tạo văn bản/file mới.
   - VD: ghép file template chuẩn + ghi chú cuộc họp thành client proposal hoàn chỉnh; lập báo cáo chỉ số tháng từ Excel thô, tự chèn biểu đồ.
3. **Task kết nối đa công cụ** — công việc phải đi qua nhiều app khác nhau (Gmail, Slack, Calendar, CRM, Drive); Cowork lập kế hoạch xuyên suốt các app này và tự thực thi chuỗi hành động, thay vì bắt người dùng gõ lệnh từng bước.
   - VD: soạn email follow-up dựa trên thông tin mời họp + người tham gia (Calendar) + ghi chú họp; quét Slack tìm thảo luận về ra mắt sản phẩm rồi tổng hợp bản cập nhật.

## Bảng tóm tắt nhanh

| Tiêu chí | Dấu hiệu nhận biết task phù hợp |
| --- | --- |
| Quy trình | Cần ≥ 2-3 bước (Đọc → Tổng hợp → Tạo file) |
| Tệp tin | Cần sửa file cũ hoặc xuất ra file Word/Excel/Deck thực, lưu vào máy |
| Ứng dụng | Cần lấy thông tin đồng thời từ ≥ 2 app (vd: Slack + Drive + Gmail) |

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Anatomy của một Task chuẩn]], [[Chuyển dịch tư duy Chat sang Cowork]], [[Progress Panel (Bảng tiến độ)]], [[Công thức Prompt chuẩn (Deliverable + Inputs + Nuances)]]
