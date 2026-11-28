# UC-15 · Chốt và đối chiếu suất ăn theo bữa

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV07 · F1.7.3, F1.7.4 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Bếp (tổ trưởng bếp và cấp dưỡng) |
| Mục tiêu | Chốt đúng số suất ăn theo lớp, ngày và từng bữa từ chuyên cần và lịch về sớm do dị ứng. Xử lý điều chỉnh sau chốt và đối chiếu số lớp thực nhận với số cần giao. |
| Điều kiện trước | Thực đơn ngày đã được duyệt và thông báo tại [UC-18](uc-18-lap-duyet-va-thong-bao-thuc-don.md). Bữa ăn và giờ chốt có hiệu lực đã cấu hình tại [UC-03](uc-03-cau-hinh-moc-gio-va-bua-an.md). Giáo viên được phân công lớp trong ngày (NV05) và nhận được danh sách từ NV03. |
| Điều kiện sau | Hệ thống lưu riêng danh sách ăn theo bữa, số chốt ban đầu, yêu cầu điều chỉnh và phần bếp xác nhận, số cần giao, số bếp giao, số lớp thực nhận và chênh lệch kèm lý do. Số liệu dùng cho vận hành và báo cáo NV09. |

## Luồng sự kiện chính

1. Giáo viên chọn lớp, ngày và bữa (`Sáng`, `Trưa`, `Xế`). Trước giờ chốt của bữa, hệ thống hiển thị danh sách dự kiến để rà soát. Bữa sáng lấy trẻ có mặt đến hết giờ chốt bữa sáng (mặc định 07:30), gồm trẻ đến đúng giờ chốt. Bữa trưa và bữa xế dùng bản bàn giao buổi sáng (mặc định 08:30) từ [UC-13](uc-13-chot-chuyen-can.md).
2. Hệ thống hiển thị thực đơn đã duyệt của bữa, lưu ý dị ứng và chế độ ăn từ NV01 (gồm cảnh báo lưu ý chờ xác nhận theo QT-HS-06) và lịch về sớm phụ huynh đã báo tại `UC-18`. Giáo viên đánh dấu các bữa trẻ không ăn tại trường, ví dụ về sớm trước bữa có món bị dị ứng. Các bữa còn lại của trẻ giữ nguyên (QT-SA-03).
3. Trước giờ chốt, giáo viên chỉ rà soát và cập nhật danh sách dự kiến. Khi đã có đủ dữ liệu đến hết giờ chốt của bữa, giáo viên xác nhận chốt chính thức. Hệ thống loại dòng trùng, mỗi trẻ tối đa một lần trong một bữa (QT-SA-02), lưu **số chốt ban đầu** và gửi bếp. Phát sinh sau giờ chốt đi qua luồng điều chỉnh (QT-SA-04).
4. Bếp chuẩn bị theo **số cần giao**, bằng số chốt ban đầu cộng các điều chỉnh bếp đã xác nhận. Khi giao tại lớp, bếp ghi số bếp giao, giáo viên ghi số lớp thực nhận.
5. Hệ thống đối chiếu số lớp thực nhận với số cần giao. Khớp thì ghi đã giao đủ. Lệch thì ghi số thiếu hoặc thừa, lý do và người xác nhận (QT-SA-05). Hệ thống tổng hợp bảng kê vận hành theo kỳ cho NV09.

## Luồng thay thế và ngoại lệ

- **Bước 2 (thực đơn hoặc thông tin dị ứng thay đổi):** Giáo viên liên hệ phụ huynh và cập nhật lịch về sớm tại `UC-18`. Nếu bữa đã chốt, giáo viên gửi yêu cầu điều chỉnh cho bếp và giữ nguyên số chốt ban đầu (QT-SA-06).
- **Bước 3–4 (điều chỉnh tăng hoặc giảm sau giờ chốt):** Trẻ đến sau giờ chốt của bữa, báo nghỉ muộn hoặc về sớm đột xuất. Giáo viên gửi yêu cầu tăng hoặc giảm suất kèm lý do và thời điểm (QT-SA-04).
  - Bếp xem tiến độ chế biến rồi xác nhận toàn bộ, xác nhận một phần hoặc từ chối (số xác nhận bằng 0). Việc này không phụ thuộc đề nghị chuyên cần tương ứng ở NV03 đang chờ duyệt.
  - Hệ thống chặn xác nhận giảm vượt số cần giao hiện tại, tức số chốt ban đầu cộng các điều chỉnh đã xác nhận trước đó, để số cần giao không âm. Số chốt ban đầu không bị ghi đè.
- **Bước 5 (phân biệt giảm được xác nhận với thiếu suất):** Xem hai ca cuối trong bảng ca kiểm thử (QT-SA-05).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã lớp, ngày, bữa. Thực đơn đã duyệt, giờ chốt. Danh sách có mặt theo giờ chốt bữa sáng, bản bàn giao buổi sáng (NV03). Lưu ý dị ứng (NV01). Phản hồi và lịch về sớm (`UC-18`). Giờ trả thực tế (NV08). Yêu cầu điều chỉnh, xác nhận của bếp, số bếp giao, số lớp thực nhận. | Danh sách trẻ ăn theo bữa. Số chốt ban đầu, số điều chỉnh đề nghị và số bếp xác nhận. Số cần giao, số bếp giao, số lớp thực nhận, chênh lệch và lý do. Bảng kê vận hành kỳ cho NV09. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 03](../../survey/forms/mau-03-bao-suat-va-giao-nhan.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Chốt theo mốc | Mầm 1 ngày 14/10/2026 | Sáng chốt 21, trưa chốt 22, xế chốt 22. HS2026-058 đến 07:30 có trong bữa sáng, HS2026-001 đến 07:45 thì không. |
| Bổ sung sau giờ chốt | HS2026-042 đến 08:50. Giáo viên Nguyễn Thị Lan đề nghị +1 trưa, +1 xế. Tổ trưởng bếp Hoàng Thị Bích xác nhận lúc 09:00 | Số chốt ban đầu giữ 22. Số cần giao trưa và xế là 23. Bếp giao 23, lớp nhận 23, chênh lệch 0. |
| Tổng toàn trường | Năm lớp ngày 14/10/2026, Mầm 2 giảm 1 suất xế do một trẻ về sớm | Trưa sau điều chỉnh 128, xế sau điều chỉnh 127. |
| Về trước bữa do dị ứng | Ngày 15/10, thực đơn trưa có món liên quan lưu ý của HS2026-042, phụ huynh nhận trẻ về trước bữa | Trẻ ghi không ăn trưa tại trường. Chuyên cần trong ngày vẫn `Có mặt`. |
| Giảm được bếp xác nhận | Chốt 23, giảm 1 được xác nhận, bếp giao 22, lớp nhận 22 | Số cần giao 22. Ghi đã giao đủ. |
| Thiếu suất | Chốt 23, không có điều chỉnh, bếp giao 22, lớp nhận 22 | Số cần giao 23. Ghi thiếu 1 suất, lưu nguyên nhân và xác nhận của hai bên. |
| Giảm vượt số cần giao | Lớp chốt 22 suất, giáo viên đề nghị giảm 23 | Hệ thống chặn xác nhận, số cần giao không âm. |
| Giảm sau một lần giảm | Lớp chốt 22 suất, bếp đã xác nhận giảm 20, giáo viên đề nghị giảm tiếp 5 | Số cần giao hiện tại là 2. Hệ thống chặn xác nhận giảm 5 dù 5 nhỏ hơn số chốt ban đầu. |

## Liên phân hệ và truy vết

- **NV01 → NV07:** Lưu ý dị ứng và chế độ ăn còn hiệu lực, chỉ đọc, kèm cảnh báo sớm với lưu ý `Chờ xác nhận` từ [UC-16](uc-16-cap-nhat-ho-so-va-luu-y-cham-soc.md) (QT-HS-06).
- **NV03 → NV07:** Danh sách có mặt theo giờ chốt bữa sáng cho bữa sáng và bản bàn giao buổi sáng cho bữa trưa, bữa xế.
- **NV05 → NV07:** Giáo viên phụ trách lớp theo ngày, gồm dạy thay, từ [UC-05](uc-05-lap-phan-cong-giao-vien.md).
- **NV08 → NV07:** Giờ trả thực tế từ [UC-09](uc-09-tra-tre-theo-nhom.md) để đối chiếu với lịch về sớm.
- **NV07 → NV09:** Số liệu vận hành theo kỳ để bếp lập báo cáo suất ăn (BC-04) tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md). Nếu điều chỉnh hoặc đối chiếu làm đổi số liệu của báo cáo đã nộp, hệ thống giữ ảnh chụp và cảnh báo người lập. Báo cáo `Đã gửi` do Ban giám hiệu xử lý yêu cầu bổ sung. Báo cáo `Đã duyệt` chuyển sang `Cần cập nhật` (QT-BC-04).
- **Tách với NV06:** Số suất và điều chỉnh của NV07 chỉ phục vụ vận hành bếp. Tiền ăn với phụ huynh ở NV06 chỉ căn cứ số ngày `Có mặt` đã khóa của NV03 (QT-HP-02, QT-SA-05).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-01, YC-SA-02, YC-SA-03 | QT-SA-01, QT-SA-02, QT-SA-03, QT-HS-06 | NV01, NV03 → NV07 · Rà soát danh sách dự kiến trước giờ chốt, chốt khi đủ dữ liệu, đối chiếu dị ứng và gửi bếp. |
| YC-SA-04, YC-SA-05 | QT-SA-04, QT-SA-05, QT-SA-06, QT-HP-02, QT-BC-04 | NV08 → NV07 → NV09 · Điều chỉnh sau chốt, thay đổi thực đơn hoặc dị ứng sau chốt, giao nhận suất, tách căn cứ tiền ăn NV06 và cập nhật báo cáo. |
