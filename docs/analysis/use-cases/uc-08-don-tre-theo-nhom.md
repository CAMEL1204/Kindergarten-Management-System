# UC-08 · Đón trẻ theo nhóm

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV08 · F1.8.1, F1.8.2, F1.8.5, F1.8.6 |
| Tác nhân chính | Giáo viên được phân công lớp trong ngày hoặc nhân sự trực được cấp quyền NV08 (ghi lượt đón). Giáo viên hoặc Văn phòng (ghi báo nghỉ). |
| Tác nhân phụ | Không có |
| Mục tiêu | Ghi lượt đón thực tế của nhiều trẻ trong một lần xác nhận. Lượt đón là nguồn duy nhất tạo kết quả `Có mặt` ở NV03. Báo nghỉ được lưu một nơi làm căn cứ xác minh vắng. |
| Điều kiện trước | Ngày có lịch học. Người thực hiện thuộc danh sách giáo viên phụ trách lớp trong ngày của NV05, không có đơn nghỉ đã duyệt và không tạm nghỉ, hoặc có quyền trực NV08 cho lớp và thời gian đó tại `UC-01` (QT-DT-05). NV02 có danh sách trẻ thuộc lớp trong ngày. Khung đón, mốc bàn giao và mốc khóa có hiệu lực đã cấu hình tại `UC-03` (mặc định 07:00–08:30, 08:30, 09:00). |
| Điều kiện sau | Mỗi trẻ được đón có một lượt đón chưa hủy, ghi giờ đến thực tế, người ghi và cờ đến muộn nếu có. NV03 đã nhận căn cứ: cập nhật `Có mặt` trước mốc khóa hoặc có đề nghị điều chỉnh từ mốc khóa. Trẻ đã đón có trong danh sách trả của [UC-09](uc-09-tra-tre-theo-nhom.md). |

## Luồng sự kiện chính

1. Người thực hiện mở màn hình đón trẻ theo lớp và ngày. Hệ thống kiểm tra quyền theo lớp và thời gian ở tầng nghiệp vụ (NFR-01, QT-DT-05).
2. Hệ thống lấy danh sách trẻ thuộc diện điểm danh trong ngày từ NV02. Mỗi dòng có trạng thái giao nhận (`Chưa đón`, `Đã đón`, `Đã trả`), nhãn báo nghỉ và nhãn nghỉ dài ngày nếu có. Bộ lọc mặc định là `Chưa đón`.
3. Khi trẻ được đưa đến, người thực hiện đánh dấu ô chọn của các trẻ đó hoặc dùng "Chọn tất cả" cho các dòng đang hiển thị. Thanh thao tác hiện "Đã chọn n trẻ" và nút "Đón n trẻ đã chọn" (YC-DT-02).
4. Hộp xác nhận liệt kê trẻ đã chọn. Người thực hiện có thể ghi người đưa trẻ đến và ghi chú khi nhận trẻ. Người thực hiện bấm xác nhận một lần.
5. Hệ thống kiểm tra lại từng dòng: trẻ thuộc lớp trong ngày, chưa có lượt đón chưa hủy, người thực hiện còn quyền. Dòng không đạt bị loại và được báo lý do.
6. Hệ thống ghi lượt đón cho các dòng hợp lệ trong một giao dịch (NFR-05): mã lượt đón, giờ đến thực tế, thời điểm ghi nhận, người ghi. Trẻ có giờ đến từ mốc bàn giao có hiệu lực trở đi được gắn cờ `Đến muộn` (QT-DT-06).
7. Hệ thống chuyển căn cứ sang NV03 theo thời điểm ghi nhận (QT-DT-01). Trước mốc khóa, `UC-13` cập nhật `Có mặt`. Từ mốc khóa, `UC-14` nhận đề nghị điều chỉnh kèm mã lượt đón, kết quả đã khóa giữ nguyên.
8. Hệ thống báo số trẻ đã đón và danh sách trẻ bị bỏ qua. Trẻ đã đón chuyển sang `Đã đón` và xuất hiện trong danh sách trả.

## Luồng con: ghi nhận báo nghỉ (F1.8.1)

1. Giáo viên (lớp mình) hoặc Văn phòng (toàn trường) chọn trẻ, ngày hoặc khoảng nghỉ, lý do, kênh báo và thời điểm nhận.
2. Hệ thống lưu báo nghỉ với người ghi nhận và hiển thị nhãn trên danh sách đón (YC-DT-05).
3. Hệ thống chuyển báo nghỉ cho NV03 làm căn cứ xác minh vắng và cho NV07 lập danh sách ăn dự kiến. Báo nghỉ không tự tạo kết quả vắng.

## Luồng con: hủy lượt đón ghi nhầm (F1.8.6)

1. Giáo viên phụ trách lớp hoặc người đã ghi chọn lượt đón và nhập lý do hủy.
2. Hệ thống chặn nếu trẻ còn lượt trả chưa hủy.
3. Hệ thống không xóa lượt đón mà đặt trạng thái `Đã hủy`, lưu lý do, người hủy và thời điểm (QT-DT-07, NFR-04). Trẻ về `Chưa đón`.
4. Trước mốc khóa, NV03 tính lại kết quả từ căn cứ còn lại như báo nghỉ và xác minh. Từ mốc khóa, hệ thống tạo đề nghị điều chỉnh tại `UC-14`, kết quả `Có mặt` giữ nguyên tới khi duyệt. Bản bàn giao buổi sáng không đổi. Thay đổi ảnh hưởng bữa ăn chuyển NV07 (QT-SA-04).

## Luồng thay thế và ngoại lệ

- **Bước 1 (không có quyền):** Ngày nghỉ theo lịch trường, giáo viên không thuộc phân công ngày đó, có đơn nghỉ đã duyệt hoặc đang tạm nghỉ, hoặc nhân sự trực ngoài phạm vi được giao: hệ thống chặn thao tác.
- **Bước 3 (trẻ đã báo nghỉ vẫn đến):** Trẻ vẫn được chọn và đón. Báo nghỉ và căn cứ xác minh đã ghi được giữ (QT-DT-06).
- **Bước 5 (trẻ không thuộc lớp trong ngày):** Dòng bị loại. Trẻ đã chuyển lớp được đón ở lớp mới theo NV02.
- **Bước 7 (đến đúng hoặc sau mốc khóa):** Trẻ đến lúc 09:00 vẫn được đón và có trong danh sách trả. NV03 nhận đề nghị điều chỉnh (QT-DD-03).
- **Bổ sung lượt đón bị sót:** Nếu khi trả phát hiện trẻ thực tế có mặt nhưng chưa có lượt đón, giáo viên xác minh rồi ghi lượt đón trước khi ghi trả (QT-DT-02). Thời điểm ghi nhận quyết định trẻ đi vào `UC-13` hay `UC-14`.
- **Lớp đông nhất:** Đón 35 trẻ của Lá 1 trong một lần xác nhận không gây treo giao diện (NFR-07).

## Tra cứu đón trả (F1.8.5)

- Giáo viên tra lớp mình, Văn phòng và Ban giám hiệu tra toàn trường. Tra theo trẻ, lớp, ngày và người nhận thực tế. Lọc lượt đến muộn, lượt đã hủy (YC-DT-06).
- Số lượt đến muộn trong kỳ là số liệu cho báo cáo chuyên cần lớp tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Lớp, ngày. Danh sách trẻ thuộc lớp trong ngày và nghỉ dài ngày (NV02). Giáo viên phụ trách lớp trong ngày (NV05). Quyền trực NV08 (`UC-01`). Mốc có hiệu lực (`UC-03`). Danh sách trẻ được chọn, người đưa đến, ghi chú. Báo nghỉ: trẻ, ngày, lý do, kênh, thời điểm. Lý do hủy. | Lượt đón: mã, trẻ, lớp, ngày, giờ đến thực tế, thời điểm ghi nhận, người ghi, cờ đến muộn, trạng thái hủy. Báo nghỉ có người ghi nhận. Căn cứ chuyển NV03 (cập nhật trực tiếp hoặc đề nghị điều chỉnh). Danh sách trẻ bị bỏ qua kèm lý do. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 02](../../survey/forms/mau-02-diem-danh-giao-nhan.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Đón trước mốc bàn giao | HS2026-001 đến 07:45 ngày 14/10/2026 | Lượt đón 07:45, NV03 ghi `Có mặt`, không đến muộn. |
| Báo nghỉ rồi vẫn đến | HS2026-042 báo nghỉ 07:20, bà ngoại đưa đến 08:50 | Lượt đón 08:50, cờ `Đến muộn`. NV03 cập nhật `Có mặt`. Báo nghỉ và bản bàn giao 08:30 được giữ. |
| Đến đúng mốc khóa | HS2026-042 đến 09:00 ngày 16/10/2026 | Lượt đón được ghi, trẻ có trong danh sách trả. NV03 giữ kết quả khóa và nhận đề nghị đổi sang `Có mặt`. |
| Hủy trước mốc khóa | 07:40 chọn nhầm một trẻ đã báo nghỉ, phát hiện lúc 08:05 | Lượt đón `Đã hủy`, NV03 đưa trẻ về kết quả theo báo nghỉ. |
| Hủy sau mốc khóa | Cùng ca trên, phát hiện lúc 09:20 | Lượt đón `Đã hủy`, NV03 nhận đề nghị điều chỉnh, `Có mặt` giữ tới khi duyệt. |
| Giáo viên đang tạm nghỉ | Lớp Mầm 2 ngày 20/10/2026, Trần Thu Trang tạm nghỉ | Tài khoản của Trần Thu Trang bị khóa từ 19/10 nên không thao tác được (YC-NS-05). Trịnh Thị Nga dạy thay được ghi lượt đón lớp Mầm 2 ([Mẫu 07](../../survey/forms/mau-07-phan-cong-va-day-thay.md)). |
| Giáo viên có đơn nghỉ ngắn ngày | Giáo viên có đơn nghỉ đã duyệt mở lớp mình trong ngày nghỉ | Tài khoản vẫn mở nhưng thao tác trên lớp bị chặn (QT-PC-06). |

## Liên phân hệ và truy vết

- **NV02 → NV08:** Danh sách trẻ thuộc lớp trong ngày và nhãn nghỉ dài ngày từ `UC-12`, `UC-17`.
- **NV05, F1.0 → NV08:** Giáo viên phụ trách lớp theo ngày từ [UC-05](uc-05-lap-phan-cong-giao-vien.md). Quyền trực NV08 từ `UC-01` (QT-DT-05). Mốc giờ có hiệu lực từ `UC-03`.
- **NV08 → NV03:** Lượt đón là nguồn duy nhất của `Có mặt`. Trước mốc khóa cập nhật `UC-13`, từ mốc khóa chuyển `UC-14` (QT-DT-01, QT-DT-06, QT-DT-07). Báo nghỉ là căn cứ xác minh vắng.
- **NV08 → NV07:** Báo nghỉ cho danh sách ăn dự kiến. Hủy hoặc đón muộn ảnh hưởng bữa ăn chuyển `UC-15` (QT-SA-04).
- **NV08 → NV09:** Số lượt đến muộn cho báo cáo lớp tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DT-01, YC-DT-02 | QT-DT-01, QT-DT-03, QT-DT-06 | NV02 → NV08 → NV03 · Màn hình đón theo lớp và ngày, chọn nhiều trẻ, xác nhận một lần. |
| YC-DT-04 | QT-DT-06, QT-DD-03 | F1.0 → NV08 → NV03 · Mốc bàn giao, mốc khóa, cờ đến muộn. |
| YC-DT-05 | QT-DT-05, QT-DT-06 | NV08 → NV03, NV07 · Báo nghỉ tại Giáo viên và Văn phòng. |
| YC-DT-06 | QT-DT-05 | NV08 → NV09 · Tra cứu và số liệu đến muộn. |
| YC-DT-08 | QT-DT-07, QT-SA-04 | NV08 → NV03, NV07 · Hủy lượt đón ghi nhầm theo mốc khóa. |
