# UC-14 · Điều chỉnh chuyên cần sau khóa

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV03 · F1.3.3 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Ban giám hiệu hoặc người được phân quyền duyệt |
| Mục tiêu | Tiếp nhận, kiểm soát và phê duyệt các thay đổi chuyên cần phát sinh từ mốc khóa có hiệu lực của ngày học (mặc định 09:00) trở đi, lưu đủ vết trước/sau và đồng bộ trạng thái cho học phí, báo cáo. |
| Điều kiện trước | Dòng chuyên cần của ngày học đã ở trạng thái `Đã khóa`. Phát sinh một trong các căn cứ: lượt đón hoặc hủy lượt đón được ghi nhận từ mốc khóa có hiệu lực tại NV08 (`QT-DT-06`, `QT-DT-07`), căn cứ xác minh vắng mới từ phụ huynh (`QT-DD-02`), hoặc ngày đã khóa cần đối chiếu do xếp lớp/chuyển lớp hồi tố tại NV02 (`QT-XL-05`). |
| Điều kiện sau | Đề nghị được `Đã duyệt` hoặc `Từ chối` kèm lịch sử trước/sau, căn cứ, người đề nghị, người duyệt và thời điểm. Chỉ kết quả đã duyệt mới làm thay đổi kết quả chuyên cần chính thức và số ngày có mặt của kỳ. |

## Luồng sự kiện chính

1. Khởi tạo đề nghị điều chỉnh theo một trong ba nguồn:
   - **Tự động từ NV08:** Khi giáo viên hoặc nhân sự trực được cấp quyền ghi nhận lượt đón mới hoặc hủy lượt đón từ mốc khóa có hiệu lực của ngày học trở đi (mặc định `09:00`, theo cấu hình `UC-03`), hệ thống tự tạo đề nghị điều chỉnh gắn mã sự kiện NV08.
   - **Thủ công từ giáo viên:** Giáo viên chọn dòng đã khóa (ví dụ `Chờ xác minh` khi phụ huynh phản hồi sau mốc khóa, hoặc ngày bị ảnh hưởng bởi xếp lớp hồi tố), chọn trạng thái đề xuất và đính kèm/nhập căn cứ.
2. Hệ thống lưu bản ghi đề nghị gồm kết quả trước và sau đề xuất, lý do, mã lượt đón/căn cứ, người đề nghị và thời điểm đề nghị ở trạng thái `Chờ duyệt`. Kết quả chuyên cần hiện hành vẫn giữ nguyên hiệu lực cho tới khi được duyệt.
3. Ban giám hiệu hoặc người được phân quyền duyệt đối chiếu lớp, ngày, chứng cứ và kiểm tra điều kiện hợp lệ:
   - Nếu đề xuất chuyển thành `Có mặt`, chỉ cho phép duyệt khi vẫn còn lượt đón hợp lệ (chưa bị hủy) tại NV08.
   - Nếu đề xuất chuyển từ `Có mặt` sang vắng do hủy lượt đón, chỉ cho phép duyệt khi lượt đón tương ứng tại NV08 đã được hủy hợp lệ theo `QT-DT-07`.
4. Người duyệt ghi quyết định (`Đã duyệt`), lý do và thời điểm. Hệ thống cập nhật kết quả chuyên cần ngày học, tính lại tổng hợp kỳ, cập nhật trạng thái đối soát NV06 và thông báo cho các bộ phận đã sử dụng số liệu (gồm NV09 nếu báo cáo kỳ đã gửi/duyệt).

## Luồng thay thế và ngoại lệ

- **Bước 1 & Bước 3 (xếp lớp/chuyển lớp hồi tố từ NV02):** Khi NV02 duyệt quyết định hồi tố qua `UC-12` hoặc `UC-17`, các ngày đã khóa thuộc khoảng hồi tố được gắn cờ cần đối chiếu. Giáo viên lớp đích (hoặc lớp cũ đối với dòng thừa) rà soát lượt đón thực tế và tạo đề nghị gán đúng lớp có hiệu lực của trẻ trong ngày; nếu trẻ thuộc lớp mới theo hồi tố, dòng ở lớp cũ được xử lý tránh đếm trùng theo mã trẻ và ngày (`QT-DD-01`).
- **Bước 2 (nhắc hạn trước khi lập bảng phải thu NV06):** Hệ thống nhắc người duyệt xử lý dứt điểm các đề nghị `Chờ duyệt` trước ngày học đầu kỳ kế tiếp (`QT-DD-07`). Nếu sang kỳ thu mới mà trẻ vẫn còn đề nghị `Chờ duyệt` có thể làm thay đổi kết quả `Có mặt`, chỉ trẻ đó bị treo đối soát tại NV06 (`QT-HP-05`).
- **Bước 3 (từ chối hoặc xung đột dữ liệu gốc):** Nếu chứng cứ không phù hợp hoặc điều kiện lượt đón tại NV08 không thỏa, người duyệt chọn `Từ chối` kèm lý do và giữ nguyên kết quả hiện hành. Nếu dòng gốc vừa thay đổi bởi một đề nghị khác trong lúc chờ duyệt, hệ thống cảnh báo để người duyệt đối chiếu lại trước khi xác nhận.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp, ngày học, nguồn phát sinh (`NV08` / xác minh bổ sung / hồi tố `NV02`), mã lượt đón hoặc lượt hủy, trạng thái cũ, trạng thái đề xuất, căn cứ, lý do, quyết định duyệt. | Trạng thái đề nghị (`Chờ duyệt` / `Đã duyệt` / `Từ chối`), lịch sử trước/sau, người và thời điểm đề nghị/duyệt, số ngày có mặt cập nhật, trạng thái gỡ/treo đối soát NV06, cảnh báo `Cần cập nhật` tới NV09. |

## Liên phân hệ và truy vết

- **NV08 → NV03:** NV08 tiếp tục ghi nhận giao nhận thực tế từ mốc khóa có hiệu lực trở đi (mặc định `09:00`) mà không bị chặn; mọi lượt đón hoặc hủy lượt đón từ mốc khóa tự động sinh đề nghị điều chỉnh tại `UC-14`.
- **NV02 → NV03:** Quyết định xếp lớp/chuyển lớp hồi tố tạo danh sách ngày đã khóa cần đối chiếu tại `UC-14`.
- **NV03 → NV07:** Thay đổi chuyên cần sau khóa không tự sửa bản bàn giao sáng có hiệu lực (mặc định `08:30`); các thay đổi ảnh hưởng bữa ăn được giáo viên và bếp xử lý riêng theo tiến độ thực tế tại `UC-15` (`QT-SA-04`).
- **NV03 → NV06:** Lượt `Chờ xác minh` chưa có lượt đón tính `0` ngày có mặt và không treo đối soát. Chỉ treo đối soát của trẻ trong kỳ khi còn ít nhất một đề nghị chờ duyệt có thể đổi kết quả `Có mặt`. Gỡ treo khi không còn đề nghị nào thỏa điều kiện đó (`QT-DD-07`, `QT-HP-05`).
- **NV03 → NV09:** Khi đề nghị được duyệt làm thay đổi số liệu chuyên cần kỳ của báo cáo đã nộp tại `NV09`, hệ thống giữ ảnh chụp và cảnh báo người lập: nếu báo cáo đang `Đã gửi`, Ban giám hiệu xử lý yêu cầu bổ sung; nếu báo cáo đang `Đã duyệt`, chuyển sang trạng thái `Cần cập nhật` để lập phiên bản mới (`QT-BC-04`, `QT-BC-05`).

| Yêu cầu | Quy tắc thực thi trực tiếp | Ràng buộc liên phân hệ | Phạm vi |
| --- | --- | --- | --- |
| YC-DD-03, YC-DD-05 | QT-DD-01, QT-DD-02, QT-DD-03, QT-DD-05, QT-DD-07 | QT-DT-06, QT-DT-07, QT-XL-05, QT-SA-04, QT-HP-05, QT-BC-04, QT-BC-05 | NV08, NV02 → NV03 → NV06, NV07, NV09 · Duyệt thay đổi sau khóa, xử lý hồi tố, điều chỉnh suất riêng, treo/gỡ đối soát trẻ và cập nhật báo cáo. |
