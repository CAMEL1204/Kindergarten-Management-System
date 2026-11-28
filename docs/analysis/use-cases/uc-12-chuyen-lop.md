# UC-12 · Chuyển lớp

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV02 · F1.2.2, F1.2.4 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu (duyệt chuyển lớp) |
| Mục tiêu | Chuyển trẻ đang học sang lớp đích đúng ngày hiệu lực trong một giao dịch và giữ nguyên lịch sử sĩ số. |
| Điều kiện trước | Trẻ đã được tiếp nhận và đang thuộc một lớp nguồn có hiệu lực, ở trạng thái `Đang học` hoặc `Nghỉ dài ngày`. Trẻ không ở trạng thái `Bảo lưu` hay `Thôi học`. Danh mục năm học, lớp, nhóm tuổi, mốc tính tuổi và chỉ tiêu đã có. |
| Điều kiện sau | Từ ngày hiệu lực, trẻ chỉ thuộc lớp đích. Khoảng xếp lớp ở lớp nguồn kết thúc tại ngày hiệu lực theo QT-XL-02. Tổng sĩ số toàn trường không đổi. |

## Luồng sự kiện chính

1. Văn phòng chọn trẻ, lớp nguồn, lớp đích, ngày hiệu lực và nhập lý do.
2. Hệ thống kiểm tra tại ngày hiệu lực: trẻ đang thuộc lớp nguồn, lớp đích khác lớp nguồn và đang hoạt động trong cùng năm học, phù hợp nhóm tuổi, chưa vượt chỉ tiêu vận hành và trần Điều 15 (QT-XL-01), không trùng khoảng xếp lớp khác. Hệ thống đọc phân công của lớp đích từ NV05 và cảnh báo nếu lớp thiếu giáo viên (QT-PC-03).
3. Ban giám hiệu xem phương án, ngày hiệu lực và các cảnh báo, rồi ghi quyết định duyệt.
4. Khi xác nhận, hệ thống kiểm tra lại sức chứa lớp đích và khoảng xếp lớp. Trong một giao dịch (NFR-05), hệ thống kết thúc khoảng xếp lớp ở lớp nguồn (ngày kết thúc bằng ngày hiệu lực) và mở khoảng xếp lớp ở lớp đích (ngày bắt đầu bằng ngày hiệu lực). Hệ thống lưu riêng thời điểm nhập, người lập và người duyệt (QT-XL-03, QT-XL-05).
5. Hệ thống cập nhật danh sách lớp theo ngày cho NV03, NV05, NV08, thông báo cho giáo viên hai lớp và ghi biến động giảm, tăng nội bộ cho NV09.

## Luồng thay thế và ngoại lệ

- **Bước 2 (đầu vào không hợp lệ):** Lớp đích trùng lớp nguồn, lớp đích đã đóng, ngày hiệu lực ngoài năm học, hoặc trẻ đang `Bảo lưu` hay `Thôi học`. Hệ thống chặn và nêu lý do.
- **Bước 2 (lớp đích đã đạt trần Điều 15):** Sĩ số lớp đích tại ngày hiệu lực đã bằng mức tối đa của nhóm tuổi: 3–12 tháng là 15, 13–24 tháng là 20, 25–36 tháng là 25, 3–4 tuổi là 25, 4–5 tuổi là 30, 5–6 tuổi là 35. Hệ thống chặn, không cho trình duyệt (QT-XL-01).
- **Bước 2 (vượt chỉ tiêu vận hành hoặc lệch nhóm tuổi):** Vẫn trong trần Điều 15. Hệ thống cảnh báo trường hợp đặc biệt để Ban giám hiệu quyết định.
- **Bước 2 và 5 (ghi hồi tố):** Ngày hiệu lực trước ngày nhập. Hệ thống đánh dấu hồi tố, bắt buộc nhập lý do và liệt kê các ngày đã khóa điểm danh, suất ăn, báo cáo bị ảnh hưởng (QT-XL-05). Sau khi Ban giám hiệu duyệt:
  - Các ngày đã khóa chuyên cần bị ảnh hưởng được gắn cờ để đối chiếu tại [UC-14](uc-14-dieu-chinh-chuyen-can-sau-khoa.md).
  - NV06 được thông báo nếu lớp đích khác nhóm tuổi hoặc đơn giá cần đối soát lại (QT-HP-02).
  - Báo cáo sĩ số NV09 đã nộp của kỳ đó giữ ảnh chụp, người lập được cảnh báo. Báo cáo `Đã gửi` do Ban giám hiệu xử lý yêu cầu bổ sung nếu cần. Báo cáo `Đã duyệt` chuyển sang `Cần cập nhật` (QT-BC-04).
- **Bước 3–4 (từ chối hoặc lỗi giao dịch):** Ban giám hiệu từ chối, lớp đích vừa hết chỗ hoặc trùng khoảng xếp lớp lúc xác nhận: trẻ ở lại lớp nguồn. Nếu một đầu cập nhật thất bại, hệ thống hoàn tác cả hai đầu (QT-XL-03).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp nguồn, lớp đích, năm học, nhóm tuổi, ngày hiệu lực, thời điểm nhập, lý do. Chỉ tiêu và trần Điều 15. Quyết định duyệt. | Hai khoảng xếp lớp: kết thúc ở lớp nguồn, bắt đầu ở lớp đích cùng ngày hiệu lực. Danh sách lớp theo ngày. Biến động giảm, tăng nội bộ của hai lớp. Danh sách ngày đã khóa cần đối chiếu nếu hồi tố. Cảnh báo cho NV03, NV06, NV09. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Chuyển đúng ngày hiệu lực | Trẻ chuyển từ lớp A sang lớp B cùng nhóm tuổi, hiệu lực ngày 15 | Ngày 14 trẻ ở lớp A, ngày 15 trẻ ở lớp B. Sĩ số A giảm 1, B tăng 1, tổng 135 của trường không đổi (QT-XL-02, QT-XL-04). |
| Lớp đích đã đạt trần | Chuyển một trẻ 3–4 tuổi từ Mầm 1 sang Mầm 2 đang có 25 trẻ (README khảo sát §1) | Chặn, vì Mầm 2 đã bằng trần 25 của nhóm 3–4 tuổi (QT-XL-01). |
| Lớp đích đầy lúc xác nhận | Lúc trình còn chỗ, lúc Ban giám hiệu xác nhận lớp đích đã đầy | Không ghi. Trẻ vẫn thuộc lớp nguồn (QT-XL-03). |
| Ghi hồi tố | Ngày 18 nhập quyết định chuyển lớp hiệu lực ngày 15, ngày 15 đã khóa điểm danh | Bắt buộc lý do. Ngày 15–17 được gắn cờ đối chiếu tại `UC-14`. Báo cáo sĩ số đã duyệt của kỳ chuyển sang `Cần cập nhật` (QT-XL-05, QT-BC-04). |
| Trẻ đang bảo lưu | Văn phòng chọn chuyển lớp cho trẻ `Bảo lưu` | Chặn, báo trẻ không thuộc lớp nguồn đang học. |

## Liên phân hệ và truy vết

- **NV02 → NV05:** Trạng thái hoạt động và sĩ số của lớp theo ngày hiệu lực cho [UC-05](uc-05-lap-phan-cong-giao-vien.md). Khi chuyển lớp, NV02 đọc cảnh báo thiếu định mức giáo viên của lớp đích (QT-PC-03).
- **NV02 → NV03, NV08:** Lớp có hiệu lực theo từng ngày học. Chuyển lớp hiệu lực ngày 15 thì ngày 14 trẻ ở lớp nguồn, ngày 15 ở lớp đích.
- **NV02 → NV06:** Chuyển lớp nội bộ không đổi diện đi học. NV06 được thông báo khi chuyển lớp hồi tố hoặc đổi nhóm tuổi làm đổi biểu phí áp dụng (QT-HP-02).
- **NV02 → NV09:** Biến động chuyển lớp nội bộ giải thích sĩ số đầu kỳ, cuối kỳ từng lớp mà không bị tính là nhập học hay thôi học. Ghi hồi tố kích hoạt `Cần cập nhật` (QT-BC-04).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-02 | QT-XL-01, QT-XL-02, QT-XL-03, QT-XL-04, NFR-05 | NV02 · Kiểm tra trần Điều 15 và chỉ tiêu, ghi hai đầu chuyển lớp trong một giao dịch, giữ lịch sử và sĩ số toàn trường. |
| YC-XL-03 | QT-XL-05, QT-HP-02, QT-PC-03, QT-BC-04 | NV02 → NV03, NV05, NV06, NV08, NV09 · Tra cứu lớp theo ngày, cảnh báo thiếu giáo viên ở lớp đích, xử lý hậu kiểm khi ghi hồi tố. |
