# UC-05 · Lập phân công giáo viên

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV05 · F1.5.1, F1.5.2, F1.5.3, F1.5.4 |
| Tác nhân chính | Người phân công (Văn phòng) |
| Tác nhân phụ | Ban giám hiệu (duyệt phương án, đơn nghỉ ngắn ngày và dạy thay). Giáo viên (gửi đơn nghỉ ngắn ngày, xem lịch của mình). |
| Mục tiêu | Mỗi lớp đang hoạt động có đủ giáo viên theo định mức trong từng ngày. Không giáo viên nào bị phân công chồng ca. Hệ thống có danh sách giáo viên phụ trách lớp theo ngày để kiểm tra quyền ở NV03 và NV08. |
| Điều kiện trước | Người phân công đăng nhập hệ thống con Văn phòng và có quyền NV05. Năm học, ca làm việc và định mức giáo viên theo nhóm tuổi đã có tại `UC-02`. NV02 có danh sách lớp đang hoạt động tại ngày hiệu lực. NV04 có danh sách nhân sự đủ điều kiện đứng lớp tại ngày hiệu lực (QT-NS-02). |
| Điều kiện sau | Phương án đã duyệt có các dòng phân công theo lớp, giáo viên, vai trò, ca, ngày bắt đầu và ngày kết thúc. Phân công cũ bị thay thế được kết thúc đúng ngày, không bị ghi đè. Danh sách giáo viên phụ trách lớp theo ngày được cập nhật cho NV03 và NV08. |

## Luồng sự kiện chính

1. Người phân công chọn năm học và ngày hiệu lực, rồi tạo phương án mới. Hệ thống lấy lớp đang hoạt động và sĩ số tại ngày hiệu lực từ NV02, nhân sự đủ điều kiện đứng lớp từ NV04, ca và định mức từ `UC-02`.
2. Với từng lớp, người phân công chọn giáo viên chủ nhiệm, giáo viên phụ và ca. Hệ thống chỉ cho chọn nhân sự có trong danh sách đủ điều kiện tại ngày hiệu lực, vào lớp đang hoạt động (QT-PC-01).
3. Khi lưu, hệ thống kiểm tra từng dòng: một giáo viên không ở hai lớp trong cùng ca và cùng ngày, mỗi lớp có tối đa một giáo viên chủ nhiệm có hiệu lực trong ngày (QT-PC-02). Hệ thống so số giáo viên mỗi lớp với định mức và đưa lớp chưa đủ vào danh sách cảnh báo (QT-PC-03).
4. Người phân công xử lý cảnh báo hoặc ghi lý do, rồi trình duyệt. Phương án chuyển sang `Chờ duyệt` và không sửa được khi đang chờ.
5. Ban giám hiệu mở phương án, xem bảng phân công và danh sách cảnh báo. Hệ thống chặn nếu người duyệt là người lập. Ban giám hiệu duyệt và chốt ngày hiệu lực.
6. Hệ thống ghi toàn bộ dòng phân công trong một giao dịch (NFR-05). Nếu phương án thay cho phân công đang có hiệu lực, hệ thống đặt ngày kết thúc của dòng cũ bằng ngày hiệu lực của dòng mới trong cùng giao dịch (QT-PC-04).
7. Hệ thống thông báo cho giáo viên và văn phòng. Danh sách giáo viên phụ trách lớp theo ngày được cập nhật cho NV03 và NV08.

## Luồng con: đơn nghỉ ngắn ngày và dạy thay (F1.5.3)

1. Giáo viên gửi đơn nghỉ cho ngày hoặc ca, ghi lý do. Hệ thống chặn đơn dài hơn 5 ngày làm việc liên tục và hướng sang tạm nghỉ ở NV04 (QT-PC-06).
2. Ban giám hiệu duyệt đơn. Đơn đã duyệt không khóa tài khoản. Trong những ngày nghỉ, giáo viên không có quyền điểm danh và giao nhận lớp được phân công.
3. Hệ thống đưa lớp của giáo viên vắng vào danh sách cần xếp dạy thay. Nguồn gồm đơn nghỉ ngắn ngày đã duyệt và tạm nghỉ đã xác nhận ở NV04 (chỉ đọc).
4. Người phân công mở lớp cần xếp. Hệ thống gợi ý giáo viên còn trống trong ca đó. Người phân công chọn người dạy thay và khoảng ngày. Hệ thống kiểm tra lại QT-PC-02 và QT-PC-03.
5. Ban giám hiệu duyệt phiếu dạy thay. Hệ thống ghi dòng phân công vai trò `Dạy thay` có ngày bắt đầu và ngày kết thúc, thông báo cho giáo viên dạy thay.

## Luồng thay thế và ngoại lệ

- **Bước 2 (nhân sự không đủ điều kiện):** Nhân sự đã nghỉ việc, đang tạm nghỉ hoặc không được giao đứng lớp không xuất hiện trong danh sách chọn. Lớp đã đóng tại ngày hiệu lực không xuất hiện (QT-PC-01).
- **Bước 3 (trùng ca):** Hệ thống chặn lưu dòng trùng ca, báo tên lớp và ca đang trùng.
- **Bước 3 (thiếu định mức):** Hệ thống không chặn lưu. Lớp được đưa vào danh sách cảnh báo để Ban giám hiệu quyết định.
- **Bước 5 (trả lại):** Ban giám hiệu trả lại kèm ý kiến. Phương án về `Nháp` để người phân công sửa và trình lại.
- **Thay đổi giữa năm:** Người phân công tạo phương án điều chỉnh cho lớp và ngày hiệu lực mới. Ngày trước ngày hiệu lực, giáo viên cũ vẫn có quyền trên lớp. Từ ngày hiệu lực, giáo viên mới có quyền (QT-PC-04).
- **Luồng con bước 4 (không có giáo viên trống ca):** Người phân công trình hiệu trưởng chọn nhân sự khác trong danh sách đủ điều kiện của NV04. Nếu vẫn không có người, lớp được ghi vào danh sách cảnh báo thiếu giáo viên kèm khoảng ngày (QT-PC-05).
- **Biến động từ NV04:** Khi giáo viên nghỉ việc hoặc tạm nghỉ có hiệu lực, hệ thống đánh dấu các dòng phân công bị ảnh hưởng và đưa lớp vào danh sách cần xếp.

## Tra cứu và tổng hợp (F1.5.4)

- Người phân công và Ban giám hiệu tra lịch sử phân công theo lớp, giáo viên và ngày. Giáo viên chỉ xem lịch của mình.
- Cuối tháng, hệ thống tổng hợp số ngày hoặc ca đứng lớp theo phân công, số ngày dạy thay và số ngày tạm nghỉ của từng giáo viên. Người phân công xuất bảng và dùng làm số liệu báo cáo phân công tại `UC-10`.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Năm học, ngày hiệu lực. Lớp đang hoạt động và sĩ số (NV02). Nhân sự đủ điều kiện đứng lớp, tạm nghỉ và nghỉ việc đã xác nhận (NV04). Ca, khung giờ, định mức (`UC-02`). Đơn nghỉ ngắn ngày. Lớp, giáo viên, vai trò, ca, khoảng ngày dạy thay. | Phương án phân công có trạng thái, người lập, người duyệt. Dòng phân công theo lớp, giáo viên, vai trò, ca, ngày bắt đầu, ngày kết thúc. Danh sách cảnh báo lớp thiếu giáo viên. Đơn nghỉ đã duyệt. Phiếu dạy thay. Danh sách giáo viên phụ trách lớp theo ngày. Bảng tổng hợp ca theo tháng. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Lập phương án đầu năm | PCA-2026-01, hiệu lực 01/09/2026, 5 lớp, mỗi lớp 1 chủ nhiệm và 1 giáo viên phụ ([Mẫu 07](../../survey/forms/mau-07-phan-cong-va-day-thay.md)) | Cả 5 lớp đạt định mức. Phương án được Lê Minh Tâm duyệt, người lập là Phạm Thu Hà. |
| Trùng ca | Giao Nguyễn Thị Lan (NS-003) làm giáo viên phụ Mầm 2 cùng ca cả ngày khi đang chủ nhiệm Mầm 1 | Chặn lưu theo QT-PC-02. |
| Dạy thay khi tạm nghỉ | Trần Thu Trang (NS-006) tạm nghỉ 19/10–30/10/2026. Không có giáo viên trống ca. | Hiệu trưởng chọn Trịnh Thị Nga (NS-011). Phiếu PDT-2026-10-01 dạy thay từ 19/10 đến hết 30/10/2026, 10 ngày học. Mầm 2 vẫn đạt định mức. |
| Đơn nghỉ quá hạn mức | Giáo viên gửi đơn nghỉ 6 ngày làm việc | Chặn, hướng sang tạm nghỉ ở NV04 (QT-PC-06). |
| Tổng hợp tháng 10/2026 | 22 ngày học | Trần Thu Trang 12 ngày đứng lớp và 10 ngày tạm nghỉ. Trịnh Thị Nga 10 ngày dạy thay. |

## Liên phân hệ và truy vết

- **NV02 → NV05:** Lớp đang hoạt động và sĩ số tại ngày hiệu lực từ `UC-12`, `UC-17` (QT-PC-01). NV02 kiểm tra cảnh báo định mức của lớp đích khi xếp lớp, quay lại hoặc chuyển lớp (QT-PC-03).
- **NV04 → NV05:** Danh sách đủ điều kiện đứng lớp và tạm nghỉ đã xác nhận từ [UC-04](uc-04-lap-va-cap-nhat-ho-so-nhan-su.md), chỉ đọc (QT-NS-02, QT-NS-03).
- **F1.0 → NV05:** Ca, khung giờ và định mức giáo viên từ `UC-02`.
- **NV05 → NV03, NV08:** Danh sách giáo viên phụ trách lớp theo ngày, gồm dạy thay, không gồm giáo viên có đơn nghỉ đã duyệt hoặc đang tạm nghỉ. Dùng để kiểm tra quyền tại `UC-13`, [UC-08](uc-08-don-tre-theo-nhom.md), [UC-09](uc-09-tra-tre-theo-nhom.md) (QT-PC-06, QT-DT-05).
- **NV05 → NV09:** Bảng tổng hợp ca cho báo cáo phân công của Văn phòng tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-PC-01 | QT-PC-01, QT-NS-02 | NV02, NV04 → NV05 · Lập phương án từ lớp đang hoạt động và nhân sự đủ điều kiện. |
| YC-PC-02 | QT-PC-02, QT-PC-03, QT-PC-04 | NV05 · Kiểm tra trùng ca, định mức, duyệt và thay phân công nguyên tử. |
| YC-PC-03 | QT-PC-05, QT-PC-06, QT-NS-03, QT-DT-05 | NV04 → NV05 → NV03, NV08 · Đơn nghỉ ngắn ngày, dạy thay và quyền theo lớp, ngày. |
| YC-PC-04 | QT-PC-04 | NV05 → NV09 · Lịch sử phân công và tổng hợp ca theo tháng. |
