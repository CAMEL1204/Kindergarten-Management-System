# UC-11 · Tiếp nhận hồ sơ trẻ

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV01 · F1.1 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu |
| Mục tiêu | Duyệt một đợt tiếp nhận và bàn giao đúng hồ sơ cho người xếp lớp. |
| Điều kiện trước | Văn phòng nhận đăng ký và có quyền lập hồ sơ. Ban giám hiệu có quyền duyệt tiếp nhận. |
| Điều kiện sau | Hồ sơ đã duyệt có ngày dự kiến học xuất hiện trong danh sách chờ xếp lớp. Hẹn bổ sung giấy tờ được theo dõi riêng. |

## Luồng sự kiện chính

1. Ghi kênh đăng ký, đối chiếu phiếu đăng ký và bản sao giấy khai sinh.
2. Tra mã trẻ, số định danh nếu có, họ tên, ngày sinh và thông tin giám hộ. Xác lập mã hồ sơ sau khi xử lý kết quả đối chiếu.
3. Ghi người giám hộ chính và danh mục giấy tờ theo mốc. Kiểm tra thỏa thuận nhập học đã ký trước khi trình duyệt.
4. Ban giám hiệu kiểm tra điều kiện tiếp nhận, chỗ trống của nhóm tuổi và ghi quyết định.
5. Văn phòng thông báo kết quả, ghi ngày dự kiến học và hẹn nộp sổ tiêm chủng, phiếu sức khỏe, danh sách người đưa đón trước ngày đi học. Hệ thống bàn giao cùng mã hồ sơ sang NV02.

## Luồng thay thế và ngoại lệ

- Bước 2, nếu xác nhận trẻ quay lại, mở đợt tiếp nhận trên cùng mã trẻ. Nếu chỉ trùng tên và ngày sinh, đối chiếu thêm giấy tờ trước khi quyết định.
- Bước 4, thiếu giấy tờ của mốc duyệt thì lập hẹn bổ sung và dừng trình duyệt. Nếu bị từ chối, lưu quyết định và kết thúc đợt xử lý.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Kênh, ngày đăng ký, họ tên, ngày sinh, định danh, thông tin giám hộ, giấy tờ từng mốc, ngày dự kiến học. | Mã trẻ, đợt tiếp nhận, kết quả, người và ngày duyệt, tình trạng hoàn thiện, lịch hẹn, danh sách chờ. |

## Liên phân hệ và truy vết

NV01 → NV02 dùng hồ sơ đã duyệt. NV08 tra người giám hộ đã xác nhận hoặc ủy quyền cố định còn hiệu lực khi trả trẻ. Văn phòng ghi họ tên, quan hệ, điện thoại, CCCD và hai ngày hiệu lực của ủy quyền. Tính đến hết ngày cuối, trừ trường hợp thu hồi sớm. Che CCCD trên danh sách thông thường.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HS-01, YC-HS-02 | QT-HS-01, QT-HS-02 | Đối chiếu trẻ, giấy tờ và từng mốc tiếp nhận. |
| YC-HS-03 | QT-HS-03 | NV01 → NV02 · Bàn giao hồ sơ đủ điều kiện. |
| YC-HS-04, YC-HS-05 | QT-HS-04, QT-HS-05, QT-DT-04 | NV01 → NV08 · Tra quyền nhận trẻ theo hiệu lực. |
