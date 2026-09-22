---
tags:
  - claude/tich-hop
aliases:
  - Claude in Chrome
  - Claude for Chrome
up: "[[Bộ tiện ích tích hợp]]"
---

# Claude in Chrome

Tiện ích mở rộng (extension) thêm thanh bên Claude trực tiếp vào trình duyệt Google Chrome, đóng vai trò như một trợ lý duyệt web tự động có khả năng quan sát màn hình và thao tác thay người dùng trên trình duyệt.

## Trường hợp sử dụng chính

- **Tóm tắt nội dung khi duyệt web** — tóm tắt nhanh bài báo, tài liệu nghiên cứu, trang web đang mở mà không cần sao chép văn bản sang nơi khác.
- **Soạn thảo email & quản lý hộp thư** — trợ giúp viết email phản hồi, sắp xếp hòm thư trực tiếp trong giao diện Gmail/Outlook trên web.
- **Tự động điền form lặp đi lặp lại** — tự đọc dữ liệu và điền vào các mẫu đăng ký, form nhập liệu dài.
- **Kiểm thử website & điều hướng quy trình đa bước** — thay người dùng nhấp chuột qua từng bước để kiểm tra tính năng web hoặc thực hiện quy trình lặp lại trên trình duyệt.
- **Trợ lý kết nối ngữ cảnh giữa các tab** — nhớ và liên kết thông tin khi chuyển đổi qua lại giữa các tab (CRM, công cụ nội bộ, dashboard).

## Ứng dụng thực tế mở khóa (What this unlocks)

Bản chất: tự động hóa mọi quy trình mà trước giờ vẫn cần con người tự mở web, đăng nhập, thao tác thủ công để lấy/nhập dữ liệu.

- **Báo cáo nội bộ (BI dashboard):** tự xem số liệu trên Tableau/Looker hay hệ thống BI nội bộ (vốn phải đăng nhập, giao diện phức tạp), tải dữ liệu về rồi đưa sang Cowork xử lý tiếp (vd: dựng báo cáo tuần).
- **Cổng đối tác & CRM không có API:** procurement portal, hệ thống sau SSO không hỗ trợ kết nối phần mềm thông thường — Claude thao tác qua chính trình duyệt đã đăng nhập sẵn của bạn, tự điều hướng, đọc ticket/dữ liệu như một nhân viên thật.
- **Web app nội bộ tự phát triển (sau login):** biến bất kỳ trang web nào thành "kịch bản" tự động hóa bằng câu lệnh tự nhiên — vd: *"Mở hệ thống mua sắm, tìm tất cả PO từ 10 nhà cung cấp hàng đầu Quý 3, tổng hợp danh mục vào Excel"*.
- **Nghiên cứu đa tab ra thẳng sản phẩm:** đọc song song hàng chục tab, trích ý chính, tổng hợp thành bản brief hoàn chỉnh — bỏ qua bước copy-paste thủ công từng tab.

> Mô hình cốt lõi: hễ nghĩ *"ước gì đưa được dữ liệu này cho Claude, nhưng nó đang nằm trên một trang web"* — đó là lúc dùng Claude in Chrome.

## Lưu ý an toàn (Security Notice)

- **Trạng thái:** đang ở bản thử nghiệm công khai (Public Beta).
- **Khuyến nghị:** Anthropic khuyên chỉ nên dùng cho các công việc rủi ro thấp trên các trang web tin tưởng.
- **Cơ chế bảo vệ:**
  - Luôn hỏi xin phép trước khi thực hiện hành động rủi ro cao (thanh toán, mua hàng, chia sẻ dữ liệu cá nhân).
  - Các trang web nhạy cảm (dịch vụ tài chính, ngân hàng, nội dung người lớn) mặc định bị chặn hoạt động.

## Lưu ý khi dùng (Watch-outs)

- **Phải tự đăng nhập trước:** Claude không tự nhập tài khoản/mật khẩu hay vượt qua xác thực (OTP, Passkey) thay bạn — nó chỉ thao tác dựa trên **phiên đăng nhập (session) sẵn có** sau khi bạn tự đăng nhập bình thường trên Chrome.
- **Cẩn trọng phạm vi truy cập:** Claude nhìn thấy và thao tác được trên **mọi thứ hiển thị trên màn hình trình duyệt** (kể cả dữ liệu nội bộ, cá nhân, tài chính mà tài khoản bạn có quyền xem) — nên giới hạn phạm vi hoạt động ở các trang nhạy cảm, và luôn rà soát kỹ danh sách hành động trước khi bấm **Approve**.
- **Hướng dẫn thiết lập:** tham khảo tài liệu chính thức *"Get started with Claude in Chrome"* của Anthropic để cài đặt/cấu hình đúng chuẩn.

## Liên kết

- Thuộc nhóm: [[Bộ tiện ích tích hợp]]
- Xem thêm: [[Claude for Slack]], [[Claude Design]], [[Claude for Excel]], [[Claude for PowerPoint]], [[Claude for Word]], [[Claude for Outlook]]
- Vai trò trong Cowork (khi app không có Connector): [[Thêm & Quản lý Connectors (Cowork)]]
