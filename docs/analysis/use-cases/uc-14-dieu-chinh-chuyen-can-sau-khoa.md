# UC-14 · Điều chỉnh chuyên cần sau khóa

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV03 · F1.3.3 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Ban giám hiệu hoặc người được phân quyền duyệt điều chỉnh |
| Mục tiêu | Tiếp nhận, kiểm soát và duyệt các thay đổi chuyên cần phát sinh từ mốc khóa của ngày học (mặc định 09:00). Lưu đủ giá trị trước và sau, cập nhật trạng thái cho học phí và báo cáo. |
| Điều kiện trước | Dòng chuyên cần của ngày học đã `Đã khóa`. Có một trong các căn cứ: lượt đón hoặc hủy lượt đón ghi nhận từ mốc khóa tại NV08 (QT-DT-06, QT-DT-07), căn cứ xác minh vắng mới từ phụ huynh (QT-DD-02), hoặc ngày đã khóa cần đối chiếu do xếp lớp, chuyển lớp hồi tố tại NV02 (QT-XL-05). |
| Điều kiện sau | Đề nghị ở trạng thái `Đã duyệt` hoặc `Từ chối`, kèm giá trị trước và sau, căn cứ, người đề nghị, người duyệt và thời điểm. Chỉ đề nghị đã duyệt mới đổi kết quả chuyên cần chính thức và số ngày có mặt của kỳ. |

## Luồng sự kiện chính

1. Đề nghị điều chỉnh được tạo theo một trong hai cách:
   - **Tự động từ NV08:** Giáo viên hoặc nhân sự trực được cấp quyền ghi lượt đón mới hoặc hủy lượt đón từ mốc khóa có hiệu lực ([UC-03](uc-03-cau-hinh-moc-gio-va-bua-an.md)). Hệ thống tự tạo đề nghị gắn mã sự kiện NV08.
   - **Thủ công từ giáo viên:** Giáo viên chọn dòng đã khóa, ví dụ `Chờ xác minh` khi phụ huynh phản hồi sau mốc khóa hoặc ngày bị ảnh hưởng bởi xếp lớp hồi tố. Giáo viên chọn trạng thái đề xuất và nhập căn cứ.
2. Hệ thống lưu đề nghị ở trạng thái `Chờ duyệt`: kết quả trước và sau, lý do, mã lượt đón hoặc căn cứ, người đề nghị, thời điểm đề nghị. Kết quả hiện hành giữ nguyên tới khi duyệt.
3. Người duyệt đối chiếu lớp, ngày, căn cứ và kiểm tra điều kiện:
   - Đề xuất đổi sang `Có mặt` chỉ được duyệt khi lượt đón tương ứng ở NV08 còn hợp lệ, chưa hủy.
   - Đề xuất đổi từ `Có mặt` sang vắng do hủy lượt đón chỉ được duyệt khi lượt đón đó đã hủy hợp lệ theo QT-DT-07.
4. Người duyệt ghi quyết định `Đã duyệt`, lý do và thời điểm. Hệ thống cập nhật kết quả của ngày học, tính lại tổng hợp kỳ, cập nhật trạng thái đối soát NV06 và thông báo cho các bộ phận đã dùng số liệu, gồm NV09 nếu báo cáo kỳ đã gửi hoặc đã duyệt (YC-DD-03).

## Luồng thay thế và ngoại lệ

- **Bước 1 và 3 (xếp lớp, chuyển lớp hồi tố từ NV02):** Khi NV02 duyệt quyết định hồi tố tại [UC-12](uc-12-chuyen-lop.md) hoặc [UC-17](uc-17-xep-lop-va-bien-dong-si-so.md), các ngày đã khóa trong khoảng hồi tố được gắn cờ cần đối chiếu. Giáo viên lớp đích, hoặc giáo viên lớp cũ với dòng thừa, rà soát lượt đón thực tế và tạo đề nghị ghi đúng lớp có hiệu lực của trẻ trong ngày. Dòng ở lớp cũ được xử lý để không đếm trùng theo mã trẻ và ngày (QT-DD-01).
- **Bước 2 (nhắc hạn trước khi lập bảng phải thu):** Hệ thống nhắc người duyệt xử lý xong các đề nghị `Chờ duyệt` trước ngày học đầu kỳ kế tiếp (QT-DD-07). Nếu sang kỳ thu mới mà trẻ vẫn còn đề nghị chờ duyệt có thể đổi `Có mặt`, chỉ trẻ đó bị treo đối soát tại NV06. Treo kéo dài tới khi mọi ngày liên quan đã khóa và không còn đề nghị như vậy (QT-HP-05).
- **Bước 3 (từ chối hoặc xung đột dữ liệu gốc):** Căn cứ không phù hợp hoặc điều kiện lượt đón ở NV08 không thỏa. Người duyệt chọn `Từ chối` kèm lý do, kết quả hiện hành giữ nguyên. Nếu dòng gốc vừa bị một đề nghị khác thay đổi trong lúc chờ, hệ thống cảnh báo để người duyệt đối chiếu lại.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp, ngày học. Nguồn phát sinh: NV08, xác minh bổ sung hoặc hồi tố NV02. Mã lượt đón hoặc lượt hủy. Trạng thái cũ, trạng thái đề xuất, căn cứ, lý do. Quyết định duyệt. | Trạng thái đề nghị `Chờ duyệt`, `Đã duyệt` hoặc `Từ chối`. Lịch sử trước và sau, người và thời điểm đề nghị, duyệt. Số ngày có mặt cập nhật. Trạng thái treo hoặc gỡ treo đối soát NV06. Cảnh báo `Cần cập nhật` cho NV09. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 02](../../survey/forms/mau-02-diem-danh-giao-nhan.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Xác minh sau mốc khóa | HS2026-061 khóa 09:00 với `Chờ xác minh` ngày 14/10/2026. Lúc 10:05 người giám hộ xác nhận trẻ nghỉ chưa xin phép | Giáo viên đề nghị đổi sang `Vắng không phép` lúc 10:05. Hiệu trưởng duyệt lúc 10:15. Tổng hợp 17:30 của Mầm 1: 23 `Có mặt`, 1 `Vắng có phép`, 1 `Vắng không phép`. |
| Lượt đón đúng mốc khóa | HS2026-042 được đón lúc 09:00 ngày 16/10/2026, trả 16:30, hiệu trưởng duyệt lúc 16:45 | Đề nghị đổi sang `Có mặt` được tạo tự động lúc 09:00. Kết quả khóa giữ tới 16:45. Sau duyệt là `Có mặt`, cờ `Đến muộn`. Lượt trả không hủy đề nghị. |
| Hủy lượt đón sau mốc khóa | Lượt đón ghi nhầm lúc 07:40, phát hiện và hủy lúc 09:20 | Đề nghị tự động được tạo. `Có mặt` giữ tới khi duyệt. Duyệt được vì lượt đón đã hủy hợp lệ. |
| Lượt đón đã bị hủy | Đề nghị đổi sang `Có mặt` nhưng lượt đón tương ứng đã bị hủy | Không duyệt được. Người duyệt chọn `Từ chối`, kết quả giữ nguyên. |
| Đề nghị còn chờ khi lập phải thu | Ngày 30/09 còn một lượt `Chờ xác minh` không có lượt đón và một đề nghị đổi sang `Có mặt` đang chờ | Ngày 01/10, NV06 đối soát các trẻ khác bình thường. Chỉ trẻ có đề nghị chờ duyệt bị treo (QT-DD-07). |
| Báo cáo đã duyệt | Báo cáo chuyên cần tháng của Mầm 1 đã duyệt, sau đó một đề nghị của kỳ được duyệt | Báo cáo chuyển sang `Cần cập nhật`. Bảng tổng hợp vẫn dùng bản đã duyệt và đánh dấu đang cập nhật (QT-BC-04, QT-BC-05). |

## Liên phân hệ và truy vết

- **NV08 → NV03:** NV08 vẫn ghi giao nhận thực tế từ mốc khóa trở đi mà không bị chặn. Mọi lượt đón hoặc hủy lượt đón từ mốc khóa tại [UC-08](uc-08-don-tre-theo-nhom.md) tự sinh đề nghị điều chỉnh.
- **NV02 → NV03:** Quyết định xếp lớp, chuyển lớp hồi tố tạo danh sách ngày đã khóa cần đối chiếu.
- **NV03 → NV07:** Thay đổi sau khóa không sửa bản bàn giao buổi sáng (mặc định 08:30). Thay đổi ảnh hưởng bữa ăn do giáo viên và bếp xử lý riêng tại [UC-15](uc-15-chot-va-doi-chieu-suat-an.md) (QT-SA-04).
- **NV03 → NV06:** Lượt `Chờ xác minh` không có lượt đón tính 0 ngày và không làm treo đối soát. Đối soát của trẻ bị treo khi trẻ còn ngày chưa khóa hoặc còn ít nhất một đề nghị chờ duyệt có thể đổi `Có mặt`. Chỉ gỡ treo khi mọi ngày liên quan của trẻ trong kỳ đã khóa và không còn đề nghị như vậy (QT-DD-07, QT-HP-05). Xem [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md).
- **NV03 → NV09:** Đề nghị được duyệt làm đổi số liệu của báo cáo đã nộp tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md). Hệ thống giữ ảnh chụp và cảnh báo người lập. Báo cáo `Đã gửi` do Ban giám hiệu xử lý yêu cầu bổ sung. Báo cáo `Đã duyệt` chuyển sang `Cần cập nhật` (QT-BC-04, QT-BC-05).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DD-03, YC-DD-05 | QT-DD-01, QT-DD-02, QT-DD-03, QT-DD-05, QT-DD-07. Liên phân hệ: QT-DT-06, QT-DT-07, QT-XL-05, QT-SA-04, QT-HP-05, QT-BC-04, QT-BC-05 | NV08, NV02 → NV03 → NV06, NV07, NV09 · Duyệt thay đổi sau khóa, xử lý hồi tố, tách điều chỉnh suất ăn, treo và gỡ treo đối soát, cập nhật báo cáo. |
