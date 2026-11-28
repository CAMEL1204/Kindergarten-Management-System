# UC-04 · Lập và cập nhật hồ sơ nhân sự

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV04 · F1.4.1, F1.4.2, F1.4.3, F1.4.4 |
| Tác nhân chính | Văn phòng (người phụ trách nhân sự) |
| Tác nhân phụ | Ban giám hiệu (xác nhận tiếp nhận và biến động nhân sự) |
| Mục tiêu | Mỗi nhân sự có một hồ sơ duy nhất kèm danh sách bằng cấp, chứng chỉ, giấy tờ và lịch sử biến động. Hệ thống cung cấp danh sách nhân sự đủ điều kiện đứng lớp tại một ngày cho NV05 và sự kiện khóa, mở tài khoản cho F1.0. |
| Điều kiện trước | Người thực hiện đăng nhập hệ thống con Văn phòng và có quyền NV04. Danh mục vị trí, loại nhân sự và loại giấy tờ đã có. |
| Điều kiện sau | Hồ sơ được lưu cùng các dòng bằng cấp, chứng chỉ, giấy tờ trong một giao dịch. Hồ sơ đã xác nhận có trạng thái làm việc, người và ngày xác nhận. Biến động có ngày hiệu lực, giá trị trước và sau, người xác nhận. Danh sách đủ điều kiện đứng lớp được cập nhật. |

## Luồng sự kiện chính

1. Văn phòng chọn "Lập hồ sơ nhân sự" và nhập họ tên, ngày sinh, giấy tờ tùy thân.
2. Hệ thống tra hồ sơ có họ tên, ngày sinh hoặc giấy tờ tùy thân trùng và hiển thị kết quả để đối chiếu (QT-NS-01, YC-NS-01).
3. Không có hồ sơ trùng, Văn phòng nhập thông tin cá nhân và thông tin công việc: vị trí, loại nhân sự, có được giao đứng lớp hoặc dạy thay, ngày bắt đầu, loại hợp đồng.
4. Trên cùng màn hình, Văn phòng thêm các dòng bằng cấp, chứng chỉ (loại, tên, chuyên ngành, ngày cấp, ngày hết hạn nếu có) và các dòng giấy tờ (loại, tình trạng, ngày nhận) (YC-NS-02).
5. Văn phòng lưu. Hệ thống ghi hồ sơ và toàn bộ dòng chi tiết trong một giao dịch (NFR-05), cấp mã nhân sự duy nhất. Hồ sơ ở trạng thái `Chờ xác nhận`. Giấy tờ chưa nhận được đưa vào danh sách cần bổ sung.
6. Ban giám hiệu xem hồ sơ, xác nhận vị trí và ngày bắt đầu. Hệ thống chuyển trạng thái sang `Đang làm việc` và lưu người, ngày xác nhận.
7. Hệ thống tính lại danh sách đủ điều kiện đứng lớp: nhân sự đang làm việc, được giao đứng lớp hoặc dạy thay, có bằng cao đẳng sư phạm mầm non (giáo dục mầm non) trở lên (QT-NS-02). Danh sách này được NV05 đọc tại [UC-05](uc-05-lap-phan-cong-giao-vien.md).

## Luồng con: ghi nhận biến động nhân sự (F1.4.3)

1. Văn phòng mở hồ sơ, chọn loại biến động: tạm nghỉ, hết tạm nghỉ, nghỉ việc hoặc chuyển vị trí. Văn phòng nhập ngày hiệu lực, ngày kết thúc nếu có, lý do và căn cứ (NS-03, NS-04).
2. Với tạm nghỉ, hệ thống kiểm tra thời gian nghỉ từ 6 ngày làm việc liên tục trở lên hoặc nghỉ theo chế độ (QT-NS-03).
3. Hệ thống tìm các dòng phân công có hiệu lực bị ảnh hưởng và hiển thị lớp, vai trò, khoảng ngày.
4. Ban giám hiệu xác nhận biến động. Hệ thống lưu biến động kèm giá trị trước và sau, người xác nhận. Trạng thái làm việc đổi theo ngày hiệu lực. Hồ sơ không bị xóa (QT-NS-03, NFR-04).
5. Hệ thống báo người phân công các lớp bị ảnh hưởng để xếp dạy thay hoặc thay phân công tại `UC-05`.
6. Với tạm nghỉ hoặc nghỉ việc, hệ thống nhắc Ban giám hiệu khóa tài khoản từ ngày hiệu lực tại `UC-01`. Khi hết tạm nghỉ, nhắc mở lại tài khoản từ ngày kết thúc (YC-NS-05).

## Luồng thay thế và ngoại lệ

- **Bước 2 (hồ sơ trùng):** Văn phòng đối chiếu. Nếu là nhân sự cũ quay lại làm, Văn phòng mở lại hồ sơ cũ và ghi biến động mới thay vì lập hồ sơ mới (QT-NS-01).
- **Bước 5 (thiếu trường bắt buộc):** Hệ thống chặn lưu ở cả giao diện và tầng nghiệp vụ, báo trường còn thiếu (NFR-06).
- **Bước 6 (chưa đủ căn cứ):** Ban giám hiệu trả lại kèm ý kiến. Hồ sơ giữ `Chờ xác nhận`.
- **Chứng chỉ sắp hết hạn:** Hệ thống đưa chứng chỉ có ngày hết hạn vào danh sách cần xử lý từ 30 ngày trước ngày hết hạn. Chứng chỉ cấp lại được ghi thành dòng mới, dòng cũ được giữ (QT-NS-05). Chứng chỉ có thời hạn như sơ cấp cứu không loại nhân sự khỏi danh sách đủ điều kiện đứng lớp (QT-NS-02).
- **Luồng con bước 2 (nghỉ ngắn):** Nghỉ tối đa 5 ngày làm việc không ghi tạm nghỉ. Hệ thống chặn và hướng giáo viên gửi đơn nghỉ ngắn ngày tại `UC-05` (QT-PC-06).
- **Sửa thông tin quan trọng:** Đổi số điện thoại, bổ sung chứng chỉ hoặc sửa giấy tờ tùy thân được lưu nội dung thay đổi, thời điểm và người cập nhật (QT-NS-04).

## Tra cứu và xuất danh sách (F1.4.4)

- Văn phòng và Ban giám hiệu tra hồ sơ theo mã, họ tên, vị trí hoặc trạng thái. Thông tin nhạy cảm chỉ hiển thị với người được phân quyền (QT-NS-04, NFR-03).
- Hệ thống lập danh sách đủ điều kiện đứng lớp tại một ngày chọn, danh sách cần xử lý (giấy tờ thiếu, chứng chỉ sắp hoặc đã hết hạn) và lịch sử biến động.
- Văn phòng lọc và xuất danh sách theo vị trí, trạng thái. Số nhân sự vào làm, nghỉ việc và chứng chỉ sắp hết hạn trong kỳ là số liệu cho báo cáo nhân sự tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Họ tên, ngày sinh, giới tính, giấy tờ tùy thân, địa chỉ, số điện thoại, email. Vị trí, loại nhân sự, được giao đứng lớp hoặc dạy thay, ngày bắt đầu, loại hợp đồng. Danh sách bằng cấp, chứng chỉ, giấy tờ. Biến động: loại, ngày hiệu lực, ngày kết thúc, lý do, người xác nhận. | Hồ sơ nhân sự có mã duy nhất và trạng thái. Danh sách bằng cấp, chứng chỉ, giấy tờ. Danh sách cần xử lý. Lịch sử biến động và lịch sử sửa đổi. Danh sách đủ điều kiện đứng lớp tại một ngày. Danh sách lớp bị ảnh hưởng. Nhắc khóa hoặc mở tài khoản. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 06](../../survey/forms/mau-06-ho-so-nhan-su.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Lập hồ sơ kèm chi tiết | NS-006 Trần Thu Trang, 1 bằng, 1 chứng chỉ, 4 giấy tờ đã nhận | Lưu trong một giao dịch. Sau khi Lê Minh Tâm xác nhận, trạng thái `Đang làm việc`, có trong danh sách đủ điều kiện đứng lớp. |
| Chứng chỉ sắp hết hạn | Chứng chỉ sơ cấp cứu hết hạn 09/12/2026 | Vào danh sách cần xử lý từ 09/11/2026. NS-006 vẫn đủ điều kiện đứng lớp. |
| Tạm nghỉ | Nghỉ ốm 10 ngày làm việc, hiệu lực 19/10/2026, trở lại 02/11/2026 | Trạng thái tạm nghỉ, loại khỏi danh sách đủ điều kiện trong khoảng đó. Lớp Mầm 2 vào danh sách bị ảnh hưởng. Nhắc khóa tài khoản từ 19/10 và mở lại từ 02/11/2026. |
| Nghỉ ngắn ghi nhầm vào NV04 | Giáo viên nghỉ 2 ngày làm việc | Chặn ghi tạm nghỉ, hướng sang đơn nghỉ ngắn ngày ở NV05. |
| Nhân sự quay lại | Nhân sự đã nghỉ việc, lập lại sau một năm với cùng giấy tờ tùy thân | Hệ thống báo trùng. Văn phòng dùng lại hồ sơ cũ, không cấp mã mới. |
| Kế toán không đủ điều kiện | Nhân viên kế toán đang làm việc | Không có trong danh sách đủ điều kiện đứng lớp (QT-NS-02). |

## Liên phân hệ và truy vết

- **NV04 → NV05:** Danh sách đủ điều kiện đứng lớp tại ngày hiệu lực và tạm nghỉ đã xác nhận, chỉ đọc tại [UC-05](uc-05-lap-phan-cong-giao-vien.md) (QT-NS-02, QT-PC-01).
- **NV04 → F1.0:** Mỗi tài khoản gắn một hồ sơ nhân sự. Biến động `Tạm nghỉ`, `Nghỉ việc`, `Hết tạm nghỉ` kích hoạt khóa hoặc mở tài khoản tại `UC-01` (YC-NS-05, QT-NS-03).
- **NV04 → NV09:** Số liệu nhân sự cho báo cáo của Văn phòng tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-NS-01 | QT-NS-01 | NV04 · Lập hồ sơ, tra trùng, dùng lại hồ sơ cũ. |
| YC-NS-02 | QT-NS-02, QT-NS-05 | NV04 · Bằng cấp, chứng chỉ, giấy tờ trên một màn hình, cảnh báo hết hạn. |
| YC-NS-03 | QT-NS-02, QT-NS-03, QT-PC-06 | NV04 → NV05 · Biến động theo ngày hiệu lực, danh sách đủ điều kiện, lớp bị ảnh hưởng. |
| YC-NS-04 | QT-NS-04 | NV04 → NV09 · Phân quyền xem, lịch sử sửa, xuất danh sách. |
| YC-NS-05 | QT-NS-03 | NV04 → F1.0 · Nhắc khóa và mở tài khoản theo biến động. |
