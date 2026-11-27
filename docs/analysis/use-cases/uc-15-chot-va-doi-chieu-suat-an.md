# UC-15 · Chốt và đối chiếu suất ăn theo bữa

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV07 · F1.7 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Bếp |
| Mục tiêu | Xác nhận đúng trẻ ăn từng bữa và đối chiếu số thực nhận với số cần giao. |
| Điều kiện trước | Có thực đơn đã duyệt, lớp được phân công, mốc chốt theo bữa và dữ liệu bàn giao NV03. |
| Điều kiện sau | Lưu được danh sách theo bữa, số chốt ban đầu, điều chỉnh được xác nhận, số giao, số nhận và lý do chênh lệch. |

## Luồng sự kiện chính

1. Giáo viên chọn lớp, ngày, bữa. Đối với bữa sáng, lấy trẻ có mặt đến hết 07:30. Với trưa và xế, dùng bản bàn giao 08:30 theo cấu hình mặc định.
2. Đối chiếu món ăn, lưu ý dị ứng NV01, phản hồi phụ huynh sau thông báo thực đơn tuần và lịch về sớm. Ghi riêng bữa trẻ ăn tại trường.
3. Giáo viên xác nhận từng trẻ và chốt tổng số trẻ ăn theo lớp, ngày, bữa. Hệ thống loại dòng trùng và gửi số chốt ban đầu cho bếp.
4. Bếp chuẩn bị theo số cần giao bằng số chốt cộng phần điều chỉnh đã xác nhận. Bếp ghi số giao, giáo viên ghi số lớp thực nhận.
5. Đối chiếu số thực nhận với số cần giao, ghi lý do, người xác nhận khi lệch và tổng hợp theo kỳ.

## Luồng thay thế và ngoại lệ

- Bước 2, khi thực đơn hoặc dị ứng thay đổi, liên hệ lại phụ huynh, xác nhận lịch đón trước bữa bị ảnh hưởng. Hiển thị cảnh báo với lưu ý chờ xác nhận.
- Bước 3–4, sau chốt, gửi yêu cầu tăng/giảm kèm lý do và thời điểm. Bếp xác nhận phần còn đáp ứng được, kể cả khi chuyên cần chờ duyệt. Giữ nguyên số chốt gốc.
- Bước 5, nếu chốt 23, giảm 1 được duyệt và nhận 22 thì đủ. Nếu chưa có giảm được duyệt và chỉ nhận 22 thì ghi thiếu 1 để đối chiếu.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp, ngày, bữa, thực đơn, mốc chốt, danh sách có mặt, dị ứng, lịch về sớm, giờ trả thực tế, phản hồi và số điều chỉnh. | Danh sách ăn theo bữa, số ban đầu, số đề nghị và được bếp xác nhận, số cần giao, số bếp giao, số lớp nhận, chênh lệch và lý do. |

## Liên phân hệ và truy vết

NV01 cung cấp lưu ý còn hiệu lực và cảnh báo chờ xác nhận. NV03 cung cấp danh sách dự kiến, NV08 cung cấp giờ về thực tế. NV09 nhận số liệu vận hành. Căn cứ tiền ăn NV06 là ngày có mặt đã xác nhận tại NV03.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-01, YC-SA-02, YC-SA-03 | QT-SA-01, QT-SA-02, QT-SA-03, QT-HS-06 | NV01, NV03 → NV07 · Chọn trẻ ăn theo mốc và lưu ý dị ứng. |
| YC-SA-04, YC-SA-05 | QT-SA-04, QT-SA-05, QT-SA-06 | NV08 → NV07 → NV09 · Giờ về, điều chỉnh sau chốt và giao nhận. |
