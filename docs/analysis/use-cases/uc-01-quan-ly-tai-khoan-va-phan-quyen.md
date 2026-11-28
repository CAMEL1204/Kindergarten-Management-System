# UC-01 · Quản lý tài khoản và phân quyền trực

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | F1.0 · F1.0.1 |
| Tác nhân chính | Ban giám hiệu |
| Tác nhân phụ | Văn phòng (cung cấp biến động nhân sự NV04) |
| Mục tiêu | Cấp phát, phân quyền và kiểm soát vòng đời tài khoản gắn với hồ sơ nhân sự, bảo đảm phân tách vai trò hệ thống con, quyền trực giao nhận và quyền duyệt ngoại lệ `NV08`. |
| Điều kiện trước | Nhân sự đã có hồ sơ hợp lệ được xác nhận tại `NV04` (trạng thái làm việc hiện hành). Người thực hiện có quyền quản trị hệ thống `F1.0`. |
| Điều kiện sau | Tài khoản được tạo/cập nhật vai trò đúng phạm vi hệ thống con (`NFR-01`, `NFR-02`), quyền trực/duyệt ngoại lệ `NV08` được ghi nhận theo lớp và khoảng thời gian, hoặc tài khoản bị khóa/mở lại đúng ngày hiệu lực biến động nhân sự (`YC-NS-05`, `QT-NS-03`). |

## Luồng sự kiện chính

1. Ban giám hiệu chọn hồ sơ nhân sự từ `NV04` để tạo mới tài khoản (mật khẩu lưu dạng băm theo `NFR-02`) hoặc điều chỉnh vai trò hệ thống con theo vị trí công việc (một người kiêm nhiều vị trí được cấp nhiều hệ thống con mà không gộp chức năng).
2. Khi phát sinh nhu cầu trực cổng/trực lớp hoặc ủy quyền duyệt ngoại lệ trả trẻ tại `NV08`, Ban giám hiệu cấp riêng **quyền trực giao nhận** (chọn nhân sự, danh sách lớp và khoảng thời gian áp dụng) hoặc **quyền duyệt ngoại lệ NV08** (`QT-DT-05`).
3. Khi `NV04` ghi nhận biến động `Tạm nghỉ` (từ 6 ngày làm việc liên tục trở lên hoặc nghỉ theo chế độ) hoặc `Nghỉ việc` có hiệu lực (`QT-NS-03`), hệ thống nhắc Ban giám hiệu khóa tài khoản gắn với hồ sơ nhân sự đó. Ban giám hiệu xác nhận khóa, khả năng đăng nhập bị chặn từ đúng ngày hiệu lực (`YC-NS-05`).
4. Khi hết thời hạn `Tạm nghỉ` đã ghi tại `NV04`, hệ thống nhắc và Ban giám hiệu mở lại tài khoản từ ngày kết thúc theo quy ước chung.

## Luồng thay thế và ngoại lệ

- **Bước 1 (nhân sự chưa xác nhận hoặc đã nghỉ việc):** Nếu nhân sự chưa có hồ sơ `NV04` đã xác nhận hoặc đang ở trạng thái `Nghỉ việc`, hệ thống chặn tạo mới hoặc kích hoạt tài khoản.
- **Bước 2 (phân tách hai quyền NV08):** Hệ thống kiểm tra tại tầng nghiệp vụ (`NFR-01`) để quyền trực giao nhận không tự động bao gồm quyền duyệt ngoại lệ hoặc quyền duyệt hủy lượt trả (`QT-DT-05`, `QT-DT-07`); hết thời hạn được cấp, quyền trực tự động hết hiệu lực.
- **Bước 3 (đơn nghỉ ngắn ngày tại NV05):** Nếu giáo viên chỉ có đơn nghỉ ngắn ngày tối đa 5 ngày làm việc liên tục được duyệt tại `NV05` (`QT-PC-06`), hệ thống **không khóa tài khoản** và chỉ giới hạn thao tác lớp trong ngày nghỉ theo phân công `NV05`.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã hồ sơ nhân sự (`NV04`), tên đăng nhập, mật khẩu (lưu băm), danh sách hệ thống con/vai trò, phạm vi quyền trực giao nhận (`NV08`: lớp, từ ngày/giờ – đến ngày/giờ), quyền duyệt ngoại lệ `NV08`, sự kiện biến động (`Tạm nghỉ`/`Nghỉ việc`/`Hết tạm nghỉ`) và ngày hiệu lực. | Tài khoản và trạng thái (`Hoạt động` / `Khóa`), quyền hệ thống con, lịch sử phân quyền trực/ngoại lệ `NV08`, nhật ký khóa/mở tài khoản gắn mã nhân sự. |

## Liên phân hệ và truy vết

- **NV04 → F1.0:** Mỗi tài khoản bắt buộc gắn một mã hồ sơ nhân sự `NV04`; biến động `Tạm nghỉ` hoặc `Nghỉ việc` kích hoạt khóa/mở tài khoản (`YC-NS-05`, `QT-NS-03`).
- **F1.0 → Tất cả phân hệ:** Kiểm tra quyền tại tầng nghiệp vụ (`NFR-01`), đặc biệt cấp tách biệt quyền trực giao nhận và quyền duyệt ngoại lệ cho `NV08` (`UC-08`, `UC-09`, `QT-DT-05`).

| Yêu cầu / NFR | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-NS-05, NFR-01, NFR-02 | QT-NS-03, QT-PC-06, QT-DT-05, QT-DT-07 | Cấp tài khoản gắn hồ sơ nhân sự, khóa/mở theo biến động `NV04` và phân quyền trực/ngoại lệ/duyệt hủy `NV08`. |
