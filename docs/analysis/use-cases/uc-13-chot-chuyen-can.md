# UC-13 · Chốt chuyên cần theo lớp và ngày

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV03 · F1.3.1, F1.3.2, F1.3.4 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Không có |
| Mục tiêu | Mỗi trẻ thuộc diện điểm danh có một kết quả chuyên cần trong ngày và được khóa đúng mốc. Bếp nhận đúng số liệu theo mốc cấu hình. Số liệu kỳ được tổng hợp và khóa để chuyển sang học phí và báo cáo. |
| Điều kiện trước | Ngày thuộc lịch học thực tế của trường. NV02 có danh sách trẻ thuộc diện điểm danh của lớp. NV05 có phân công hợp lệ. Các mốc giờ có hiệu lực đã cấu hình tại [UC-03](uc-03-cau-hinh-moc-gio-va-bua-an.md): mặc định chốt bữa sáng 07:30, bàn giao buổi sáng 08:30, khóa ngày 09:00. |
| Điều kiện sau | Mỗi trẻ có một kết quả duy nhất trong ngày học: `Có mặt`, `Vắng có phép`, `Vắng không phép` hoặc `Chờ xác minh`. Bản bàn giao buổi sáng được lưu tách khỏi kết quả khóa ngày. Số liệu kỳ đã khóa sẵn sàng cho NV06 và NV09. |

## Luồng sự kiện chính

1. Giáo viên mở bảng chuyên cần theo lớp và ngày. Hệ thống kiểm tra ngày có lịch học và quyền giáo viên phụ trách, gồm giáo viên dạy thay, trừ giáo viên có đơn nghỉ ngắn ngày (QT-PC-06) hoặc đang tạm nghỉ (QT-NS-03). Hệ thống lấy danh sách thuộc diện điểm danh từ NV02 và đọc các dòng đã có để tránh tạo trùng (YC-DD-01).
2. Hệ thống nhận lượt đón hợp lệ từ NV08 ([UC-08](uc-08-don-tre-theo-nhom.md)) làm nguồn duy nhất tạo `Có mặt`. Lượt đón có thời điểm ghi nhận trước mốc khóa (mặc định 09:00) cập nhật trực tiếp. Trẻ có giờ đến thực tế từ mốc bàn giao (mặc định 08:30) được gắn cờ `Đến muộn` (QT-DT-06). Lượt trả ([UC-09](uc-09-tra-tre-theo-nhom.md)) chỉ cập nhật giờ về, không làm mất `Có mặt`.
3. Với trẻ chưa có lượt đón, giáo viên đối chiếu báo nghỉ từ NV08 và xác minh theo nội quy để kết luận `Vắng có phép` hoặc `Vắng không phép`. Đã nhận báo nghỉ hoặc chưa liên hệ được mà chưa đủ căn cứ thì giữ `Chờ xác minh` (QT-DD-02).
4. Theo các mốc có hiệu lực, hệ thống cung cấp cho NV07 danh sách trẻ `Có mặt` đến hết mốc chốt bữa sáng (mặc định 07:30). Tại mốc bàn giao (mặc định 08:30), hệ thống lưu bản bàn giao buổi sáng cho bữa trưa và bữa xế. Bản này không đổi khi có thay đổi sau mốc bàn giao. Giáo viên tiếp tục xác minh các trường hợp còn treo tới trước mốc khóa.
5. Tại mốc khóa (mặc định 09:00), hệ thống khóa kết quả của ngày học, kể cả các dòng `Chờ xác minh`. Giáo viên không sửa trực tiếp sau mốc này.
6. Khi tổng hợp theo kỳ (tuần hoặc tháng), hệ thống:
   - tổng hợp theo mã trẻ và ngày học để không đếm trùng khi trẻ chuyển lớp (QT-DD-01);
   - tính tỷ lệ chuyên cần bằng lượt có mặt chia lượt phải điểm danh nhân 100%, mẫu số bằng 0 thì hiển thị không áp dụng (QT-DD-04);
   - báo riêng số lượt `Chờ xác minh`;
   - xác định kỳ `Đã khóa` khi đã qua mốc khóa của ngày học cuối kỳ (QT-DD-07);
   - nhắc người duyệt xử lý các đề nghị điều chỉnh còn chờ trước ngày học đầu kỳ kế tiếp (YC-DD-05);
   - cho lọc và xuất số liệu kỳ cho NV06 và NV09 (YC-DD-04).

## Luồng thay thế và ngoại lệ

- **Bước 1 (ngoài quyền hoặc ngày nghỉ):** Ngày nghỉ theo lịch trường, lớp không hoạt động, giáo viên không thuộc phân công ngày đó, có đơn nghỉ ngắn ngày (QT-PC-06) hoặc đang tạm nghỉ (QT-NS-03). Hệ thống chặn thao tác.
- **Bước 1 và 3 (nghỉ dài ngày đã xác nhận tại NV02):** Với ngày học trong khoảng nghỉ dài ngày (QT-DD-06), hệ thống ghi `Vắng có phép` từ đầu ngày, không qua `Chờ xác minh`. Giáo viên không sửa tay sang `Vắng không phép`. Nếu trẻ thực tế đến trường, lượt đón trước mốc khóa vẫn cập nhật `Có mặt` và giữ lịch sử.
- **Bước 2 (đón trẻ sau khi đã ghi vắng):** Trẻ đã ghi `Vắng có phép` nhưng có lượt đón trước mốc khóa. Lượt đón cập nhật sang `Có mặt` và giữ lịch sử báo nghỉ (QT-DD-05).
- **Bước 2 (hủy lượt đón ghi nhầm trước mốc khóa):** Hệ thống tính lại kết quả từ báo nghỉ và kết luận xác minh còn lại (QT-DT-07).
- **Bước 2 và 5 (phát sinh hoặc hủy lượt đón từ mốc khóa):** Lượt đón mới, kể cả nhập bổ sung cho giờ đến trước đó, hoặc lượt hủy có thời điểm ghi nhận từ mốc khóa trở đi. Kết quả đã khóa giữ nguyên. Hệ thống chuyển sang đề nghị điều chỉnh tại [UC-14](uc-14-dieu-chinh-chuyen-can-sau-khoa.md).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã lớp, ngày học, lịch học của trường. Khoảng xếp lớp và nghỉ dài ngày (NV02). Phân công và đơn nghỉ của giáo viên (NV05), tạm nghỉ (NV04). Báo nghỉ, mã lượt đón, lượt trả và thời điểm ghi nhận (NV08). Giờ đến, giờ về thực tế. Căn cứ xác minh. | Bảng chuyên cần theo trẻ và ngày, cờ `Đến muộn`, trạng thái khóa ngày và khóa kỳ. Danh sách ăn sáng theo mốc chốt bữa sáng. Bản bàn giao buổi sáng. Tỷ lệ chuyên cần theo kỳ. Số ngày `Có mặt` chuyển NV06, `Chờ xác minh` không có lượt đón tính 0. Số liệu lớp cho NV09. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 02](../../survey/forms/mau-02-diem-danh-giao-nhan.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Bàn giao 08:30 | Mầm 1 ngày 14/10/2026, 25 trẻ phải điểm danh | Bản bàn giao: 22 `Có mặt`, 2 `Vắng có phép`, 1 `Chờ xác minh`. |
| Báo nghỉ có xác minh | HS2026-015 báo nghỉ qua Zalo lúc 07:10, giáo viên xác nhận có phép | `Vắng có phép`. Báo nghỉ được giữ làm căn cứ. |
| Đến muộn trước mốc khóa | HS2026-042 báo nghỉ 07:20, đã xác nhận có phép, bà ngoại đưa đến 08:50 | `Có mặt`, cờ `Đến muộn`. Lịch sử báo nghỉ giữ nguyên. Bản bàn giao 08:30 không đổi. |
| Khóa 09:00 | Cùng lớp, cùng ngày | 23 `Có mặt`, 1 `Vắng có phép`, 1 `Chờ xác minh` (HS2026-061). Sau 09:00 giáo viên không sửa trực tiếp được. |
| Danh sách ăn sáng | HS2026-058 đến 07:30, HS2026-001 đến 07:45 ([Mẫu 03](../../survey/forms/mau-03-bao-suat-va-giao-nhan.md)) | Mầm 1 có 21 trẻ trong danh sách ăn sáng. HS2026-058 có, HS2026-001 không. |
| Nghỉ dài ngày | Trẻ nghỉ dài ngày từ 05/10 đến 16/10, ngày 12/10 được đưa đến lúc 08:10 | Các ngày học khác trong khoảng tự ghi `Vắng có phép`. Ngày 12/10 ghi `Có mặt` (QT-DD-06). |
| Giáo viên tạm nghỉ | Trần Thu Trang mở bảng Mầm 2 ngày 20/10/2026 ([Mẫu 07](../../survey/forms/mau-07-phan-cong-va-day-thay.md)) | Chặn. Trịnh Thị Nga dạy thay được điểm danh Mầm 2. |
| Tổng hợp tháng 09/2026 | Mầm 1: 24 trẻ học cả 20 ngày, HS2026-042 thuộc lớp 18 ngày ([Mẫu 05](../../survey/forms/mau-05-bao-cao-thang.md)) | 498 lượt phải điểm danh, 470 lượt có mặt, tỷ lệ 94,4%. HS2026-042 có 16 ngày `Có mặt` chuyển NV06. |

## Liên phân hệ và truy vết

- **NV02, NV05 → NV03:** NV02 xác định trẻ thuộc diện điểm danh và nghỉ dài ngày (QT-DD-06). NV05 xác định quyền của giáo viên chủ nhiệm, giáo viên phụ và giáo viên dạy thay trên lớp trong ngày tại [UC-05](uc-05-lap-phan-cong-giao-vien.md) (QT-DT-05, QT-PC-06).
- **NV08 → NV03:** Báo nghỉ và lượt đón, lượt trả thực tế (QT-DT-01, QT-DT-06, QT-DT-07).
- **NV03 → NV07:** Danh sách có mặt đến hết mốc chốt bữa sáng cho bữa sáng và bản bàn giao buổi sáng cho bữa trưa, bữa xế tại [UC-15](uc-15-chot-va-doi-chieu-suat-an.md).
- **NV03 → NV06:** Sau khi khóa kỳ, số ngày `Có mặt` đã xác nhận theo từng trẻ, chỉ đọc, cho [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md). Lượt `Chờ xác minh` không có lượt đón tính 0 ngày và không làm treo đối soát. Đối soát của trẻ bị treo khi trẻ còn ngày chưa khóa hoặc còn đề nghị chờ duyệt tại `UC-14` có thể đổi `Có mặt`. Chỉ gỡ treo khi mọi ngày liên quan đã khóa và không còn đề nghị như vậy (QT-DD-07, QT-HP-05).
- **NV03 → NV09:** Số liệu chuyên cần đã khóa theo lớp và kỳ, kèm danh sách lượt chờ xác minh, để giáo viên lập báo cáo chuyên cần lớp tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DD-01, YC-DD-05 | QT-DD-01, QT-DD-06, QT-PC-06, QT-NS-03 | NV02, NV05 → NV03 · Kiểm tra lớp, quyền giáo viên và dạy thay (trừ nghỉ ngắn ngày, tạm nghỉ), tránh đếm trùng, tự ghi nghỉ dài ngày. |
| YC-DD-02 | QT-DD-02, QT-DD-05, QT-DT-01, QT-DT-05, QT-DT-06, QT-DT-07 | NV08 → NV03 → NV07 · Nhận lượt giao nhận, xác minh vắng, gắn cờ đến muộn, giữ `Chờ xác minh`, chốt theo các mốc có hiệu lực từ `UC-03`. |
| YC-DD-04, YC-DD-05 | QT-DD-04, QT-DD-07, QT-HP-05 | NV03 → NV06, NV09 · Tổng hợp kỳ, khóa kỳ, nhắc xử lý đề nghị còn chờ và cung cấp số liệu cho học phí, báo cáo. |
