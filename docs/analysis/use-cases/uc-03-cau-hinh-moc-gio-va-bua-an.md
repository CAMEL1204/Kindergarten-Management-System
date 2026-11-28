# UC-03 · Cấu hình khung giờ vận hành, bữa ăn và mốc chốt

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | F1.0 · F1.0.3 |
| Tác nhân chính | Ban giám hiệu |
| Tác nhân phụ | Không có. NV03, NV07 và NV08 áp dụng cấu hình. |
| Mục tiêu | Cấu hình danh mục bữa ăn, định mức theo bữa, khung đón, khung trả, mốc bàn giao buổi sáng, mốc khóa chuyên cần và giờ chốt từng bữa mà không cần sửa mã nguồn (YC-SA-03, YC-DT-04, NFR-08). |
| Điều kiện trước | Người thực hiện có quyền quản trị F1.0 của Ban giám hiệu. Đã có năm học hoặc ngày dự kiến áp dụng cấu hình. |
| Điều kiện sau | Cấu hình có ngày hiệu lực và lịch sử thay đổi (NFR-04). NV03, NV07, NV08 dùng cấu hình này từ ngày hiệu lực. |

## Luồng sự kiện chính

1. Ban giám hiệu mở chức năng cấu hình mốc giờ và bữa ăn của trường (NFR-08).
2. Ban giám hiệu nhập các mốc giao nhận và chuyên cần (YC-DT-04, YC-DD-02):
   - Khung đón chính, mặc định 07:00 đến trước 08:30.
   - Mốc bàn giao buổi sáng, cũng là mốc đánh dấu `Đến muộn`, mặc định 08:30.
   - Mốc khóa chuyên cần hằng ngày, mặc định 09:00.
   - Khung trả thường lệ, mặc định 16:00–17:30.
3. Ban giám hiệu nhập danh mục bữa ăn (`Sáng`, `Trưa`, `Xế`), nguồn lấy danh sách và giờ chốt từng bữa (YC-SA-03). Bữa sáng mặc định chốt lúc 07:30, lấy trẻ có mặt đến hết 07:30. Bữa trưa và bữa xế mặc định dùng bản bàn giao 08:30. Ban giám hiệu nhập định mức khẩu phần chuẩn của từng bữa. Tiền ăn không tách theo bữa (QT-HP-02).
4. Ban giám hiệu chọn ngày bắt đầu hiệu lực và lưu. Hệ thống kiểm tra thứ tự các mốc trong ngày, lưu lịch sử và áp dụng cho NV03, NV07, NV08 từ ngày hiệu lực.

## Luồng thay thế và ngoại lệ

- **Bước 4 (sai thứ tự mốc):** Giờ chốt bữa sáng sau mốc bàn giao, mốc bàn giao sau mốc khóa, hoặc giờ bắt đầu khung đón, khung trả không trước giờ kết thúc. Hệ thống chặn lưu và báo lỗi (NFR-06).
- **Bước 4 (đổi cấu hình trong ngày đang vận hành):** Nếu trong ngày đã có lượt đón, lượt trả (NV08), đã chốt suất (NV07) hoặc đã khóa chuyên cần (NV03), hệ thống không áp dụng cho ngày đó. Ngày hiệu lực sớm nhất là ngày học kế tiếp. Dữ liệu cũ giữ cấu hình cũ.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Ngày bắt đầu hiệu lực. Khung đón chính, mốc bàn giao buổi sáng, mốc khóa chuyên cần, khung trả thường lệ. Danh mục bữa ăn, định mức khẩu phần, giờ chốt và nguồn chốt từng bữa. Người cấu hình. | Bộ tham số mốc giờ và bữa ăn theo ngày hiệu lực. Lịch sử phiên bản cấu hình cho NV03, NV07, NV08. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Cấu hình mặc định | Chốt sáng 07:30, bàn giao 08:30, khóa 09:00, khung đón 07:00–08:30, khung trả 16:00–17:30 | Lưu thành công. Các mốc áp dụng cho NV03, NV07, NV08 từ ngày hiệu lực. |
| Sai thứ tự mốc | Mốc bàn giao 09:10, mốc khóa 09:00 | Chặn lưu, báo mốc bàn giao phải trước mốc khóa (NFR-06). |
| Đổi trong ngày đang vận hành | Ngày 14/10/2026 đã có lượt đón, Ban giám hiệu đổi mốc khóa sang 09:15 | Không áp dụng cho 14/10. Ngày hiệu lực sớm nhất là 15/10/2026. |
| Trẻ đến đúng giờ chốt sáng | HS2026-058 đến 07:30, HS2026-001 đến 07:45 ([Mẫu 03](../../survey/forms/mau-03-bao-suat-va-giao-nhan.md)) | HS2026-058 có trong danh sách ăn sáng. HS2026-001 không có. Cả hai có trong bản bàn giao 08:30. |

## Liên phân hệ và truy vết

- **F1.0 → NV08 ([UC-08](uc-08-don-tre-theo-nhom.md), [UC-09](uc-09-tra-tre-theo-nhom.md)):** Khung đón, mốc `Đến muộn`, mốc chuyển sang đề nghị điều chỉnh và khung trả để cảnh báo trẻ chưa trả (YC-DT-04, QT-DT-06).
- **F1.0 → NV03 ([UC-13](uc-13-chot-chuyen-can.md), [UC-14](uc-14-dieu-chinh-chuyen-can-sau-khoa.md)):** Mốc chốt bữa sáng 07:30, mốc bàn giao 08:30 và mốc khóa 09:00 (YC-DD-02, QT-DD-03, QT-DD-07).
- **F1.0 → NV07 ([UC-15](uc-15-chot-va-doi-chieu-suat-an.md), [UC-18](uc-18-lap-duyet-va-thong-bao-thuc-don.md)):** Danh mục bữa ăn và giờ chốt từng bữa để giáo viên chốt suất và điều chỉnh sau chốt (YC-SA-03, QT-SA-04).

| Yêu cầu / NFR | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-03, YC-DT-04, YC-DD-02, NFR-04, NFR-06, NFR-08 | QT-DD-03, QT-DD-07, QT-SA-04, QT-DT-06, QT-HP-02 | F1.0 → NV03, NV07, NV08 · Cấu hình bữa ăn, định mức, giờ chốt bữa và các mốc giờ vận hành dùng chung. |
