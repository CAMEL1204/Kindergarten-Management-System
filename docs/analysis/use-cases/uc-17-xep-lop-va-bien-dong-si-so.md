# UC-17 · Xếp lớp ban đầu và quản lý biến động sĩ số

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV02 · F1.2.1, F1.2.3, F1.2.4 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu |
| Mục tiêu | Xếp trẻ mới từ danh sách chờ vào lớp phù hợp, quản lý các biến động nghỉ dài ngày, bảo lưu, thôi học, quay lại và tổng hợp đúng diện đi học theo ngày. |
| Điều kiện trước | Với xếp lớp mới: hồ sơ trẻ đã duyệt tiếp nhận và có ngày dự kiến bắt đầu học từ NV01. Với biến động: trẻ đang có hồ sơ xếp lớp hợp lệ tại NV02. Danh mục lớp, năm học, nhóm tuổi, mốc tính tuổi và chỉ tiêu đã được thiết lập. |
| Điều kiện sau | Trạng thái và khoảng xếp lớp có hiệu lực được lưu theo ngày hiệu lực; danh sách diện đi học theo ngày được cập nhật cho NV03, NV05, NV06, NV08 và số liệu biến động sẵn sàng cho báo cáo sĩ số NV09. |

## Luồng sự kiện chính

### Nhánh A · Xếp lớp ban đầu (vào lớp)

1. Văn phòng mở danh sách trẻ chờ xếp lớp do NV01 bàn giao (cùng mã hồ sơ) và chọn lớp dự kiến, ngày hiệu lực vào lớp.
2. Hệ thống tính tuổi theo mốc quy định (năm sinh với mẫu giáo; tháng tuổi tại ngày trẻ bắt đầu học với nhà trẻ), kiểm tra lớp đang hoạt động, phù hợp nhóm tuổi, sức chứa còn trống theo chỉ tiêu vận hành và trần tối đa Điều 15 (`QT-XL-01`), chưa có khoảng xếp lớp trùng ngày, đồng thời tra cứu trạng thái phân công giáo viên từ NV05 để cảnh báo nếu lớp thiếu giáo viên theo định mức (`QT-PC-03`).
3. Ban giám hiệu duyệt phương án xếp lớp và ngày hiệu lực.
4. Hệ thống tạo khoảng xếp lớp; đối chiếu ngày bắt đầu học từ NV01 với ngày hiệu lực để xác định trạng thái tại ngày tra cứu (`Đang học` khi thỏa cả hai điều kiện, hoặc `Chờ đủ điều kiện bắt đầu học` ở danh sách dự kiến).

### Nhánh B · Ghi nhận nghỉ dài ngày, bảo lưu, thôi học và quay lại

1. Văn phòng chọn trẻ và loại biến động (`Nghỉ dài ngày`, `Bảo lưu`, `Thôi học`, `Quay lại`), nhập ngày hiệu lực, ngày kết thúc dự kiến (nếu nghỉ dài ngày), lý do và ghi riêng thời điểm nhập (`QT-XL-05`).
2. Hệ thống kiểm tra điều kiện hợp lệ theo từng loại:
   - `Nghỉ dài ngày`: có ngày bắt đầu và ngày kết thúc dự kiến; trẻ vẫn giữ chỗ và tính vào sĩ số đang học của lớp.
   - `Bảo lưu`: rút trẻ khỏi danh sách đang học và ngừng thuộc diện điểm danh từ ngày hiệu lực, chuyển sang danh sách bảo lưu.
   - `Thôi học`: đóng khoảng xếp lớp hiện hành tại ngày hiệu lực, giữ lịch sử và ngừng thuộc diện điểm danh.
   - `Quay lại` (sau bảo lưu): đối chiếu hồ sơ hiện hành tại NV01, kiểm tra chỉ tiêu/trần Điều 15 của lớp tiếp nhận tại ngày quay lại (`QT-XL-01`) và cảnh báo nếu lớp thiếu giáo viên theo định mức `NV05` (`QT-PC-03`).
3. Ban giám hiệu duyệt biến động và ngày hiệu lực.
4. Hệ thống cập nhật trạng thái/khoảng xếp lớp theo quy ước ngày hiệu lực (`QT-XL-02`), cập nhật diện đi học cho NV03, NV06, NV08 và ghi nhận tăng/giảm sĩ số theo lớp, khối, năm học (`YC-XL-04`).

## Luồng thay thế và ngoại lệ

- **Nhánh A & B (trần Điều 15 và trường hợp đặc biệt):** Nếu lớp đạt mức tối đa theo Điều 15 Điều lệ Trường mầm non (`QT-XL-01`), hệ thống chặn cứng. Nếu chỉ vượt chỉ tiêu vận hành trong giới hạn Điều 15 hoặc lệch nhóm tuổi, cảnh báo để Ban giám hiệu quyết định.
- **Nhánh A, Bước 4 (hồ sơ NV01 còn thiếu giấy tờ mốc đi học):** Trẻ đã xếp lớp vẫn nằm trong danh sách dự kiến; danh sách điểm danh NV03 và diện tạm thu NV06 chỉ bắt đầu từ ngày học đầu tiên thỏa cả hai điều kiện (đủ điều kiện bắt đầu học và ngày hiệu lực xếp lớp).
- **Nhánh B, Bước 1–4 (ghi hồi tố `QT-XL-05`):** Nếu nhập sau ngày hiệu lực, bắt buộc có lý do, liệt kê các ngày đã khóa liên quan để đối chiếu trước khi duyệt; sau khi duyệt hồi tố, gửi cảnh báo đối chiếu ngày đã khóa sang `UC-14` (`NV03`), thông báo `NV06` tính bù trừ/tỷ lệ lại, và với báo cáo sĩ số `NV09` đã nộp của kỳ liên quan thì giữ ảnh chụp và cảnh báo người lập (`Đã gửi` để Ban giám hiệu xử lý yêu cầu bổ sung; `Đã duyệt` chuyển sang `Cần cập nhật` theo `QT-BC-04`).
- **Nhánh B (`Thôi học` không còn kỳ thu sau):** Thông báo `NV06` chuyển phần chênh lệch tiền ăn hoặc số dư có sang luồng điều chỉnh/hoàn tiền (`QT-HP-05`).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã hồ sơ trẻ (NV01), lớp, loại biến động (`Vào lớp`, `Nghỉ dài ngày`, `Bảo lưu`, `Thôi học`, `Quay lại`), ngày hiệu lực, ngày kết thúc dự kiến (nếu có), thời điểm nhập, lý do, quyết định duyệt. | Kết quả và lịch sử xếp lớp, danh sách lớp và diện đi học theo ngày, danh sách dự kiến/bảo lưu, biến động tăng/giảm và bảng tổng hợp sĩ số đầu kỳ/tăng/giảm/cuối kỳ phục vụ tra cứu và báo cáo NV09. |

## Liên phân hệ và truy vết

- **NV01 → NV02:** Nhận hồ sơ đủ điều kiện tiếp nhận cùng mã hồ sơ và ngày được bắt đầu học.
- **NV02 → NV05:** Cung cấp danh sách lớp đang hoạt động tại ngày hiệu lực để phân công giáo viên (`QT-PC-01`).
- **NV02 → NV03:** Cung cấp danh sách thuộc diện điểm danh theo ngày; với trẻ `Nghỉ dài ngày`, NV03 tự khởi tạo `Vắng có phép` ngay từ đầu ngày (`QT-DD-06`). Trẻ `Bảo lưu`, `Thôi học`, hoặc `Chờ đủ điều kiện bắt đầu học` không nằm trong danh sách điểm danh.
- **NV02 → NV06:** Cung cấp khoảng thời gian trẻ thuộc diện đi học cùng lịch học trường để tạm thu tiền ăn (`QT-HP-02`) và tính khoản tháng theo tỷ lệ khi vào học, thôi học, bắt đầu hoặc kết thúc bảo lưu giữa kỳ; `Nghỉ dài ngày` vẫn thu đủ khoản tháng (`QT-HP-06`).
- **NV02 → NV09:** Cung cấp số liệu sĩ số đầu kỳ, tăng, giảm (tách biệt nhập học/thôi học với chuyển lớp nội bộ) và cuối kỳ theo lớp/khối/năm học để Văn phòng lập và gửi báo cáo tại `UC-10`.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-01 | QT-XL-01, QT-PC-01, QT-PC-03 | Nhận hồ sơ NV01, kiểm tra mốc tuổi, chỉ tiêu vận hành, trần Điều 15 và cảnh báo định mức giáo viên khi xếp lớp ban đầu hoặc quay lại. |
| YC-XL-02 | QT-XL-02, QT-XL-04, QT-DD-06, QT-HP-06 | Lập, duyệt và cập nhật vào lớp, nghỉ dài ngày, bảo lưu, thôi học, quay lại; bảo toàn lịch sử sĩ số. |
| YC-XL-03 | QT-XL-05, QT-HP-02, QT-HP-05, QT-BC-04 | Cung cấp danh sách theo ngày và diện đi học; kiểm soát hồi tố, chuyển hoàn/điều chỉnh `Thôi học` sang `NV06` và cập nhật báo cáo `NV09`. |
| YC-XL-04 | QT-XL-04 | Tổng hợp, tra cứu, xuất sĩ số đầu kỳ/tăng/giảm/cuối kỳ và cung cấp nguồn cho báo cáo Văn phòng (NV09). |
