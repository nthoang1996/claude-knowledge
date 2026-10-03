---
tags:
  - claude/cowork
aliases:
  - Distribution
  - Phân phối Plugin
  - Chia sẻ Plugin cho team
  - Scale workflows
up: "[[Claude Cowork]]"
---

# Phân phối Plugin cho Tổ chức (Distribution)

Bước cuối của chuỗi *tạo → kiểm thử → đóng gói → **phân phối***: đưa [[Plugins|Plugin]] đến **tất cả người cần dùng trong tổ chức** một cách nhanh, đồng bộ và đúng quy định — chuyển từ cá nhân hóa sang **chuẩn hóa ở cấp nhóm/doanh nghiệp**.

```mermaid
graph LR
    A["Mẹo cá nhân<br/>của vài thành viên"] --> B["Skill<br/>đã qua evals"]
    B --> C["Đóng gói<br/>thành Plugin"]
    C --> D["Phân phối<br/>cho toàn tổ chức"]
```

## Bối cảnh: vì sao cần phân phối

- **Nguồn gốc:** các [[Skills|Skill]] xuất phát từ thói quen/mẹo làm việc của một vài thành viên.
- **Đã kiểm chứng:** qua [[Đánh giá Skill & Plugin bằng Evals|evals]] kỹ lưỡng, hiệu quả ổn định trên nhiều trường hợp dùng.
- **Kết quả:** team có tập Skill chất lượng cao, đáng tin cậy → xứng đáng được chia sẻ rộng.

## Đóng gói rồi mới phân phối

- Cách tốt nhất để chia sẻ nhiều Skill là **gói thành một Plugin** (xem [[Tự xây dựng Plugin (Cowork)]]; khoảng 3–4 skills + connectors là đáng chia sẻ).
- Bài này tập trung vào bước kế: **làm sao Plugin đến tay mọi người** — nhanh (không cài tay từng máy), **đồng bộ** (cùng một phiên bản, cập nhật một nơi cả team nhận), **đúng quy định** (admin kiểm soát ai được dùng gì).

## Private Marketplace (thị trường nội bộ)

- Với công ty lớn, cách chia sẻ được khuyến nghị là **danh mục riêng của tổ chức** — kho công cụ do **Admin quản lý và kiểm duyệt**.
- **Quy trình bàn giao:** tác giả **không tự đăng** lên toàn công ty, mà **bàn giao file/mã nguồn plugin** cho người quản lý danh mục (Trưởng nhóm, Trưởng bộ phận Vận hành/Hỗ trợ, hoặc IT) để họ **kiểm duyệt rồi publish**.

```mermaid
graph LR
    A["Tác giả<br/>bàn giao plugin"] --> B["Admin / người quản lý<br/>danh mục kiểm duyệt"]
    B --> C["Publish lên<br/>Private Marketplace"]
    C --> D["Nhân viên thấy<br/>trong Directory"]
```

## 3 chế độ triển khai

Khi publish, Admin chọn một trong ba chế độ:

| Chế độ | Mô tả | Trải nghiệm người dùng |
| --- | --- | --- |
| **Available** | Xuất hiện trong thư mục công ty | Nhân viên **tự chọn cài** nếu cần |
| **Installed by default** | Tự cài sẵn cho toàn bộ nhân viên | Có sẵn, nhưng **có thể tắt** nếu không dùng |
| **Required** | Tự cài và **không thể tắt** | Cho quy trình bắt buộc (tuân thủ, bảo mật) phải thực hiện giống nhau |

- Chọn theo mức ràng buộc: tiện ích tùy chọn → Available; muốn phổ cập nhưng vẫn linh hoạt → Installed by default; bắt buộc đồng nhất → Required.

## Trải nghiệm của đồng nghiệp (end-user)

- Plugin hiện trong **Directory** với **nhãn xác nhận nguồn từ chính công ty**, nằm cạnh các plugin công cộng của Anthropic (xem thêm [[Tự xây dựng Plugin (Cowork)#Trước khi tạo: kiểm tra Directory|kiểm tra Directory]]).
- Người dùng **dùng được (và tắt được nếu không phải Required)** nhưng **không sửa được** mã nguồn/chỉ dẫn bên trong.
- **Cập nhật tự đồng bộ** từ người bảo trì (**Maintainer**) — sửa một nơi, cả tổ chức nhận bản mới. Vì vậy cần có người chịu trách nhiệm bảo trì và quản lý phiên bản rõ ràng (`version` trong manifest).

## Xác định tình huống tổ chức của bạn

Tùy hệ thống IT của công ty:

- Đã có Marketplace và quy trình bàn giao rõ ràng → bàn giao plugin theo quy trình đó.
- Chưa bật tính năng này → cần trao đổi với Admin/IT để bật hoặc tìm kênh chia sẻ thay thế.
- Bài gốc có công cụ tương tác giúp xác định thực trạng — **chưa ghi** trong vault.

> [!note] Plugin có thể truy cập dữ liệu
> Plugin gom cả connectors/script nên việc kiểm duyệt trước khi publish là bước quan trọng, nhất là với chế độ Installed by default / Required.

## 4 thói quen bảo trì Plugin dùng chung

Mục tiêu: tránh Plugin bị **"mốc"** — lỗi thời, không còn chính xác hoặc không ai dùng.

```mermaid
graph LR
    A["1. Một chủ sở hữu"] --> B["2. Evals trước mỗi<br/>lần publish"]
    B --> C["3. Đặt tên<br/>cụ thể"]
    C --> D["4. Rà soát<br/>hàng quý"]
```

| Thói quen | Nội dung cần nhớ |
| --- | --- |
| **One owner** | Mỗi plugin có **một người phụ trách cụ thể (named person)**: duyệt thay đổi, chạy lại [[Đánh giá Skill & Plugin bằng Evals\|evals]] sau khi sửa, quyết định khi nào cập nhật hoặc **retire** (gỡ bỏ) |
| **Evals before every publish** | Coi evals là **cổng kiểm soát (gate)**: sửa xong mà kịch bản quan trọng không đạt → **không đẩy bản cập nhật** ra toàn công ty |
| **Name specifically** | Tên chung chung dễ **va chạm (collision)** với plugin khác trong thư mục công ty. Vd: `meeting-prep` ❌ → `sales-customer-renewal-prep` ✔ |
| **Review rhythm** | Rà soát **hàng quý**: plugin nào đang cài và có người dùng thật; **gỡ** plugin không ai dùng; **cải tiến** theo phản hồi thực tế |

- Liên quan chế độ triển khai: plugin **Installed by default / Required** ảnh hưởng toàn công ty nên càng cần owner rõ và evals chặt trước khi publish bản mới.

## Liên hệ Claude Code

Trong [[Claude Code]], hình thức tương đương là **marketplace** (thường là repo Git) — xem [[Plugins#Plugin trong Claude Code|Plugin trong Claude Code]]. Phân biệt với cài plugin cá nhân ở [[Cài đặt & Tùy biến Plugins (Cowork)]].

## Liên kết

- Thuộc nhóm: [[Claude Cowork]]
- Khái niệm nền: [[Plugins]], [[Skills]]
- Bước trước đó: [[Đánh giá Skill & Plugin bằng Evals]], [[Tự xây dựng Plugin (Cowork)]]
- Xem thêm: [[Cài đặt & Tùy biến Plugins (Cowork)]], [[Tích lũy Giá trị theo Thời gian (Compounding Value)]]
