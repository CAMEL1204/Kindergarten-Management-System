# UC-15 · Chốt và đối chiếu suất ăn theo bữa

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV07 · F1.7.3, F1.7.4 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Bếp (Bếp trưởng và nhân viên bếp/cấp dưỡng) |
| Mục tiêu | Chốt đúng số suất ăn theo lớp, ngày và từng bữa dựa trên chuyên cần và lịch về sớm do dị ứng; xử lý điều chỉnh sau chốt và đối chiếu số thực nhận với số cần giao. |
| Điều kiện trước | Thực đơn ngày đã được duyệt và thông báo qua [UC-18](uc-18-lap-duyet-va-thong-bao-thuc-don.md). Ban giám hiệu đã cấu hình bữa ăn và mốc chốt có hiệu lực cho ngày được chọn tại [UC-03](uc-03-cau-hinh-moc-gio-va-bua-an.md). Giáo viên được phân công lớp trong ngày (`NV05`) và nhận được danh sách từ `NV03`. |
| Điều kiện sau | Lưu tách biệt danh sách ăn theo bữa, số chốt ban đầu, các yêu cầu điều chỉnh và phần bếp xác nhận, số cần giao, số bếp giao, số lớp thực nhận và chênh lệch (kèm lý do) cho vận hành và báo cáo `NV09`. |

## Luồng sự kiện chính

1. Giáo viên chọn lớp, ngày và bữa (`Sáng`, `Trưa`, `Xế`). Trước mốc chốt của bữa, hệ thống hiển thị danh sách dự kiến để giáo viên rà soát: bữa sáng lấy trẻ có mặt tính đến hết mốc chốt bữa sáng có hiệu lực (mặc định `07:30`, gồm trẻ đến đúng mốc); bữa trưa và bữa xế dùng bản bàn giao buổi sáng có hiệu lực (mặc định `08:30`).
2. Hệ thống hiển thị thực đơn đã duyệt của bữa đó, các lưu ý dị ứng/chế độ ăn từ `NV01` (gồm cả cảnh báo `lưu ý chờ xác nhận` theo `QT-HS-06`) và lịch về sớm do phụ huynh đã báo ở `UC-18`. Giáo viên rà soát và đánh dấu các bữa trẻ không ăn tại trường (về sớm trước bữa bị dị ứng hoặc nghỉ giữa ngày), giữ nguyên các bữa còn lại của trẻ (`QT-SA-03`).
3. Trước mốc chốt, giáo viên chỉ rà soát và cập nhật danh sách dự kiến; khi đã lấy đủ dữ liệu đến hết **mốc chốt có hiệu lực của bữa** (`UC-03`), giáo viên xác nhận chốt chính thức. Hệ thống loại trừ dòng trùng (mỗi trẻ tối đa 1 lần trong 1 bữa theo `QT-SA-02`), lưu **số chốt ban đầu** và gửi sang Bếp. Mọi phát sinh sau mốc chốt đi qua luồng điều chỉnh ở Bước 4 (`QT-SA-04`).
4. Bếp chuẩn bị suất ăn theo **số cần giao** (`= số chốt ban đầu + tổng số điều chỉnh đã được Bếp xác nhận`). Khi giao tại lớp, Bếp ghi `số bếp giao` và giáo viên ghi `số lớp thực nhận`.
5. Hệ thống đối chiếu `số lớp thực nhận` với `số cần giao`: nếu khớp ghi nhận đã giao đủ; nếu lệch, ghi rõ số thiếu/thừa, lý do và người xác nhận (`QT-SA-05`), tổng hợp bảng kê vận hành theo kỳ cho `NV09`.

## Luồng thay thế và ngoại lệ

- **Bước 2 (thay đổi thực đơn hoặc thông tin dị ứng):** Thực hiện liên hệ phụ huynh và cập nhật lịch về sớm tại `UC-18`; nếu phát sinh sau khi bữa đã chốt, giáo viên chuyển ngay sang luồng gửi yêu cầu điều chỉnh cho Bếp ở Bước 3–4 và **giữ nguyên số chốt ban đầu** (`QT-SA-06`).
- **Bước 3–4 (yêu cầu điều chỉnh tăng/giảm sau mốc chốt theo `QT-SA-04`):** Khi có trẻ đến sau mốc chốt có hiệu lực của bữa, báo nghỉ muộn hoặc về sớm đột xuất sau giờ chốt, giáo viên gửi yêu cầu tăng hoặc giảm suất kèm lý do và thời điểm.
  - Bếp xem xét tiến độ chế biến để **xác nhận toàn bộ**, **xác nhận một phần** hoặc **từ chối** (số điều chỉnh xác nhận = 0), **kể cả khi đề nghị chuyên cần tương ứng ở NV03 đang chờ duyệt**.
  - Hệ thống chặn không cho xác nhận giảm vượt quá `số chốt ban đầu` (`số cần giao >= 0`). `Số chốt ban đầu` không bị ghi đè.
- **Bước 5 (phân biệt giảm được duyệt với thiếu suất theo `QT-SA-05`):** Ví dụ chốt 23 suất, giảm 1 được bếp xác nhận (số cần giao = 22) và lớp nhận 22 thì đánh dấu `Đủ`; nếu chốt 23, chưa có điều chỉnh giảm được xác nhận (số cần giao = 23) mà lớp chỉ nhận 22 thì đánh dấu `Thiếu 1` để đối chiếu lý do.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã lớp, ngày, bữa, thực đơn đã duyệt, mốc chốt, danh sách có mặt theo mốc chốt bữa sáng có hiệu lực (mặc định `07:30`) / bản bàn giao buổi sáng (mặc định `08:30`), lưu ý dị ứng (`NV01`), phản hồi và lịch về sớm (`UC-18`), giờ trả thực tế (`NV08`), yêu cầu điều chỉnh tăng/giảm, xác nhận của bếp, số bếp giao và số lớp thực nhận. | Danh sách trẻ ăn theo bữa, số chốt ban đầu, số điều chỉnh đề nghị và số bếp xác nhận, số cần giao, số bếp giao, số lớp thực nhận, chênh lệch và lý do, bảng kê vận hành kỳ cho NV09. |

## Liên phân hệ và truy vết

- **NV01 → NV07:** Cung cấp chỉ đọc lưu ý dị ứng/chế độ ăn còn hiệu lực và cảnh báo sớm lưu ý `Chờ xác nhận` (`QT-HS-06`).
- **NV03 → NV07:** Cung cấp danh sách có mặt theo mốc chốt bữa sáng có hiệu lực (mặc định `07:30`) cho bữa sáng và bản bàn giao buổi sáng (mặc định `08:30`) cho bữa trưa, bữa xế.
- **NV08 → NV07:** Cung cấp giờ trả thực tế để đối chiếu với lịch về sớm của bữa ăn.
- **NV07 → NV09:** Cung cấp số liệu vận hành theo kỳ (số chốt ban đầu, điều chỉnh, số cần giao, thực nhận, chênh lệch) để Bếp lập báo cáo suất ăn (`BC-04`) tại `UC-10`. Nếu điều chỉnh hoặc đối chiếu suất ăn làm thay đổi số liệu kỳ của báo cáo đã nộp, hệ thống giữ ảnh chụp và cảnh báo người lập (`Đã gửi` xử lý yêu cầu bổ sung; `Đã duyệt` chuyển sang `Cần cập nhật` theo `QT-BC-04`).
- **Phân tách với NV06:** Số suất ăn và điều chỉnh của NV07 chỉ phục vụ vận hành bếp; tiền ăn đối soát với phụ huynh ở NV06 căn cứ duy nhất vào số ngày `Có mặt` đã khóa của NV03 (`QT-HP-02`, `QT-SA-05`).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-01, YC-SA-02, YC-SA-03 | QT-SA-01, QT-SA-02, QT-SA-03, QT-HS-06 | NV01, NV03 → NV07 · Rà soát danh sách dự kiến trước mốc, chốt đủ dữ liệu tại mốc cấu hình có hiệu lực, đối chiếu dị ứng và gửi bếp. |
| YC-SA-04, YC-SA-05 | QT-SA-04, QT-SA-05, QT-SA-06, QT-HP-02, QT-BC-04 | NV08 → NV07 → NV09 · Điều chỉnh sau chốt, thay đổi thực đơn/dị ứng sau chốt, giao nhận, phân tách căn cứ tiền ăn NV06 và cập nhật báo cáo NV09. |
