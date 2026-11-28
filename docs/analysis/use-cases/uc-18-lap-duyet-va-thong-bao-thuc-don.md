# UC-18 · Lập, duyệt và thông báo thực đơn tuần

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV07 · F1.7.1, F1.7.2 |
| Tác nhân chính | Bếp (Bếp trưởng) |
| Tác nhân phụ | Ban giám hiệu (người duyệt thực đơn), Giáo viên |
| Mục tiêu | Lập và duyệt thực đơn theo ngày và từng bữa, gửi thông báo thực đơn tuần để ghi nhận phản hồi dị ứng và lịch về sớm của trẻ trước khi chốt suất ăn. |
| Điều kiện trước | Danh mục bữa ăn của trường (sáng, trưa, xế) đã được thiết lập. Giáo viên được phân công lớp (`NV05`). Hồ sơ trẻ có lưu ý dị ứng/chế độ ăn từ `NV01` (`UC-16`). |
| Điều kiện sau | Thực đơn có trạng thái `Đã duyệt` làm căn cứ cho [UC-15](uc-15-chot-va-doi-chieu-suat-an.md). Các phản hồi phụ huynh và lịch về sớm theo trẻ, ngày, bữa được lưu sẵn sàng để giáo viên rà soát khi chốt suất. |

## Luồng sự kiện chính

1. Trước kỳ áp dụng, Bếp trưởng lập thực đơn theo ngày và từng bữa (sáng, trưa, xế) rồi gửi duyệt.
2. Ban giám hiệu (hoặc người được ủy quyền duyệt thực đơn) kiểm tra thực đơn và xác nhận `Đã duyệt`; hệ thống mở chế độ xem thực đơn đã duyệt cho các lớp và phục vụ xuất/gửi thông báo đầu tuần cho phụ huynh.
3. Giáo viên xem thực đơn tuần của lớp mình kèm lưu ý dị ứng/chế độ ăn từ `NV01` (bao gồm cảnh báo `lưu ý chờ xác nhận` theo `QT-HS-06`) và gửi thực đơn tuần cho phụ huynh qua kênh liên lạc của trường.
4. Khi phụ huynh phản hồi về món liên quan đến dị ứng của trẻ, giáo viên ghi nhận mã trẻ, ngày, bữa bị ảnh hưởng và lịch phụ huynh dự kiến nhận trẻ về sớm trước bữa đó vào hệ thống (`QT-SA-03`).

## Luồng thay thế và ngoại lệ

- **Bước 2 (từ chối hoặc yêu cầu chỉnh sửa thực đơn):** Nếu thực đơn chưa đạt, người duyệt trả lại kèm ghi chú để Bếp trưởng chỉnh sửa và trình duyệt lại; thực đơn chưa duyệt không được dùng làm căn cứ chốt suất ở `UC-15`.
- **Bước 3–4 (lưu ý dị ứng mới phát sinh từ phụ huynh):** Nếu phụ huynh báo thông tin dị ứng mới chưa có trong hồ sơ, giáo viên đồng thời gửi đề nghị cập nhật sang `NV01` ([UC-16](uc-16-cap-nhat-ho-so-va-luu-y-cham-soc.md)) để văn phòng ghi và phó hiệu trưởng chuyên môn xác nhận; lưu ý vẫn hiện cảnh báo sớm trong lúc chờ xác nhận (`QT-HS-06`).
- **Bước 4 (thực đơn hoặc lưu ý dị ứng thay đổi sau thông báo đầu tuần — `QT-SA-06`):** Khi Bếp cập nhật thực đơn đã duyệt (trình duyệt lại) hoặc có lưu ý dị ứng mới ảnh hưởng đến món trong tuần, hệ thống cảnh báo giáo viên của trẻ liên quan để liên hệ lại phụ huynh, cập nhật lịch về sớm trước bữa bị ảnh hưởng. Nếu bữa đó đã chốt tại `UC-15`, giáo viên thực hiện gửi điều chỉnh cho bếp và giữ nguyên số chốt ban đầu.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Tuần/ngày áp dụng, bữa (`Sáng`, `Trưa`, `Xế`), danh sách món ăn, quyết định duyệt thực đơn, lưu ý dị ứng từ NV01, phản hồi phụ huynh (mã trẻ, ngày, bữa, giờ dự kiến nhận trẻ về sớm). | Thực đơn theo ngày/bữa (`Nháp` / `Chờ duyệt` / `Đã duyệt`), bản xuất thông báo thực đơn tuần, danh sách phản hồi và lịch về sớm theo trẻ/ngày/bữa chuyển sang `UC-15`. |

## Liên phân hệ và truy vết

- **NV01 → NV07:** Đọc chỉ đọc lưu ý dị ứng còn hiệu lực và cảnh báo `lưu ý chờ xác nhận` (`QT-HS-06`); giáo viên/bếp không sửa trực tiếp lưu ý chăm sóc tại NV07.
- **NV07 nội bộ (`UC-18 → UC-15`):** Thực đơn đã duyệt và lịch về sớm theo trẻ/ngày/bữa là đầu vào bắt buộc cho giao dịch chốt suất tại `UC-15`.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-01 | QT-SA-01 | Ghi nhận và duyệt thực đơn theo ngày và bữa làm căn cứ chốt suất. |
| YC-SA-02 | QT-SA-03, QT-HS-06 | NV01 → NV07 · Thông báo thực đơn tuần, đối chiếu dị ứng và lưu phản hồi/lịch về sớm. |
| YC-SA-04 | QT-SA-06 | Rà soát lại lịch về sớm và số suất liên quan khi thực đơn hoặc dị ứng thay đổi. |
