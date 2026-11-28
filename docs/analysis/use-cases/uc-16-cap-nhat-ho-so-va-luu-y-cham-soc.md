# UC-16 · Cập nhật hồ sơ trẻ, ủy quyền nhận trẻ và lưu ý chăm sóc

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV01 · F1.1.3, F1.1.4, F1.1.5 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu (hiệu trưởng duyệt sửa nhân thân, phó hiệu trưởng chuyên môn xác nhận lưu ý chăm sóc). Giáo viên (đề nghị cập nhật lưu ý chăm sóc) |
| Mục tiêu | Giữ một nguồn hồ sơ điện tử cho nhân thân trẻ, danh sách người được nhận trẻ và lưu ý chăm sóc. Mọi thay đổi có lịch sử và đúng phân quyền giữa các bộ phận. |
| Điều kiện trước | Trẻ đã có hồ sơ trên hệ thống. Giáo viên chỉ gửi đề nghị cho trẻ thuộc lớp mình phụ trách. |
| Điều kiện sau | Mọi thay đổi lưu giá trị trước và sau, người đề nghị, người thực hiện, người duyệt và thời điểm. Dữ liệu còn hiệu lực được cung cấp chỉ đọc cho NV06, NV07 và NV08 theo quyền. |

## Luồng sự kiện chính

### Nhánh A · Cập nhật nhân thân và người giám hộ chính

1. Văn phòng tra hồ sơ trẻ, nhập nội dung sửa (ngày sinh, thông tin liên hệ, người giám hộ chính) và ghi căn cứ giấy tờ gốc đã đối chiếu.
2. Nếu thay đổi thuộc diện cần duyệt (ngày sinh hoặc người giám hộ chính), hệ thống lưu ở trạng thái `Chờ duyệt`. Các phân hệ khác vẫn dùng giá trị cũ cho tới khi duyệt.
3. Hiệu trưởng đối chiếu căn cứ và duyệt. Hệ thống áp dụng giá trị mới, lưu lịch sử và cập nhật định danh, người giám hộ chính chỉ đọc cho NV06 và NV08 (QT-HS-04).

### Nhánh B · Quản lý người được ủy quyền cố định nhận trẻ

1. Văn phòng mở mục người đưa đón trên hồ sơ trẻ, chọn đăng ký mới, gia hạn hoặc thu hồi ủy quyền cố định do người giám hộ xác nhận.
2. Văn phòng nhập họ tên, quan hệ với trẻ, số điện thoại, số CCCD, `Từ ngày` và `Đến ngày`. Hệ thống kiểm tra `Từ ngày` không sau `Đến ngày`. Hệ thống lưu ngày kết thúc bằng `Đến ngày` cộng một ngày theo quy ước chung và che số CCCD trên danh sách thông thường (QT-HS-05).
3. Hệ thống cập nhật danh sách người được nhận trẻ theo ngày để NV08 tra cứu tại thời điểm trả trẻ.

### Nhánh C · Đề nghị và xác nhận lưu ý chăm sóc

1. Giáo viên nhận thông báo từ phụ huynh và gửi đề nghị cập nhật lưu ý chăm sóc cho trẻ lớp mình. Văn phòng cũng có thể nhận trực tiếp từ phụ huynh.
2. Văn phòng là bộ phận duy nhất ghi và sửa lưu ý. Văn phòng ghi nội dung, ngày bắt đầu và ngày kết thúc dự kiến nếu có. Lưu ý ở trạng thái `Chờ xác nhận`.
3. Hệ thống hiển thị ngay cảnh báo lưu ý chờ xác nhận cho giáo viên lớp và bếp (NV07) để rà soát các bữa ăn bị ảnh hưởng (QT-HS-06).
4. Phó hiệu trưởng chuyên môn kiểm tra và xác nhận. Lưu ý có hiệu lực chính thức và hiển thị trên màn hình lớp và bếp.

## Luồng thay thế và ngoại lệ

- **Nhánh A (sửa liên hệ thông thường):** Thay đổi không thuộc diện duyệt, ví dụ số điện thoại liên hệ, được Văn phòng lưu trực tiếp kèm lịch sử.
- **Nhánh A (từ chối):** Hiệu trưởng từ chối sửa ngày sinh hoặc người giám hộ chính. Hệ thống lưu lý do và giữ dữ liệu cũ.
- **Nhánh B, bước 2 (sai khoảng ngày hoặc thiếu căn cứ):** `Từ ngày` sau `Đến ngày`, thiếu CCCD hoặc thiếu xác nhận của người giám hộ. Hệ thống chặn lưu.
- **Nhánh B (thu hồi sớm):** Người giám hộ yêu cầu thu hồi trước hạn. Văn phòng ghi thời điểm thu hồi. Quyền nhận trẻ hết hiệu lực ngay từ lúc thu hồi (QT-HS-05). NV08 không còn chọn được người này khi trả trẻ.
- **Nhánh C (giáo viên tự sửa hoặc bị từ chối):** Giáo viên không sửa trực tiếp lưu ý chăm sóc. Nếu phó hiệu trưởng chuyên môn từ chối, hệ thống ghi lý do, gỡ cảnh báo chờ xác nhận và giữ lịch sử (QT-HS-06).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, loại cập nhật, giá trị trước và sau, căn cứ giấy tờ. Người được ủy quyền: họ tên, quan hệ, số điện thoại, CCCD, `Từ ngày`, `Đến ngày`, thông tin thu hồi. Đề nghị và nội dung lưu ý chăm sóc. Quyết định duyệt, xác nhận và lý do. | Hồ sơ trẻ hiện hành và lịch sử thay đổi. Danh sách người được nhận trẻ còn hiệu lực, CCCD được che theo quyền. Lưu ý chăm sóc chính thức và cảnh báo `Chờ xác nhận`. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 01](../../survey/forms/mau-01-ho-so-tre.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Hiệu lực ủy quyền đến hết ngày | ND-042-02 Trần Văn Nam, từ 07/09/2026 đến hết 30/11/2026 | Lưu ngày kết thúc 01/12/2026. Ngày 30/11 còn hiệu lực, từ 01/12 hết hiệu lực (QT-HS-05). |
| Sai khoảng ngày | `Từ ngày` 01/12/2026, `Đến ngày` 30/11/2026 | Chặn lưu. |
| Thu hồi sớm | Người giám hộ thu hồi ủy quyền của ND-042-02 trước 30/11/2026 | Quyền hết hiệu lực từ lúc thu hồi. NV08 không chọn được người này ở [UC-09](uc-09-tra-tre-theo-nhom.md). |
| Lưu ý dị ứng đã xác nhận | HS2026-042 dị ứng tôm và cua. Văn phòng ghi, phó hiệu trưởng chuyên môn xác nhận ngày 04/09/2026, áp dụng từ 07/09/2026 | Lưu ý có hiệu lực từ 07/09/2026, hiện trên màn hình lớp Mầm 1 và bếp. |
| Lưu ý chờ xác nhận | Phụ huynh báo trẻ dị ứng sữa lúc 07:45, chưa được xác nhận trước bữa sáng | Giáo viên lớp và bếp vẫn thấy cảnh báo ngay (QT-HS-06). |
| Sửa ngày sinh | Ngày sinh trên hệ thống lệch với giấy tờ đã đối chiếu | Ở `Chờ duyệt`, các phân hệ vẫn dùng giá trị cũ. Sau khi hiệu trưởng duyệt mới áp dụng giá trị mới (QT-HS-04). |
| Phạm vi xem của Kế toán | Kế toán mở hồ sơ HS2026-042 từ NV06 | Chỉ thấy định danh trẻ và người giám hộ chính Trần Thị Mai. Không thấy lưu ý chăm sóc và danh sách ủy quyền. |

## Liên phân hệ và truy vết

- **NV01 → NV08:** Người giám hộ chính còn hiệu lực và người được ủy quyền cố định còn hiệu lực theo ngày trả, kể cả thu hồi tức thời (QT-HS-05), để [UC-09](uc-09-tra-tre-theo-nhom.md) kiểm tra theo QT-DT-04. Chỉ người được cấp quyền đối chiếu tại NV08 xem được đủ số CCCD.
- **NV01 → NV06:** Định danh trẻ và người giám hộ chính còn hiệu lực (họ tên, quan hệ, số điện thoại) để phát hành thông báo học phí, ghi biên lai và xét miễn giảm tại [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md), [UC-07](uc-07-ghi-thu-tien-va-in-bien-lai.md). Không chia sẻ lưu ý chăm sóc, giấy tờ hay danh sách ủy quyền.
- **NV01 → NV07 và giáo viên:** Lưu ý chăm sóc còn hiệu lực và cảnh báo ngay với lưu ý `Chờ xác nhận` (QT-HS-06).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HS-04 | QT-HS-04 | NV01 → NV06 · Sửa nhân thân có căn cứ, lưu lịch sử, phân quyền trình duyệt và cung cấp chỉ đọc cho NV06. |
| YC-HS-05 | QT-HS-05, QT-DT-04 | NV01 → NV08 · Đăng ký, gia hạn, thu hồi ủy quyền cố định, quy ước `Đến ngày` cộng một ngày và che CCCD. |
| YC-HS-06 | QT-HS-06 | NV01 → NV07 · Giáo viên đề nghị, Văn phòng ghi, phó hiệu trưởng chuyên môn xác nhận, cảnh báo sớm. |
