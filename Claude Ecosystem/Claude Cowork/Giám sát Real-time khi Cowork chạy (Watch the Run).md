---
tags:
  - claude/cowork
aliases:
  - Giám sát Real-time Cowork
  - Watch the Run
  - 3 Hàng rào An toàn Real-time
up: "[[Claude Cowork]]"
---

# Giám sát Real-time trong lúc Cowork chạy

Sau khi đã thiết lập [[Chọn Thư mục & Phân quyền Đọc-Ghi|thư mục an toàn]] và [[Viết Prompt An toàn (Tránh Hiểu Lầm)|viết prompt rõ ràng]], đây là hàng rào kiểm soát cuối cùng: quan sát trực tiếp trong lúc Claude đang thực thi để phát hiện và ngăn sai sót sớm, thay vì chỉ nghiệm thu sau khi xong.

## 1. Đọc kế hoạch ngay khi Claude vừa tạo xong (Read the plan)

Khi bắt đầu một task, Claude hiển thị kế hoạch từng bước trên [[Progress Panel (Bảng tiến độ)|Progress Panel]]. Lướt qua ngay để kiểm tra: thứ tự các bước có hợp lý không, Claude có đang dùng đúng nguồn dữ liệu không — can thiệp trước khi Claude bắt tay vào làm nếu thấy kế hoạch lệch hướng.

## 2. Quan sát dấu hiệu bất thường (Watch for unexpected patterns)

Không cần soi từng dòng lệnh nhỏ, nhưng để ý hành vi tổng thể: Claude đụng vào tệp/trang web **không hề được nhắc tới**, hoặc phạm vi công việc bị "phình ra" (scope creep) so với yêu cầu ban đầu. Thấy "có gì đó sai sai" thì dừng tác vụ ngay qua [[Điều chỉnh Hướng đi Mid-task (Steer & Interrupt)|Queue/Interrupt]] — linh cảm này thường đúng và giúp chặn hậu quả lớn hơn.

## 3. Duyệt hộp thoại xác nhận một cách cẩn trọng (Approve deliberately)

Giữ chế độ **Ask before acting** ([[Mô hình Phân quyền (Permissions)|2 chế độ phê duyệt]]) cho các hành động gửi tin/đăng bài/chia sẻ dữ liệu, và đọc kỹ nội dung hộp thoại trước khi bấm duyệt.

> Đa số sự cố nghiêm trọng không phải do hệ thống bảo vệ thất bại, mà do người dùng **bấm duyệt quá nhanh mà không đọc kỹ**. Xem hộp thoại xác nhận là ranh giới an toàn thật sự — chỉ đồng ý khi hoàn toàn chắc chắn đó là hành động mình muốn.

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Xem thêm: [[Progress Panel (Bảng tiến độ)]], [[Điều chỉnh Hướng đi Mid-task (Steer & Interrupt)]], [[Mô hình Phân quyền (Permissions)]], [[Viết Prompt An toàn (Tránh Hiểu Lầm)]], [[Khi nào KHÔNG nên dùng Cowork (Giới hạn Sử dụng)]]
