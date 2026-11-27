# UC-13 · Chốt chuyên cần theo lớp và ngày

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV03 · F1.3 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Không có tác nhân phụ trực tiếp trong luồng chốt. |
| Mục tiêu | Khóa một kết quả cho từng trẻ thuộc diện điểm danh và bàn giao đúng số liệu tại từng mốc. |
| Điều kiện trước | Có lịch học, lớp có hiệu lực và phân công giáo viên trong ngày. Các mốc bàn giao, khóa đã được cấu hình. |
| Điều kiện sau | Mỗi trẻ có một kết quả theo ngày. Bản bàn giao cho bếp được lưu riêng với kết quả đã khóa. |

## Luồng sự kiện chính

1. Giáo viên mở bảng theo lớp và ngày. Hệ thống kiểm tra quyền, lịch và danh sách NV02, đọc lại dòng đã có để tránh ghi trùng.
2. Tiếp nhận lượt đón hợp lệ NV08 làm căn cứ duy nhất ghi Có mặt. Gắn cờ đến muộn từ 08:30 theo giờ đến thực tế. Lượt trả chỉ cập nhật giờ về.
3. Với trẻ chưa được đón, giáo viên đối chiếu báo nghỉ và xác minh để kết luận Vắng có phép hoặc Vắng không phép. Giữ Chờ xác minh nếu thiếu căn cứ.
4. Hệ thống cung cấp danh sách có mặt tính đến hết 07:30 cho bữa sáng, lưu bản bàn giao 08:30 cho bữa trưa và xế. Giáo viên tiếp tục xác minh đến giờ khóa.
5. Tại 09:00 mặc định, khóa kết quả từng trẻ, kể cả Chờ xác minh. Tổng hợp theo mã trẻ và ngày học để tránh đếm trùng khi chuyển lớp.

## Luồng thay thế và ngoại lệ

- Bước 3, với ngày học thuộc kỳ nghỉ dài ngày đã xác nhận, khởi tạo Vắng có phép nếu chưa có lượt đón. Lượt đón trước khóa vẫn cập nhật Có mặt và giữ lịch sử.
- Bước 2, hủy lượt đón ghi nhầm trước khóa thì tính lại kết quả. Từ đúng giờ khóa, phát sinh hoặc hủy lượt đón chuyển sang UC-14.
- Bước 1, ngày nghỉ hoặc giáo viên ngoài phân công thì dừng thao tác.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Lớp, ngày, lịch trường, khoảng xếp lớp và nghỉ dài ngày, phân công, báo nghỉ, mã lượt đón/trả, giờ thực tế, căn cứ xác minh. | Trạng thái theo trẻ/ngày, cờ đến muộn, số có mặt từng mốc, ngày khóa, tỷ lệ chuyên cần theo kỳ (lượt có mặt / lượt phải điểm danh × 100%). Mẫu số bằng 0 hiển thị không áp dụng. Báo riêng lượt chờ xác minh. |

## Liên phân hệ và truy vết

NV05 xác định quyền trên lớp. NV08 → NV03 cung cấp giao nhận và báo nghỉ. NV03 → NV07 bàn giao danh sách theo mốc. Sau mốc khóa ngày học cuối kỳ, NV06 đọc số ngày có mặt đã xác nhận để đối soát. NV09 nhận số liệu chuyên cần theo kỳ.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DD-01, YC-DD-05 | QT-DD-01, QT-DD-06 | NV02, NV05 → NV03 · Lớp, quyền, nghỉ dài ngày. |
| YC-DD-02 | QT-DD-02, QT-DD-05, QT-DT-01, QT-DT-05, QT-DT-07 | NV08 → NV03 → NV07 · Giao nhận, xác minh và bàn giao theo mốc. |
| YC-DD-04, YC-DD-05 | QT-DD-04, QT-DD-07, QT-HP-05 | NV03 → NV06, NV09 · Khóa kỳ và tổng hợp. |
