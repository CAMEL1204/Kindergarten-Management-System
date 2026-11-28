# UC-13 · Chốt chuyên cần theo lớp và ngày

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV03 · F1.3.1, F1.3.2, F1.3.4 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Không có |
| Mục tiêu | Ghi nhận và khóa một kết quả chuyên cần cho từng trẻ thuộc diện điểm danh trong ngày, bàn giao đúng số liệu theo mốc cấu hình cho bếp và tổng hợp/khóa số liệu kỳ chuyển sang học phí và báo cáo. |
| Điều kiện trước | Ngày học thuộc lịch học thực tế của trường. Có danh sách lớp/trẻ thuộc diện điểm danh từ NV02 và phân công hợp lệ từ NV05. Các mốc giờ vận hành có hiệu lực (mặc định: chốt bữa sáng `07:30`, bàn giao sáng `08:30`, khóa ngày `09:00`) đã được thiết lập tại [UC-03](uc-03-cau-hinh-moc-gio-va-bua-an.md). |
| Điều kiện sau | Mỗi trẻ có một kết quả duy nhất trong ngày học (`Có mặt`, `Vắng có phép`, `Vắng không phép` hoặc `Chờ xác minh`). Bản bàn giao sáng được lưu tách biệt với kết quả khóa ngày. Số liệu kỳ đã khóa sẵn sàng cho NV06 và NV09. |

## Luồng sự kiện chính

1. Giáo viên mở bảng chuyên cần theo lớp và ngày. Hệ thống kiểm tra ngày có lịch học, quyền giáo viên phụ trách (gồm giáo viên dạy thay, loại trừ giáo viên có đơn nghỉ ngắn ngày `QT-PC-06` hoặc tạm nghỉ `QT-NS-03`), lấy danh sách thuộc diện điểm danh từ NV02 và đọc lại dòng đã tồn tại để tránh tạo trùng.
2. Hệ thống tiếp nhận lượt đón hợp lệ từ NV08 (`UC-08`) làm nguồn duy nhất tạo kết quả `Có mặt` trước **mốc khóa có hiệu lực của ngày học** (mặc định `09:00`, tính theo **thời điểm ghi nhận**). Gắn cờ `Đến muộn` nếu **giờ đến thực tế** từ **mốc bàn giao sáng có hiệu lực** trở đi (mặc định `08:30`, theo `QT-DT-06`). Lượt trả (`UC-09`) chỉ cập nhật giờ về, không làm mất `Có mặt`.
3. Với trẻ chưa có lượt đón, giáo viên đối chiếu thông báo nghỉ từ NV08 và xác minh theo nội quy để kết luận `Vắng có phép` hoặc `Vắng không phép`. Nếu đã nhận báo nghỉ hoặc chưa liên hệ được mà chưa đủ căn cứ kết luận, giữ trạng thái `Chờ xác minh` (`QT-DD-02`).
4. Theo mốc cấu hình có hiệu lực tại `UC-03`, hệ thống cung cấp danh sách trẻ `Có mặt` tính đến hết mốc chốt bữa sáng (mặc định `07:30`) cho bữa sáng (`NV07`), và tại mốc bàn giao sáng (mặc định `08:30`) lưu bản chụp bàn giao buổi sáng cho `NV07` dùng cho bữa trưa và bữa xế; bản bàn giao này giữ nguyên không đổi khi phát sinh thay đổi sau mốc bàn giao sáng. Giáo viên tiếp tục xác minh các trường hợp còn treo đến trước giờ khóa.
5. Tại **mốc khóa có hiệu lực** (mặc định `09:00`), hệ thống khóa kết quả chuyên cần của ngày học, kể cả các dòng đang ở `Chờ xác minh` (và không cho giáo viên sửa trực tiếp sau mốc này).
6. Khi tổng hợp theo kỳ (tuần/tháng), hệ thống tổng hợp theo mã trẻ và ngày học để tránh đếm trùng khi trẻ chuyển lớp (`QT-DD-01`), tính tỷ lệ chuyên cần (`lượt có mặt / lượt phải điểm danh × 100%`; mẫu số bằng 0 hiển thị `không áp dụng`), báo riêng số lượt `Chờ xác minh`, xác định kỳ `Đã khóa` sau mốc khóa có hiệu lực của ngày học cuối kỳ (`QT-DD-07`), nhắc người duyệt xử lý các đề nghị điều chỉnh còn chờ trước ngày học đầu kỳ kế tiếp và cho phép lọc/xuất số liệu kỳ cho NV06 và NV09.

## Luồng thay thế và ngoại lệ

- **Bước 1 (ngoài quyền hoặc ngày nghỉ):** Nếu là ngày nghỉ theo lịch trường, lớp không hoạt động, hoặc giáo viên không thuộc phân công ngày đó / đang nghỉ theo đơn nghỉ ngắn ngày (`QT-PC-06`) hay tạm nghỉ (`QT-NS-03`), hệ thống chặn thao tác.
- **Bước 1 & Bước 3 (trẻ nghỉ dài ngày đã xác nhận tại NV02):** Với ngày học nằm trong khoảng nghỉ dài ngày (`QT-DD-06`), hệ thống tự khởi tạo `Vắng có phép` ngay từ đầu ngày (không qua `Chờ xác minh`), không cho giáo viên sửa tay sang `Vắng không phép`; nếu trẻ thực tế đến trường, lượt đón NV08 trước mốc khóa có hiệu lực vẫn cập nhật `Có mặt` và giữ lịch sử.
- **Bước 2 (đón trẻ sau vắng hoặc hủy lượt đón ghi nhầm trước khóa):** Nếu trẻ đã ghi `Vắng có phép` nhưng được ghi nhận đón trước mốc khóa có hiệu lực, lượt đón cập nhật sang `Có mặt` và giữ lịch sử báo nghỉ (`QT-DD-05`). Nếu lượt đón bị hủy do ghi nhầm trước mốc khóa (`QT-DT-07`), hệ thống tính lại kết quả từ báo nghỉ/xác minh còn lại.
- **Bước 2 & Bước 5 (phát sinh hoặc hủy lượt đón từ đúng mốc khóa có hiệu lực):** Nếu lượt đón được ghi nhận mới (kể cả nhập bổ sung cho giờ đến trước đó) hoặc bị hủy từ mốc khóa có hiệu lực của ngày học trở đi (mặc định `09:00`), kết quả đã khóa giữ nguyên và hệ thống tự chuyển sang luồng đề nghị điều chỉnh tại [UC-14](uc-14-dieu-chinh-chuyen-can-sau-khoa.md).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã lớp, ngày học, lịch học trường, khoảng xếp lớp và nghỉ dài ngày (NV02), phân công và đơn nghỉ giáo viên (NV05/NV04), báo nghỉ, mã lượt đón/trả và thời điểm ghi nhận (NV08), giờ đến/về thực tế, căn cứ xác minh. | Bảng chuyên cần theo trẻ/ngày (`Có mặt`, `Vắng có phép`, `Vắng không phép`, `Chờ xác minh`), cờ `Đến muộn`, trạng thái khóa ngày/kỳ, danh sách ăn sáng theo mốc chốt bữa sáng có hiệu lực (mặc định `07:30`), bản bàn giao buổi sáng (mặc định `08:30`), tỷ lệ chuyên cần theo kỳ, số ngày có mặt chuyển NV06 (tính 0 cho `Chờ xác minh` không có lượt đón) và số liệu lớp cho NV09. |

## Liên phân hệ và truy vết

- **NV02, NV05 → NV03:** NV02 xác định trẻ thuộc diện điểm danh và nghỉ dài ngày (`QT-DD-06`); NV05 xác định quyền thao tác của giáo viên chủ nhiệm/phụ/dạy thay trên lớp trong ngày (`QT-DT-05`, `QT-PC-06`).
- **NV08 → NV03:** NV08 cung cấp nguồn báo nghỉ và lượt đón/trả thực tế (`QT-DT-01`, `QT-DT-06`, `QT-DT-07`).
- **NV03 → NV07:** Cung cấp danh sách có mặt đến hết mốc chốt bữa sáng có hiệu lực (mặc định `07:30`) và bản bàn giao buổi sáng (mặc định `08:30`) cho bữa trưa, bữa xế.
- **NV03 → NV06:** Sau khi khóa kỳ, cung cấp chỉ đọc số ngày `Có mặt` đã xác nhận theo từng trẻ. Mọi lượt `Chờ xác minh` không có lượt đón luôn tính `0` ngày có mặt và **không làm treo** đối soát tiền ăn; chỉ trẻ còn ngày chưa khóa hoặc còn đề nghị điều chỉnh chờ duyệt tại `UC-14` có thể làm thay đổi kết quả `Có mặt` mới bị treo đối soát, và chỉ được gỡ treo khi mọi ngày liên quan đã khóa và không còn đề nghị như vậy (`QT-DD-07`, `QT-HP-05`).
- **NV03 → NV09:** Cung cấp số liệu chuyên cần đã khóa theo lớp và kỳ (kèm danh sách lượt chờ xác minh/chưa hoàn tất) để giáo viên lập báo cáo chuyên cần lớp tại `UC-10`.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DD-01, YC-DD-05 | QT-DD-01, QT-DD-06, QT-PC-06, QT-NS-03 | NV02, NV05 → NV03 · Kiểm tra lớp, quyền giáo viên/dạy thay (trừ nghỉ ngắn ngày/tạm nghỉ), tránh đếm trùng và tự ghi nghỉ dài ngày. |
| YC-DD-02 | QT-DD-02, QT-DD-05, QT-DT-01, QT-DT-05, QT-DT-06, QT-DT-07 | NV08 → NV03 → NV07 · Tiếp nhận giao nhận, xác minh vắng, gắn cờ đến muộn, giữ `Chờ xác minh`, chốt theo các mốc có hiệu lực từ UC-03. |
| YC-DD-04, YC-DD-05 | QT-DD-04, QT-DD-07, QT-HP-05 | NV03 → NV06, NV09 · Tổng hợp kỳ, khóa kỳ, nhắc xử lý đề nghị chờ và bàn giao đối soát học phí/báo cáo. |
