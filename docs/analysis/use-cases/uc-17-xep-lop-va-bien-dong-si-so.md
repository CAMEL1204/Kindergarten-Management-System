# UC-17 · Xếp lớp ban đầu và quản lý biến động sĩ số

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV02 · F1.2.1, F1.2.3, F1.2.4 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu (duyệt xếp lớp và biến động) |
| Mục tiêu | Xếp trẻ mới từ danh sách chờ vào lớp phù hợp. Quản lý nghỉ dài ngày, bảo lưu, thôi học, quay lại. Tổng hợp đúng diện đi học theo ngày. |
| Điều kiện trước | Với xếp lớp mới: hồ sơ trẻ đã duyệt tiếp nhận và có ngày dự kiến bắt đầu học từ NV01. Với biến động: trẻ đang có xếp lớp hợp lệ tại NV02. Danh mục lớp, năm học, nhóm tuổi, mốc tính tuổi và chỉ tiêu đã có. |
| Điều kiện sau | Trạng thái và khoảng xếp lớp được lưu theo ngày hiệu lực. Diện đi học theo ngày được cập nhật cho NV03, NV05, NV06, NV08. Số liệu biến động sẵn sàng cho báo cáo sĩ số NV09. |

## Luồng sự kiện chính

### Nhánh A · Xếp lớp ban đầu

1. Văn phòng mở danh sách trẻ chờ xếp lớp do NV01 bàn giao, chọn lớp dự kiến và ngày hiệu lực vào lớp.
2. Hệ thống tính tuổi theo mốc quy định: lớp mẫu giáo theo năm sinh, nhóm nhà trẻ theo tháng tuổi tại ngày bắt đầu học. Hệ thống kiểm tra lớp đang hoạt động, phù hợp nhóm tuổi, còn chỗ theo chỉ tiêu vận hành và trần Điều 15 (QT-XL-01), chưa có khoảng xếp lớp trùng ngày. Hệ thống đọc phân công từ NV05 và cảnh báo nếu lớp thiếu giáo viên theo định mức (QT-PC-03).
3. Ban giám hiệu duyệt phương án và ngày hiệu lực.
4. Hệ thống tạo khoảng xếp lớp. Hệ thống đối chiếu ngày được bắt đầu học từ NV01 với ngày hiệu lực. Trẻ thỏa cả hai điều kiện có trạng thái `Đang học`. Trẻ chưa thỏa có trạng thái `Chờ đủ điều kiện bắt đầu học` ở danh sách dự kiến.

### Nhánh B · Nghỉ dài ngày, bảo lưu, thôi học và quay lại

1. Văn phòng chọn trẻ và loại biến động (`Nghỉ dài ngày`, `Bảo lưu`, `Thôi học`, `Quay lại`), nhập ngày hiệu lực, ngày kết thúc dự kiến nếu nghỉ dài ngày và lý do. Thời điểm nhập được ghi riêng (QT-XL-05).
2. Hệ thống kiểm tra theo từng loại:
   - `Nghỉ dài ngày`: có ngày bắt đầu và ngày kết thúc dự kiến. Trẻ giữ chỗ và vẫn tính vào sĩ số lớp.
   - `Bảo lưu`: trẻ rời danh sách đang học và ngừng thuộc diện điểm danh từ ngày hiệu lực, chuyển sang danh sách bảo lưu.
   - `Thôi học`: khoảng xếp lớp kết thúc tại ngày hiệu lực, lịch sử được giữ, trẻ ngừng thuộc diện điểm danh.
   - `Quay lại` sau bảo lưu: đối chiếu hồ sơ hiện hành tại NV01, kiểm tra chỉ tiêu và trần Điều 15 của lớp tiếp nhận tại ngày quay lại (QT-XL-01), cảnh báo nếu lớp thiếu giáo viên (QT-PC-03).
3. Ban giám hiệu duyệt biến động và ngày hiệu lực.
4. Hệ thống cập nhật trạng thái và khoảng xếp lớp theo quy ước ngày hiệu lực (QT-XL-02), cập nhật diện đi học cho NV03, NV06, NV08 và ghi tăng, giảm sĩ số theo lớp, khối, năm học (YC-XL-04).

## Luồng thay thế và ngoại lệ

- **Nhánh A và B (trần Điều 15):** Lớp đã đạt mức tối đa theo Điều 15 (QT-XL-01). Hệ thống chặn.
- **Nhánh A và B (trường hợp đặc biệt):** Vượt chỉ tiêu vận hành nhưng vẫn trong trần Điều 15, hoặc lệch nhóm tuổi. Hệ thống cảnh báo để Ban giám hiệu quyết định.
- **Nhánh A, bước 4 (hồ sơ còn thiếu giấy tờ của mốc đi học):** Trẻ đã xếp lớp vẫn ở danh sách dự kiến. Điểm danh NV03 và tạm thu NV06 chỉ bắt đầu từ ngày học đầu tiên thỏa cả hai điều kiện: đủ điều kiện bắt đầu học và đã đến ngày hiệu lực xếp lớp.
- **Nhánh B (ghi hồi tố):** Nhập sau ngày hiệu lực thì bắt buộc lý do và liệt kê các ngày đã khóa liên quan trước khi duyệt (QT-XL-05). Sau khi duyệt, hệ thống gửi cảnh báo đối chiếu ngày đã khóa sang [UC-14](uc-14-dieu-chinh-chuyen-can-sau-khoa.md), thông báo NV06 tính lại bù trừ hoặc tỷ lệ. Báo cáo sĩ số NV09 đã nộp của kỳ đó giữ ảnh chụp và người lập được cảnh báo. Báo cáo `Đã gửi` do Ban giám hiệu xử lý yêu cầu bổ sung nếu cần. Báo cáo `Đã duyệt` chuyển sang `Cần cập nhật` (QT-BC-04).
- **Nhánh B (thôi học không còn kỳ thu sau):** Hệ thống thông báo NV06 chuyển chênh lệch tiền ăn hoặc số dư có sang luồng điều chỉnh, hoàn tiền tại [UC-07](uc-07-ghi-thu-tien-va-in-bien-lai.md) (QT-HP-05).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã hồ sơ trẻ (NV01), lớp. Loại biến động: `Vào lớp`, `Nghỉ dài ngày`, `Bảo lưu`, `Thôi học`, `Quay lại`. Ngày hiệu lực, ngày kết thúc dự kiến nếu có, thời điểm nhập, lý do. Quyết định duyệt. | Kết quả và lịch sử xếp lớp. Danh sách lớp và diện đi học theo ngày. Danh sách dự kiến, danh sách bảo lưu. Biến động tăng, giảm. Bảng tổng hợp sĩ số đầu kỳ, tăng, giảm, cuối kỳ cho tra cứu và báo cáo NV09. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Xếp lớp trẻ mới | HS2026-042 sinh 14/05/2023, được bắt đầu học từ 07/09/2026 ([Mẫu 01](../../survey/forms/mau-01-ho-so-tre.md)) | Tính tuổi theo năm sinh, trẻ thuộc nhóm 3–4 tuổi. Xếp vào Mầm 1 từ 07/09/2026, sĩ số Mầm 1 từ 24 lên 25 ([Mẫu 05](../../survey/forms/mau-05-bao-cao-thang.md)). |
| Vượt trần Điều 15 | Xếp trẻ thứ 31 vào lớp 4–5 tuổi | Chặn, vì trần của nhóm 4–5 tuổi là 30 (QT-XL-01). |
| Kiểm tra tại ngày hiệu lực | Lớp còn chỗ hôm nay nhưng đã đủ chỉ tiêu vào ngày trẻ dự kiến vào lớp | Hệ thống tính sức chứa tại ngày hiệu lực và báo lớp đã đủ chỉ tiêu (QT-XL-01). |
| Nghỉ dài ngày | Trẻ nghỉ dài ngày từ 05/10 đến 16/10 | Trẻ vẫn tính vào sĩ số lớp. NV03 tự ghi `Vắng có phép` các ngày học trong khoảng này (QT-DD-06). NV06 thu đủ khoản tháng (QT-HP-06). |
| Bảo lưu giữa kỳ | Trẻ bắt đầu bảo lưu giữa tháng | Trẻ rời danh sách đang học từ ngày hiệu lực. NV06 tính khoản tháng theo tỷ lệ (QT-HP-06). |
| Tổng hợp sĩ số tháng 09/2026 | Đầu kỳ 133, nhập mới 3, thôi học 1 ([Mẫu 05](../../survey/forms/mau-05-bao-cao-thang.md)) | Cuối kỳ 135. Mầm 2 có 1 nhập mới, 1 thôi học, cuối kỳ vẫn 25. |
| Thôi học không còn kỳ thu | Trẻ thôi học còn số dư có 300.000 đồng | NV06 được nhắc lập phiếu hoàn tiền tại `UC-07`. |

## Liên phân hệ và truy vết

- **NV01 → NV02:** Hồ sơ đủ điều kiện tiếp nhận cùng mã và ngày được bắt đầu học từ [UC-11](uc-11-tiep-nhan-ho-so-tre.md).
- **NV02 → NV05:** Danh sách lớp đang hoạt động tại ngày hiệu lực cho [UC-05](uc-05-lap-phan-cong-giao-vien.md) (QT-PC-01).
- **NV02 → NV03:** Danh sách thuộc diện điểm danh theo ngày. Với trẻ `Nghỉ dài ngày`, NV03 tự ghi `Vắng có phép` từ đầu ngày (QT-DD-06). Trẻ `Bảo lưu`, `Thôi học` hoặc `Chờ đủ điều kiện bắt đầu học` không có trong danh sách điểm danh.
- **NV02 → NV06:** Khoảng thời gian trẻ thuộc diện đi học cùng lịch học của trường để tạm thu tiền ăn (QT-HP-02). Khoản tháng tính tỷ lệ khi vào học, thôi học, bắt đầu hoặc kết thúc bảo lưu giữa kỳ. `Nghỉ dài ngày` thu đủ khoản tháng (QT-HP-06).
- **NV02 → NV09:** Sĩ số đầu kỳ, tăng, giảm và cuối kỳ theo lớp, khối, năm học, tách nhập học, thôi học với chuyển lớp nội bộ, để Văn phòng lập báo cáo tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-01 | QT-XL-01, QT-PC-01, QT-PC-03 | NV01 → NV02 → NV05 · Nhận hồ sơ NV01, kiểm tra mốc tuổi, chỉ tiêu, trần Điều 15 và cảnh báo định mức giáo viên khi xếp lớp hoặc quay lại. |
| YC-XL-02 | QT-XL-02, QT-XL-04, QT-DD-06, QT-HP-06 | NV02 · Lập, duyệt và cập nhật vào lớp, nghỉ dài ngày, bảo lưu, thôi học, quay lại, giữ lịch sử sĩ số. |
| YC-XL-03 | QT-XL-05, QT-HP-02, QT-HP-05, QT-BC-04 | NV02 → NV03, NV06, NV09 · Danh sách theo ngày và diện đi học, kiểm soát hồi tố, chuyển xử lý thôi học sang NV06, cập nhật báo cáo. |
| YC-XL-04 | QT-XL-04 | NV02 → NV09 · Tổng hợp, tra cứu, xuất sĩ số đầu kỳ, tăng, giảm, cuối kỳ cho báo cáo Văn phòng. |
