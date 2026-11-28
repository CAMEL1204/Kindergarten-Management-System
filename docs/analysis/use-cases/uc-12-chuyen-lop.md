# UC-12 · Chuyển lớp

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV02 · F1.2.2, F1.2.4 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu |
| Mục tiêu | Chuyển trẻ đang học sang lớp đích đúng ngày hiệu lực trong cùng một giao dịch nguyên tử và bảo toàn lịch sử sĩ số. |
| Điều kiện trước | Trẻ có hồ sơ đã tiếp nhận và đang thuộc một lớp nguồn có hiệu lực (trạng thái `Đang học` hoặc `Nghỉ dài ngày`, không ở trạng thái `Bảo lưu` hoặc `Thôi học`). Có danh mục năm học, lớp, nhóm tuổi, mốc tính tuổi và chỉ tiêu đã thiết lập. |
| Điều kiện sau | Từ ngày hiệu lực, trẻ thuộc duy nhất lớp đích; khoảng xếp lớp nguồn được đóng tại cùng ngày hiệu lực theo quy ước `QT-XL-02`. Tổng sĩ số toàn trường không đổi. |

## Luồng sự kiện chính

1. Văn phòng chọn trẻ, lớp nguồn hiện hành, lớp đích, ngày hiệu lực chuyển lớp và nhập lý do.
2. Hệ thống kiểm tra tại ngày hiệu lực: trẻ đang thuộc lớp nguồn, lớp đích khác lớp nguồn và đang hoạt động trong cùng năm học, phù hợp nhóm tuổi, chưa vượt chỉ tiêu vận hành và giới hạn tối đa theo Điều 15 Điều lệ Trường mầm non (`QT-XL-01`), không trùng khoảng xếp lớp khác, đồng thời tra cứu trạng thái phân công giáo viên của lớp đích từ NV05 để cảnh báo nếu lớp thiếu giáo viên (`QT-PC-03`).
3. Ban giám hiệu xem phương án chuyển lớp, ngày hiệu lực và các cảnh báo (nếu có) rồi ghi quyết định duyệt.
4. Khi xác nhận duyệt, hệ thống kiểm tra lại sức chứa lớp đích và khoảng xếp lớp. Trong **cùng một giao dịch nguyên tử (`NFR-05`)**, hệ thống đóng khoảng xếp lớp tại lớp nguồn (ngày kết thúc = ngày hiệu lực) và mở khoảng xếp lớp tại lớp đích (ngày bắt đầu = ngày hiệu lực); lưu riêng thời điểm nhập, người lập và người duyệt (`QT-XL-03`, `QT-XL-05`).
5. Cập nhật danh sách lớp theo ngày cho NV03, NV05, NV08, thông báo cho giáo viên hai lớp và ghi nhận biến động giảm/tăng nội bộ phục vụ đối chiếu sĩ số tại NV09.

## Luồng thay thế và ngoại lệ

- **Bước 2 (dữ liệu đầu vào không hợp lệ):** Nếu lớp đích trùng lớp nguồn, lớp đích đã đóng, ngày hiệu lực nằm ngoài năm học hoặc trẻ đang ở trạng thái `Bảo lưu`/`Thôi học`, hệ thống chặn thao tác và nêu rõ lý do.
- **Bước 2 (chỉ tiêu và nhóm tuổi theo `QT-XL-01`):**
  - Nếu tổng sĩ số lớp đích tại ngày hiệu lực đạt **mức tối đa theo Điều 15 Điều lệ Trường mầm non** (3–12 tháng: 15; 13–24 tháng: 20; 25–36 tháng: 25; 3–4 tuổi: 25; 4–5 tuổi: 30; 5–6 tuổi: 35), hệ thống **chặn cứng**, không cho trình duyệt.
  - Nếu chỉ vượt chỉ tiêu vận hành của trường (vẫn trong trần Điều 15) hoặc trẻ lệch nhóm tuổi, hệ thống hiển thị cảnh báo trường hợp đặc biệt để Ban giám hiệu xem xét quyết định theo thẩm quyền.
- **Bước 2 & Bước 5 (ghi hồi tố `QT-XL-05`):** Nếu `ngày hiệu lực < ngày nhập hiện tại`, hệ thống đánh dấu ghi hồi tố, bắt buộc nhập lý do, liệt kê các ngày đã khóa điểm danh/suất ăn/báo cáo chịu ảnh hưởng trước khi trình duyệt. Sau khi Ban giám hiệu duyệt hồi tố:
  - Gắn cờ các ngày đã khóa chuyên cần bị ảnh hưởng để giáo viên/người có quyền đối chiếu qua [UC-14](uc-14-dieu-chinh-chuyen-can-sau-khoa.md) (`NV03`);
  - Thông báo cho `NV06` nếu lớp đích khác nhóm tuổi/đơn giá biểu phí cần đối soát lại (`QT-HP-02`);
  - Với báo cáo sĩ số `NV09` đã nộp của kỳ tương ứng: giữ nguyên ảnh chụp và cảnh báo người lập (`Đã gửi` để Ban giám hiệu xử lý yêu cầu bổ sung nếu cần; `Đã duyệt` chuyển sang `Cần cập nhật` theo `QT-BC-04`).
- **Bước 3–4 (từ chối hoặc lỗi giao dịch):** Nếu Ban giám hiệu từ chối, lớp đích vừa đủ chỗ hoặc trùng khoảng xếp lớp tại thời điểm xác nhận, giữ nguyên lớp nguồn. Nếu một đầu cập nhật thất bại, hệ thống hoàn tác toàn bộ cả hai đầu (`QT-XL-03`).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp nguồn, lớp đích, năm học, nhóm tuổi, ngày hiệu lực, thời điểm nhập, lý do, chỉ tiêu/trần Điều 15, quyết định duyệt. | Hai khoảng xếp lớp (đóng lớp nguồn và mở lớp đích cùng ngày hiệu lực), danh sách lớp theo ngày, biến động giảm/tăng nội bộ hai lớp, danh sách ngày đã khóa cần đối chiếu nếu hồi tố, cảnh báo tới NV03/NV06/NV09. |

## Liên phân hệ và truy vết

- **NV02 → NV05:** Cung cấp trạng thái hoạt động và sĩ số thực tế của lớp theo ngày hiệu lực cho NV05 lập phân công; đọc cảnh báo thiếu định mức giáo viên của lớp đích khi chuyển lớp.
- **NV02 → NV03, NV08:** Cung cấp lớp có hiệu lực theo từng ngày học (ví dụ chuyển hiệu lực ngày 15 thì ngày 14 trẻ ở lớp nguồn, ngày 15 trẻ ở lớp đích).
- **NV02 → NV06:** Giữ nguyên diện đi học của trẻ (do chuyển lớp nội bộ không làm trẻ thôi học), đồng thời thông báo nếu chuyển lớp hồi tố hoặc đổi nhóm tuổi làm thay đổi biểu phí áp dụng (`QT-HP-02`).
- **NV02 → NV09:** Cung cấp biến động chuyển lớp nội bộ để giải thích sĩ số đầu kỳ/cuối kỳ từng lớp mà không cộng nhầm thành nhập học/thôi học toàn trường, và kích hoạt `Cần cập nhật` khi ghi hồi tố (`QT-BC-04`).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-02 | QT-XL-01, QT-XL-02, QT-XL-03, QT-XL-04, NFR-05 | Kiểm tra trần Điều 15/chỉ tiêu, ghi nguyên tử hai đầu chuyển lớp, bảo toàn lịch sử và sĩ số toàn trường. |
| YC-XL-03 | QT-XL-05, QT-HP-02, QT-PC-03, QT-BC-04 | NV02 → NV03, NV05, NV06, NV08, NV09 · Tra cứu lớp theo ngày, cảnh báo thiếu giáo viên lớp đích và xử lý hậu kiểm khi ghi hồi tố. |
