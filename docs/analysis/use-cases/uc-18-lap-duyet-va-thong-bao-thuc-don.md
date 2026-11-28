# UC-18 · Lập, duyệt và thông báo thực đơn tuần

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV07 · F1.7.1, F1.7.2 |
| Tác nhân chính | Bếp (tổ trưởng bếp) |
| Tác nhân phụ | Ban giám hiệu (duyệt thực đơn). Giáo viên (gửi thực đơn cho phụ huynh, ghi phản hồi và lịch về sớm) |
| Mục tiêu | Lập và duyệt thực đơn theo ngày và từng bữa. Gửi thông báo thực đơn tuần để ghi nhận phản hồi về dị ứng và lịch về sớm của trẻ trước khi chốt suất. |
| Điều kiện trước | Danh mục bữa ăn (sáng, trưa, xế) đã có tại [UC-03](uc-03-cau-hinh-moc-gio-va-bua-an.md). Giáo viên được phân công lớp (NV05). Hồ sơ trẻ có lưu ý dị ứng và chế độ ăn từ NV01 ([UC-16](uc-16-cap-nhat-ho-so-va-luu-y-cham-soc.md)). |
| Điều kiện sau | Thực đơn `Đã duyệt` là căn cứ cho [UC-15](uc-15-chot-va-doi-chieu-suat-an.md). Phản hồi của phụ huynh và lịch về sớm theo trẻ, ngày, bữa được lưu để giáo viên rà soát khi chốt suất. |

## Luồng sự kiện chính

1. Trước kỳ áp dụng, tổ trưởng bếp lập thực đơn theo ngày và từng bữa, rồi gửi duyệt (YC-SA-01).
2. Ban giám hiệu hoặc người được ủy quyền kiểm tra và duyệt thực đơn. Hệ thống mở thực đơn đã duyệt cho các lớp và cho xuất thông báo đầu tuần (QT-SA-01).
3. Giáo viên xem thực đơn tuần của lớp kèm lưu ý dị ứng và chế độ ăn từ NV01, gồm cảnh báo lưu ý chờ xác nhận (QT-HS-06). Giáo viên gửi thực đơn tuần cho phụ huynh qua kênh liên lạc của trường (YC-SA-02).
4. Khi phụ huynh phản hồi về món liên quan đến dị ứng của trẻ, giáo viên ghi mã trẻ, ngày, bữa bị ảnh hưởng và giờ phụ huynh dự kiến nhận trẻ về sớm trước bữa đó (QT-SA-03).

## Luồng thay thế và ngoại lệ

- **Bước 2 (trả lại thực đơn):** Người duyệt trả lại kèm ghi chú. Tổ trưởng bếp sửa và trình lại. Thực đơn chưa duyệt không được dùng để chốt suất ở `UC-15`.
- **Bước 3–4 (dị ứng mới từ phụ huynh):** Thông tin dị ứng chưa có trong hồ sơ. Giáo viên gửi đề nghị cập nhật sang NV01 tại `UC-16` để Văn phòng ghi và phó hiệu trưởng chuyên môn xác nhận. Cảnh báo sớm vẫn hiện trong lúc chờ xác nhận (QT-HS-06).
- **Bước 4 (thực đơn hoặc dị ứng thay đổi sau thông báo đầu tuần):** Bếp sửa thực đơn đã duyệt và trình duyệt lại, hoặc có lưu ý dị ứng mới ảnh hưởng món trong tuần. Hệ thống cảnh báo giáo viên của trẻ liên quan để liên hệ lại phụ huynh và cập nhật lịch về sớm (QT-SA-06). Nếu bữa đó đã chốt tại `UC-15`, giáo viên gửi điều chỉnh cho bếp và giữ nguyên số chốt ban đầu (YC-SA-04).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Tuần hoặc ngày áp dụng, bữa (`Sáng`, `Trưa`, `Xế`), danh sách món. Quyết định duyệt thực đơn. Lưu ý dị ứng từ NV01. Phản hồi của phụ huynh: mã trẻ, ngày, bữa, giờ dự kiến nhận trẻ về sớm. | Thực đơn theo ngày và bữa ở trạng thái `Nháp`, `Chờ duyệt` hoặc `Đã duyệt`. Bản xuất thông báo thực đơn tuần. Danh sách phản hồi và lịch về sớm theo trẻ, ngày, bữa chuyển sang `UC-15`. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Duyệt thực đơn tuần | Tổ trưởng bếp lập thực đơn tuần có ngày 14/10/2026 và 15/10/2026, Ban giám hiệu duyệt | Thực đơn `Đã duyệt`, các lớp xem được và xuất được thông báo đầu tuần. |
| Trả lại thực đơn | Ban giám hiệu trả lại thực đơn kèm ghi chú | Thực đơn về `Nháp`. `UC-15` không dùng thực đơn này để chốt suất. |
| Lịch về sớm do dị ứng | Thực đơn trưa ngày 15/10 có món liên quan lưu ý dị ứng tôm và cua của HS2026-042 ([Mẫu 01](../../survey/forms/mau-01-ho-so-tre.md), [Mẫu 03](../../survey/forms/mau-03-bao-suat-va-giao-nhan.md)) | Giáo viên thấy lưu ý khi xem thực đơn. Phụ huynh xác nhận nhận trẻ về trước bữa trưa. Lịch về sớm được lưu cho HS2026-042, ngày 15/10, bữa trưa. |
| Dị ứng mới chưa có trong hồ sơ | Phụ huynh báo trẻ dị ứng sữa khi nhận thông báo tuần | Giáo viên gửi đề nghị sang `UC-16`. Cảnh báo chờ xác nhận hiện ngay cho giáo viên lớp và bếp. |
| Đổi thực đơn sau khi bữa đã chốt | Bếp đổi món trưa đã chốt của một ngày, món mới liên quan dị ứng của một trẻ | Hệ thống cảnh báo giáo viên. Giáo viên gửi điều chỉnh giảm cho bếp tại `UC-15`, số chốt ban đầu giữ nguyên (QT-SA-06). |

## Liên phân hệ và truy vết

- **NV01 → NV07:** Lưu ý dị ứng còn hiệu lực và cảnh báo lưu ý chờ xác nhận, chỉ đọc (QT-HS-06). Giáo viên và bếp không sửa trực tiếp lưu ý chăm sóc tại NV07.
- **NV07 nội bộ (`UC-18` → `UC-15`):** Thực đơn đã duyệt và lịch về sớm theo trẻ, ngày, bữa là đầu vào bắt buộc để chốt suất.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-01 | QT-SA-01 | NV07 · Ghi nhận và duyệt thực đơn theo ngày và bữa làm căn cứ chốt suất. |
| YC-SA-02 | QT-SA-03, QT-HS-06 | NV01 → NV07 · Thông báo thực đơn tuần, đối chiếu dị ứng, lưu phản hồi và lịch về sớm. |
| YC-SA-04 | QT-SA-06 | NV07 · Rà soát lại lịch về sớm và số suất liên quan khi thực đơn hoặc dị ứng thay đổi. |
