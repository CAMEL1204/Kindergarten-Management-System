# UC-02 · Quản lý danh mục dùng chung

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | F1.0 · F1.0.2 |
| Tác nhân chính | Ban giám hiệu |
| Tác nhân phụ | Văn phòng (lập/cập nhật danh mục lớp NV02), Kế toán (lập/cập nhật biểu phí và danh mục miễn giảm NV06) |
| Mục tiêu | Thiết lập, phê duyệt và duy trì thống nhất các danh mục nền tảng dùng chung cho toàn trường mà không trùng lặp hoặc sửa trực tiếp dữ liệu đã phát sinh giao dịch. |
| Điều kiện trước | Người dùng có tài khoản thuộc hệ thống con tương ứng (Ban giám hiệu, Văn phòng, Kế toán). Các quy định nền của trường (Điều 15 Điều lệ Trường mầm non, kế hoạch năm học) đã được xác định. |
| Điều kiện sau | Các danh mục `Đã duyệt` / `Đang áp dụng` được cung cấp chỉ đọc làm chuẩn cho toàn bộ các phân hệ nghiệp vụ `NV01`–`NV09` (`NFR-08`). |

## Luồng sự kiện chính

1. Ban giám hiệu thiết lập và ban hành các danh mục dùng chung thuộc quyền quản trị: `năm học`, `kỳ học`, `lịch nghỉ của trường`, `nhóm tuổi` (kèm sĩ số tối đa theo Điều 15), `ca làm việc` (khung giờ), `định mức giáo viên` theo lớp/nhóm tuổi và `danh mục loại báo cáo` (`YC-BC-05`: bộ phận lập, kỳ, hạn gửi, chỉ tiêu).
2. Văn phòng lập hoặc cập nhật `danh mục lớp` (mã lớp, tên lớp, năm học, nhóm tuổi, mốc tính tuổi, chỉ tiêu vận hành, tình trạng hoạt động) và trình Ban giám hiệu duyệt (`YC-XL-01`, `QT-XL-01`).
3. Kế toán lập hoặc đề xuất ban hành mới `biểu phí` (khoản thu, đơn giá, đơn vị tính, nhóm tuổi/lớp áp dụng, ngày hiệu lực) và `danh mục miễn giảm` rồi trình Ban giám hiệu duyệt (`YC-HP-01`, `QT-HP-01`).
4. Ban giám hiệu kiểm tra tính hợp lệ, ghi quyết định duyệt và phát hành danh mục áp dụng chung cho các phân hệ liên quan.

## Luồng thay thế và ngoại lệ

- **Bước 2 (chỉ tiêu lớp vượt trần Điều 15):** Nếu Văn phòng nhập chỉ tiêu lớp vượt mức tối đa theo nhóm tuổi tại Điều 15 Điều lệ Trường mầm non (`QT-XL-01`), hệ thống chặn lưu và yêu cầu điều chỉnh.
- **Bước 3 (sửa biểu phí đã sử dụng):** Nếu biểu phí đã được dùng để lập bảng phải thu tại `NV06`, hệ thống chặn sửa trực tiếp bản ghi cũ và yêu cầu ban hành biểu phí mới kèm ngày hiệu lực mới (`QT-HP-01`).
- **Bước 1–4 (ngừng sử dụng danh mục đã phát sinh dữ liệu):** Không xóa cứng lớp, năm học, nhóm tuổi hay loại báo cáo đã có dữ liệu lịch sử (`NFR-04`); chỉ cho phép chuyển trạng thái `Ngừng hoạt động` / `Hết hiệu lực`. Nếu thay đổi lịch nghỉ trường ảnh hưởng ngày đã khóa chuyên cần hoặc đã tạm thu, hệ thống cảnh báo để đối chiếu.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Năm học, kỳ học, lịch nghỉ trường, nhóm tuổi (trần Điều 15), ca làm việc, định mức giáo viên (`NV05`), danh mục lớp (`NV02`), biểu phí và danh mục miễn giảm (`NV06`), danh mục loại báo cáo (`NV09`), quyết định duyệt. | Bộ danh mục dùng chung chuẩn hóa ở trạng thái `Hoạt động` / `Ngừng hoạt động`, lịch sử phiên bản biểu phí và thông tin duyệt. |

## Liên phân hệ và truy vết

- **F1.0 → NV02, NV03, NV05, NV06:** Cung cấp năm học, kỳ học, lịch học/nghỉ thực tế, nhóm tuổi, lớp, ca làm việc và định mức giáo viên (`QT-XL-01`, `QT-PC-03`, `QT-DD-01`, `QT-HP-02`).
- **F1.0 → NV06:** Phê duyệt biểu phí và danh mục miễn giảm làm căn cứ lập bảng phải thu (`YC-HP-01`, `QT-HP-01`, `QT-HP-03`).
- **F1.0 → NV09:** Cung cấp danh mục loại báo cáo, kỳ và hạn gửi cho `UC-10` (`YC-BC-05`, `QT-BC-06`).

| Yêu cầu / NFR | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-01, YC-HP-01, YC-BC-05, NFR-04, NFR-08 | QT-XL-01, QT-HP-01, QT-HP-02, QT-HP-03, QT-PC-03, QT-DD-01, QT-BC-06 | Thiết lập, duyệt và bảo toàn lịch sử các danh mục dùng chung cho toàn trường. |
