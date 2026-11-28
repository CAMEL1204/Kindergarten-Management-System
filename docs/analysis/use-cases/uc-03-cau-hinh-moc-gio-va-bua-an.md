# UC-03 · Cấu hình khung giờ vận hành, bữa ăn và mốc chốt

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | F1.0 · F1.0.3 |
| Tác nhân chính | Ban giám hiệu |
| Tác nhân phụ | Không có tác nhân phụ trực tiếp (NV03, NV07 và NV08 áp dụng cấu hình) |
| Mục tiêu | Cấu hình động danh mục bữa ăn, định mức áp dụng theo bữa, các khung giờ giao nhận trẻ, mốc bàn giao buổi sáng, mốc khóa chuyên cần và mốc chốt suất ăn mà không cần sửa mã nguồn (`YC-SA-03`, `YC-DT-04`, `NFR-08`). |
| Điều kiện trước | Người thực hiện có quyền quản trị hệ thống `F1.0` của Ban giám hiệu. Đã có năm học hoặc ngày dự kiến áp dụng cấu hình. |
| Điều kiện sau | Cấu hình khung giờ, bữa ăn và các mốc chốt có ngày hiệu lực được lưu vết (`NFR-04`) và làm chuẩn vận hành cho `NV03`, `NV07`, `NV08` từ ngày hiệu lực. |

## Luồng sự kiện chính

1. Ban giám hiệu mở chức năng cấu hình mốc giờ vận hành và bữa ăn theo trường (`NFR-08`).
2. Ban giám hiệu thiết lập hoặc điều chỉnh các mốc giao nhận và chuyên cần (`YC-DT-04`, `YC-DD-02`):
   - Khung đón chính (mặc định `07:00` – trước `08:30`);
   - Mốc bàn giao buổi sáng kiêm mốc đánh dấu `Đến muộn` (mặc định `08:30`);
   - Mốc khóa chuyên cần hằng ngày (mặc định `09:00`);
   - Khung trả thường lệ (mặc định `16:00` – `17:30`).
3. Ban giám hiệu thiết lập danh mục bữa ăn áp dụng (`Sáng`, `Trưa`, `Xế`), quy ước nguồn lấy danh sách và **giờ chốt từng bữa** (`YC-SA-03`): bữa sáng (mặc định `07:30`, lấy trẻ có mặt đến hết `07:30`), bữa trưa và bữa xế (mặc định theo bản bàn giao buổi sáng `08:30`), cùng định mức khẩu phần/bữa chuẩn của trường (không tách tiền ăn theo bữa theo kịch bản `QT-HP-02`).
4. Ban giám hiệu chọn ngày bắt đầu hiệu lực và xác nhận lưu; hệ thống kiểm tra tính nhất quán thứ tự thời gian giữa các mốc trong ngày, lưu lịch sử thay đổi và áp dụng cấu hình mới cho `NV03`, `NV07`, `NV08` từ ngày hiệu lực.

## Luồng thay thế và ngoại lệ

- **Bước 4 (mâu thuẫn thứ tự mốc giờ):** Nếu giờ chốt bữa sáng `> mốc bàn giao buổi sáng`, mốc bàn giao `> mốc khóa chuyên cần`, hoặc giờ bắt đầu khung đón/trả `>=` giờ kết thúc tương ứng, hệ thống chặn lưu và thông báo lỗi kiểm tra dữ liệu (`NFR-06`).
- **Bước 4 (đổi cấu hình trong ngày đang vận hành):** Nếu ngày hiện tại đã phát sinh lượt đón/trả (`NV08`), đã chốt suất ăn (`NV07`) hoặc đã chạy khóa chuyên cần (`NV03`), hệ thống không áp dụng hồi tố lên ngày hiện tại mà yêu cầu chọn ngày hiệu lực từ ngày học kế tiếp trở đi, giữ nguyên cấu hình cũ cho dữ liệu lịch sử.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Ngày bắt đầu hiệu lực, khung đón chính, mốc bàn giao sáng / đến muộn, mốc khóa chuyên cần, khung trả thường lệ, danh mục bữa ăn (`Sáng`/`Trưa`/`Xế`), định mức khẩu phần/bữa chuẩn, mốc chốt từng bữa, quy ước nguồn chốt bữa, người cấu hình. | Bộ tham số cấu hình mốc giờ và bữa ăn theo ngày hiệu lực, lịch sử phiên bản cấu hình phục vụ `NV03`, `NV07`, `NV08`. |

## Liên phân hệ và truy vết

- **F1.0 → NV08 (`UC-08`, `UC-09`):** Cung cấp khung đón, mốc `Đến muộn`, mốc chuyển đề nghị sau khóa và khung trả cảnh báo trẻ chưa trả cuối giờ (`YC-DT-04`, `QT-DT-06`).
- **F1.0 → NV03 (`UC-13`, `UC-14`):** Cung cấp mốc chốt bữa sáng `07:30`, mốc lưu bản bàn giao `08:30` và mốc tự động khóa chuyên cần `09:00` (`YC-DD-02`, `QT-DD-03`, `QT-DD-07`).
- **F1.0 → NV07 (`UC-15`, `UC-18`):** Cung cấp danh mục bữa ăn và giờ chốt theo bữa để giáo viên chốt suất và xử lý điều chỉnh sau chốt (`YC-SA-03`, `QT-SA-04`).

| Yêu cầu / NFR | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-SA-03, YC-DT-04, YC-DD-02, NFR-04, NFR-06, NFR-08 | QT-DD-03, QT-DD-07, QT-SA-04, QT-DT-06, QT-HP-02 | Cấu hình danh mục bữa ăn, định mức/quy ước chốt bữa và các mốc giờ vận hành dùng chung cho `NV03`, `NV07`, `NV08`. |
