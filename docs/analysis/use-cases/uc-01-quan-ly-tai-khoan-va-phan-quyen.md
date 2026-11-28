# UC-01 · Quản lý tài khoản và phân quyền trực

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | F1.0 · F1.0.1 |
| Tác nhân chính | Ban giám hiệu |
| Tác nhân phụ | Văn phòng (cung cấp biến động nhân sự NV04) |
| Mục tiêu | Mỗi tài khoản gắn một hồ sơ nhân sự và được cấp hệ thống con theo vị trí công việc. Quyền trực giao nhận và quyền duyệt ngoại lệ NV08 được cấp riêng. Tài khoản bị khóa và mở lại đúng ngày hiệu lực của biến động nhân sự. |
| Điều kiện trước | Nhân sự có hồ sơ đã xác nhận tại NV04 và đang làm việc. Người thực hiện có quyền quản trị F1.0. |
| Điều kiện sau | Tài khoản có vai trò đúng phạm vi hệ thống con (NFR-01, NFR-02). Quyền trực và quyền duyệt ngoại lệ NV08 được lưu theo lớp và khoảng thời gian. Tài khoản bị khóa hoặc mở lại theo ngày hiệu lực của biến động (YC-NS-05, QT-NS-03). |

## Luồng sự kiện chính

1. Ban giám hiệu chọn hồ sơ nhân sự từ NV04 để tạo tài khoản. Mật khẩu được lưu dạng băm (NFR-02).
2. Ban giám hiệu cấp hệ thống con theo vị trí công việc. Người kiêm nhiều vị trí được cấp nhiều hệ thống con, chức năng của từng hệ thống con không bị gộp.
3. Khi cần người trực cổng, trực lớp hoặc người duyệt ngoại lệ trả trẻ, Ban giám hiệu cấp riêng **quyền trực giao nhận** (nhân sự, lớp, khoảng thời gian) hoặc **quyền duyệt ngoại lệ NV08** (QT-DT-05).
4. Khi NV04 ghi `Tạm nghỉ` hoặc `Nghỉ việc` có hiệu lực (QT-NS-03), hệ thống nhắc Ban giám hiệu khóa tài khoản gắn với hồ sơ đó. Ban giám hiệu xác nhận khóa. Tài khoản không đăng nhập được từ đúng ngày hiệu lực (YC-NS-05).
5. Khi NV04 ghi `Hết tạm nghỉ`, hệ thống nhắc Ban giám hiệu mở lại tài khoản. Tài khoản đăng nhập lại được từ ngày kết thúc tạm nghỉ.

## Luồng thay thế và ngoại lệ

- **Bước 1 (nhân sự chưa xác nhận hoặc đã nghỉ việc):** Hệ thống chặn tạo hoặc kích hoạt tài khoản.
- **Bước 3 (tách hai quyền NV08):** Hệ thống kiểm tra ở tầng nghiệp vụ (NFR-01). Quyền trực giao nhận không bao gồm quyền duyệt ngoại lệ hay quyền duyệt hủy lượt trả (QT-DT-05, QT-DT-07). Hết khoảng thời gian được cấp, quyền trực tự hết hiệu lực.
- **Bước 4 (đơn nghỉ ngắn ngày ở NV05):** Giáo viên có đơn nghỉ tối đa 5 ngày làm việc liên tục đã duyệt (QT-PC-06) không bị khóa tài khoản. Hệ thống chỉ chặn thao tác trên lớp trong những ngày nghỉ.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã hồ sơ nhân sự (NV04), tên đăng nhập, mật khẩu, hệ thống con và vai trò. Phạm vi quyền trực NV08: lớp, từ ngày giờ, đến ngày giờ. Quyền duyệt ngoại lệ NV08. Biến động `Tạm nghỉ`, `Nghỉ việc`, `Hết tạm nghỉ` kèm ngày hiệu lực. | Tài khoản và trạng thái `Hoạt động` hoặc `Khóa`. Quyền hệ thống con. Lịch sử cấp quyền trực và quyền duyệt ngoại lệ NV08. Nhật ký khóa và mở tài khoản theo mã nhân sự. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Khóa khi tạm nghỉ | NS-006 Trần Thu Trang tạm nghỉ hiệu lực 19/10/2026, trở lại 02/11/2026 ([Mẫu 06](../../survey/forms/mau-06-ho-so-nhan-su.md)) | Hệ thống nhắc Ban giám hiệu. Sau khi Lê Minh Tâm xác nhận, tài khoản bị khóa từ 19/10 và mở lại từ 02/11/2026. |
| Đơn nghỉ ngắn ngày | Giáo viên có đơn nghỉ 2 ngày làm việc đã duyệt | Tài khoản vẫn mở. Thao tác trên lớp trong 2 ngày nghỉ bị chặn (QT-PC-06). |
| Nhân sự chưa xác nhận | Hồ sơ nhân sự đang `Chờ xác nhận` tại NV04 | Chặn tạo tài khoản. |
| Kiêm nhiệm | Nhân viên văn phòng kiêm kế toán (README khảo sát §1) | Được cấp hai hệ thống con Văn phòng và Kế toán. Mỗi hệ thống con giữ đúng chức năng của mình. |
| Quyền trực có thời hạn | Nhân sự trực được cấp quyền giao nhận một lớp trong một khoảng giờ | Thao tác được trong phạm vi lớp và giờ được cấp. Ngoài khoảng đó bị chặn. Không duyệt được ngoại lệ nhận trẻ hộ. |
| Quyền duyệt ngoại lệ | Hiệu trưởng Lê Minh Tâm duyệt DT05-MP-1014-058 lúc 17:05 ngày 14/10/2026 ([Mẫu 02](../../survey/forms/mau-02-diem-danh-giao-nhan.md)) | Người có quyền duyệt ngoại lệ duyệt được. Giáo viên chỉ có quyền giao nhận thì không duyệt được. |

## Liên phân hệ và truy vết

- **NV04 → F1.0:** Mỗi tài khoản gắn một mã hồ sơ nhân sự. Biến động `Tạm nghỉ`, `Nghỉ việc`, `Hết tạm nghỉ` tại [UC-04](uc-04-lap-va-cap-nhat-ho-so-nhan-su.md) kích hoạt nhắc khóa hoặc mở tài khoản (YC-NS-05, QT-NS-03).
- **F1.0 → mọi phân hệ:** Kiểm tra quyền ở tầng nghiệp vụ (NFR-01). Quyền trực giao nhận và quyền duyệt ngoại lệ được cấp riêng cho [UC-08](uc-08-don-tre-theo-nhom.md), [UC-09](uc-09-tra-tre-theo-nhom.md) (QT-DT-05).

| Yêu cầu / NFR | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-NS-05, NFR-01, NFR-02 | QT-NS-03, QT-PC-06, QT-DT-05, QT-DT-07 | NV04 → F1.0 → NV08 · Cấp tài khoản gắn hồ sơ nhân sự, khóa và mở theo biến động NV04, tách quyền trực, quyền duyệt ngoại lệ và quyền duyệt hủy lượt trả NV08. |
