# UC-12 · Chuyển lớp

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV02 · F1.2 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu |
| Mục tiêu | Chuyển trẻ sang lớp đích đúng ngày và bảo toàn lịch sử sĩ số. |
| Điều kiện trước | Trẻ có hồ sơ được tiếp nhận và có lớp nguồn. Có danh mục lớp, mốc tính tuổi, chỉ tiêu và năm học. |
| Điều kiện sau | Đến ngày hiệu lực, trẻ thuộc duy nhất lớp đích. Sĩ số toàn trường giữ nguyên. |

## Luồng sự kiện chính

1. Văn phòng chọn trẻ, lớp nguồn, lớp đích, ngày hiệu lực và nhập lý do.
2. Hệ thống kiểm tra lớp nguồn của trẻ, tình trạng hoạt động của lớp đích, nhóm tuổi, chỉ tiêu dự kiến và các khoảng xếp lớp trùng nhau tại ngày hiệu lực.
3. Ban giám hiệu xem phương án, ngày hiệu lực và quyết định duyệt.
4. Khi xác nhận, kiểm tra lại sức chứa và lịch xếp lớp. Trong cùng giao dịch, đóng lớp nguồn và mở lớp đích tại cùng ngày; lưu thời điểm nhập, người lập và người duyệt.
5. Cập nhật danh sách theo ngày, thông báo cho hai lớp và cung cấp diện đi học cho các bộ phận liên quan.

## Luồng thay thế và ngoại lệ

- Bước 2, nếu nhập hồi tố, liệt kê các ngày đã khóa, yêu cầu lý do và đối chiếu dữ liệu chịu ảnh hưởng trước khi duyệt.
- Bước 3–4, nếu bị từ chối, lớp đích đầy hoặc trùng khoảng, giữ nguyên lớp nguồn. Nếu một đầu cập nhật lỗi, hoàn tác cả hai đầu.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp nguồn, lớp đích, năm học, nhóm tuổi, ngày chuyển, lý do, chỉ tiêu và quyết định duyệt. | Hai khoảng xếp lớp, danh sách trước và sau ngày chuyển, biến động giảm/tăng tại hai lớp, các ngày cần đối chiếu. |

## Liên phân hệ và truy vết

NV02 → NV05 cung cấp lớp đang hoạt động tại ngày phân công. NV03 và NV08 dùng lớp có hiệu lực theo ngày. NV06 dùng diện đi học cùng lịch trường để tạm thu. NV09 nhận biến động để giải thích sĩ số đầu kỳ và cuối kỳ.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-01, YC-XL-02 | QT-XL-01, QT-XL-02, QT-XL-03, QT-XL-04, QT-PC-01 | NV02 → NV05 · Kiểm tra lớp hoạt động, ghi hai đầu chuyển lớp. |
| YC-XL-03 | QT-XL-05, QT-HP-02 | NV02 → NV03, NV06 · Diện đi học theo ngày và đối chiếu hồi tố. |
