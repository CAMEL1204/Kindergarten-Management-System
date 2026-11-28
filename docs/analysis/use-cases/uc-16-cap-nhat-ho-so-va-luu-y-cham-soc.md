# UC-16 · Cập nhật hồ sơ trẻ, ủy quyền nhận trẻ và lưu ý chăm sóc

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV01 · F1.1.3, F1.1.4, F1.1.5 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu (Hiệu trưởng duyệt sửa nhân thân; Phó hiệu trưởng chuyên môn xác nhận lưu ý chăm sóc), Giáo viên (đề nghị cập nhật lưu ý chăm sóc) |
| Mục tiêu | Duy trì một nguồn hồ sơ điện tử thống nhất cho nhân thân trẻ, danh sách người có quyền nhận trẻ và lưu ý chăm sóc, bảo đảm lưu vết và phân quyền giữa các bộ phận. |
| Điều kiện trước | Trẻ đã có hồ sơ trên hệ thống. Giáo viên chỉ thao tác đề nghị đối với trẻ thuộc lớp mình phụ trách. |
| Điều kiện sau | Mọi thay đổi được lưu lịch sử trước/sau, người đề nghị, người thực hiện, người duyệt và thời điểm. Dữ liệu đang có hiệu lực được cung cấp chỉ đọc cho NV06, NV07 và NV08 theo đúng quyền. |

## Luồng sự kiện chính

### Nhánh A · Cập nhật thông tin nhân thân và người giám hộ chính

1. Văn phòng tra cứu hồ sơ trẻ, nhập nội dung sửa (ngày sinh, thông tin liên hệ, người giám hộ chính), đính kèm hoặc ghi căn cứ giấy tờ gốc đã đối chiếu.
2. Nếu thay đổi thuộc diện cần duyệt (ngày sinh hoặc người giám hộ chính), hệ thống lưu ở trạng thái `Chờ duyệt` và giữ nguyên thông tin cũ đang có hiệu lực cho các phân hệ khác cho tới khi được duyệt.
3. Ban giám hiệu (Hiệu trưởng) đối chiếu căn cứ và duyệt; hệ thống áp dụng giá trị mới, lưu lịch sử trước/sau và cập nhật thông tin định danh/giám hộ chính chỉ đọc cho NV06 và NV08.

### Nhánh B · Quản lý người được ủy quyền cố định nhận trẻ

1. Văn phòng mở mục người đưa đón trên hồ sơ trẻ và chọn đăng ký mới, gia hạn hoặc thu hồi ủy quyền cố định do người giám hộ xác nhận.
2. Văn phòng nhập đủ họ tên, quan hệ với trẻ, số điện thoại, số CCCD, `Từ ngày` và `Đến ngày`. Hệ thống kiểm tra `Từ ngày <= Đến ngày`, lưu ngày kết thúc nội bộ bằng `Đến ngày + 1` theo quy ước chung và ẩn/che số CCCD trên các danh sách thông thường.
3. Hệ thống cập nhật danh sách người có quyền nhận trẻ theo ngày cho NV08 tra cứu tại thời điểm trả trẻ.

### Nhánh C · Đề nghị và xác nhận lưu ý chăm sóc (dị ứng, chế độ ăn, sức khỏe)

1. Giáo viên lớp phụ trách (khi nhận thông báo từ phụ huynh) gửi đề nghị cập nhật lưu ý chăm sóc cho trẻ lớp mình; hoặc văn phòng tiếp nhận trực tiếp từ phụ huynh.
2. Văn phòng (bộ phận duy nhất được ghi/sửa) cập nhật nội dung lưu ý, thời điểm bắt đầu và kết thúc dự kiến (nếu có) vào hồ sơ trẻ ở trạng thái `Chờ xác nhận`.
3. Hệ thống hiển thị ngay cảnh báo `lưu ý chờ xác nhận` cho giáo viên lớp và bếp (NV07) để rà soát các bữa ăn bị ảnh hưởng trong khi chờ duyệt.
4. Phó hiệu trưởng chuyên môn kiểm tra và xác nhận; lưu ý chuyển sang hiệu lực chính thức và hiển thị trên màn hình lớp và bếp.

## Luồng thay thế và ngoại lệ

- **Nhánh A, Bước 1–3 (sửa liên hệ thông thường hoặc từ chối duyệt):** Nếu chỉ cập nhật số điện thoại liên hệ hoặc thông tin không thuộc diện trình duyệt (theo quy định trường), văn phòng lưu trực tiếp kèm lịch sử thay đổi. Nếu Ban giám hiệu từ chối sửa ngày sinh/giám hộ chính, lưu lý do và giữ nguyên dữ liệu cũ.
- **Nhánh B, Bước 2 (sai khoảng ngày hoặc thiếu CCCD):** Nếu `Từ ngày > Đến ngày`, thiếu CCCD hoặc thiếu xác nhận của người giám hộ, hệ thống chặn lưu.
- **Nhánh B, Bước 1–3 (thu hồi ủy quyền sớm):** Khi người giám hộ yêu cầu thu hồi trước hạn, văn phòng ghi nhận thời điểm thu hồi; quyền nhận trẻ hết hiệu lực ngay từ lúc thu hồi có hiệu lực (`QT-HS-05`) và NV08 không còn chọn được người này khi trả trẻ.
- **Nhánh C, Bước 1–4 (giáo viên tự sửa hoặc bị từ chối):** Giáo viên không có quyền sửa trực tiếp lưu ý chăm sóc. Nếu Phó hiệu trưởng chuyên môn từ chối đề nghị/lưu ý, hệ thống ghi lý do, gỡ trạng thái hiệu lực/cảnh báo chờ xác nhận và giữ nguyên lịch sử đối chiếu (`QT-HS-06`).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, loại cập nhật, nội dung trước/sau, căn cứ giấy tờ, thông tin người được ủy quyền (họ tên, quan hệ, SĐT, CCCD, `Từ ngày`–`Đến ngày`, thông tin thu hồi), đề nghị/nội dung lưu ý chăm sóc, quyết định duyệt/xác nhận và lý do. | Hồ sơ trẻ hiện hành, lịch sử thay đổi trước/sau, danh sách người có quyền nhận trẻ còn hiệu lực (che CCCD theo quyền), lưu ý chăm sóc chính thức và cảnh báo `Chờ xác nhận`. |

## Liên phân hệ và truy vết

- **NV01 → NV08:** Cung cấp chỉ đọc người giám hộ chính đang có hiệu lực và người được ủy quyền cố định còn hiệu lực theo ngày trả (kể cả cập nhật thu hồi tức thời theo `QT-HS-05`) để NV08 (`UC-09`) kiểm tra theo `QT-DT-04`. Chỉ người được cấp quyền đối chiếu tại NV08 mới xem được đầy đủ CCCD.
- **NV01 → NV06:** Cung cấp chỉ đọc thông tin định danh trẻ và người giám hộ chính đang có hiệu lực (họ tên, quan hệ, SĐT) để phát hành thông báo học phí, ghi biên lai và xét miễn giảm; không chia sẻ lưu ý chăm sóc, giấy tờ hay danh sách ủy quyền.
- **NV01 → NV07 & Giáo viên:** Cung cấp chỉ đọc lưu ý chăm sóc còn hiệu lực và hiển thị cảnh báo ngay với lưu ý `Chờ xác nhận` (`QT-HS-06`).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HS-04 | QT-HS-04 | Sửa thông tin nhân thân có căn cứ, lưu lịch sử, phân quyền trình duyệt và cung cấp chỉ đọc cho NV06. |
| YC-HS-05 | QT-HS-05, QT-DT-04 | NV01 → NV08 · Đăng ký, gia hạn, thu hồi ủy quyền cố định, quy ước `Đến ngày + 1` và che CCCD phục vụ tra cứu trả trẻ. |
| YC-HS-06 | QT-HS-06 | NV01 → NV07 · Đề nghị của giáo viên, văn phòng ghi, phó hiệu trưởng chuyên môn xác nhận và cảnh báo sớm. |
