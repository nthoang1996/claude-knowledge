---
tags:
  - claude/tich-hop
aliases:
  - Claude for Excel
up: "[[Bộ tiện ích tích hợp]]"
---

# Claude for Excel

Thao tác, phân tích và tạo công thức/dữ liệu trực tiếp trong Excel.

## Trường hợp sử dụng chính

- **Viết & giải thích công thức** — công thức phức tạp (VLOOKUP/XLOOKUP, hàm mảng, điều kiện lồng nhau) kèm giải thích logic.
- **Sửa lỗi công thức** — chẩn đoán và fix lỗi tham chiếu (`#REF!`, circular reference...).
- **Chạy kịch bản thử nghiệm (scenario test)** — thử what-if trên số liệu mà không phá vỡ mô hình gốc.
- **Làm sạch dữ liệu** — chuẩn hóa định dạng, loại bỏ trùng lặp, tách/gộp cột từ dữ liệu thô.
- **Tổng hợp & trực quan hóa** — tạo bảng tổng hợp (pivot table), biểu đồ nhanh từ dữ liệu có sẵn.
- **Tạo sheet mới từ mẫu có sẵn** — dựng sheet theo đúng cấu trúc/template đã dùng trong workbook.
- **Đọc hiểu bảng tính có sẵn** — giải thích logic, công thức của một bảng tính phức tạp do người khác tạo, trích dẫn chính xác vị trí ô (cell reference).
- **Tự động hóa thao tác lặp lại** — gợi ý công thức/macro cho các tác vụ lặp đi lặp lại theo mô tả.

## Tác vụ mạnh nhất (Strongest move)

> "Lấy actuals từ sheet Q3, so sánh với kế hoạch Q3 trong cùng workbook, rồi viết variance commentary vào cột F ngay bên cạnh từng mục."

Điểm mạnh: xử lý xuyên nhiều sheet trong cùng workbook, tự định vị đúng ô để chèn nhận xét — không cần người dùng chỉ tay từng bước.

## Liên kết

- Thuộc nhóm: [[Bộ tiện ích tích hợp]]
- Xem thêm: [[Claude for Slack]], [[Claude Design]], [[Claude for PowerPoint]], [[Claude for Word]], [[Claude for Outlook]], [[Claude in Chrome]]
- Cơ chế tích hợp chung (cross-app context, phân biệt với Cowork): [[Claude trong M365 (Cơ chế tích hợp)]]
