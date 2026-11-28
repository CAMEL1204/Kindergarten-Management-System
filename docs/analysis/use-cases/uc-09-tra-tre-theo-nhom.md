# UC-09 · Trả trẻ theo nhóm

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV08 · F1.8.3, F1.8.4, F1.8.5, F1.8.6 |
| Tác nhân chính | Giáo viên được phân công lớp trong ngày hoặc nhân sự trực được cấp quyền NV08 |
| Tác nhân phụ | Ban giám hiệu hoặc người được giao quyền duyệt ngoại lệ (duyệt nhận trẻ hộ, duyệt hủy lượt trả) |
| Mục tiêu | Giao từng trẻ cho đúng người nhận trẻ đủ điều kiện tại thời điểm trả, ghi người thực tế nhận và giờ giao thực tế. Trẻ chưa đủ căn cứ giao vẫn ở danh sách chưa trả và tiếp tục được trông. |
| Điều kiện trước | Trẻ có lượt đón chưa hủy và chưa có lượt trả chưa hủy (QT-DT-02). Người thực hiện có quyền giao nhận cho lớp và thời gian đó (QT-DT-05). NV01 có người giám hộ chính đã xác nhận và ủy quyền cố định của trẻ. Khung trả có hiệu lực đã cấu hình tại `UC-03` (mặc định 16:00–17:30). |
| Điều kiện sau | Mỗi dòng hợp lệ có một lượt trả: người thực tế nhận, giờ giao thực tế, người giao, mã căn cứ ủy quyền hoặc mã duyệt ngoại lệ. Trẻ chuyển sang `Đã trả`. Dòng bị chặn vẫn ở `Đã đón`, có lý do. Giờ về thực tế được chuyển cho NV03 và NV07. |

## Luồng sự kiện chính

1. Người thực hiện mở màn hình trả trẻ theo lớp và ngày. Hệ thống kiểm tra quyền (NFR-01, QT-DT-05) và chỉ hiển thị trẻ đã đón và chưa trả, kể cả khi điều chỉnh chuyên cần đang chờ duyệt (YC-DT-03).
2. Người thực hiện đánh dấu ô chọn của các trẻ có người đến nhận hoặc dùng "Chọn tất cả", rồi bấm "Trả n trẻ đã chọn" (YC-DT-02).
3. Hộp xác nhận hiển thị danh sách người đủ điều kiện của từng trẻ tại thời điểm trả: người giám hộ chính đã xác nhận, người có ủy quyền cố định còn hiệu lực (lưu `Đến ngày + 1`, hết hiệu lực ngay khi thu hồi) và người đã được duyệt nhận trẻ hộ cho lượt này (QT-HS-05, QT-DT-04). Số CCCD được che (NFR-03). Ô người nhận ban đầu để trống.
4. Với từng trẻ, người thực hiện đối chiếu danh tính thực tế, chọn đúng người đã đối chiếu và ghi giờ giao thực tế.
5. Người thực hiện bấm xác nhận một lần. Hệ thống kiểm tra lại từng dòng: quyền người thực hiện, người nhận đã chọn, căn cứ nhận còn hiệu lực tại giờ giao, có giờ giao thực tế (QT-DT-03).
6. Hệ thống ghi lượt trả cho các dòng đủ điều kiện trong một giao dịch (NFR-05). Dòng thiếu điều kiện được báo rõ lý do và tiếp tục ở danh sách chưa trả.
7. Hệ thống chuyển giờ về thực tế cho NV03 (không làm mất `Có mặt` hay hủy đề nghị đang chờ) và cho NV07 đối chiếu giao nhận suất ăn.

## Luồng con: xác minh và duyệt nhận trẻ hộ (F1.8.3)

1. Khi người đến nhận không có trong danh sách đủ điều kiện, có ủy quyền hết hạn hoặc bị thu hồi, người thực hiện lập yêu cầu nhận trẻ hộ cho trẻ và lượt trả đó. Yêu cầu ở `Chờ xác minh`.
2. Người thực hiện ghi căn cứ người giám hộ xác nhận trước khi giao: kênh đã đăng ký, thời điểm, nội dung. Người thực hiện đối chiếu CCCD người đến nhận tại cổng hoặc lớp và ghi kết quả. Chỉ người được cấp quyền đối chiếu mới xem thông tin định danh cần thiết (YC-DT-07).
3. Đủ căn cứ thì yêu cầu chuyển sang `Chờ duyệt` và được gửi cho Ban giám hiệu hoặc người được giao quyền duyệt ngoại lệ. Quyền ghi giao nhận và quyền duyệt ngoại lệ được cấp riêng (QT-DT-05).
4. Người duyệt xem căn cứ, rồi duyệt hoặc từ chối kèm lý do. Duyệt chỉ có giá trị cho trẻ, người nhận và lượt trả đã xác minh.
5. Sau khi duyệt, người nhận xuất hiện trong danh sách chọn ở bước 3 của luồng chính cho lượt này. Từ chối thì trẻ vẫn ở `Đã đón` và tiếp tục được trông.

## Luồng thay thế và ngoại lệ

- **Bước 1 (không có quyền):** Giáo viên không thuộc phân công trong ngày, có đơn nghỉ đã duyệt, đang tạm nghỉ, hoặc nhân sự trực ngoài phạm vi được giao: hệ thống chặn.
- **Bước 1 (trẻ sót lượt đón):** Trẻ thực tế có mặt nhưng không có trong danh sách trả. Giáo viên xác minh và bổ sung lượt đón tại [UC-08](uc-08-don-tre-theo-nhom.md) trước khi ghi trả (QT-DT-02).
- **Bước 5 (chưa chọn người nhận hoặc chưa có giờ giao):** Dòng bị chặn, báo lý do, không tạo lượt trả.
- **Bước 5 (ủy quyền hết hiệu lực):** Ủy quyền hết hạn hoặc vừa bị thu hồi tại `UC-16`: dòng bị chặn. Người thực hiện chuyển sang luồng con nhận trẻ hộ.
- **Luồng con bước 2 (chưa liên hệ được người giám hộ):** Yêu cầu giữ `Chờ xác minh`. Trẻ tiếp tục được trông tại trường (QT-DT-04).
- **Kết thúc khung trả:** Hệ thống cảnh báo trẻ còn ở trường và yêu cầu nhận trẻ hộ còn chờ (YC-DT-04). Người trực liên hệ người giám hộ, báo Ban giám hiệu và ghi người tiếp tục trông trẻ.
- **Hủy lượt trả ghi nhầm (F1.8.6):** Chỉ khi trẻ thực tế còn ở trường. Người thực hiện nhập lý do, Ban giám hiệu hoặc người được giao duyệt. Lượt trả được đặt `Đã hủy`, không xóa, trẻ quay lại `Đã đón` (QT-DT-07, NFR-04). Bản bàn giao 08:30 không đổi.
- **Lớp đông nhất:** Trả 35 trẻ của Lá 1 trong một lần xác nhận không gây treo giao diện (NFR-07).

## Tra cứu đón trả (F1.8.5)

- Tra lượt trả theo trẻ, lớp, ngày, người nhận thực tế. Lọc lượt trả theo ngoại lệ đã duyệt, yêu cầu đang chờ xử lý và trả muộn sau khung trả (YC-DT-06).
- Số lượt trả theo ngoại lệ và trả muộn trong kỳ là số liệu cho báo cáo lớp tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Lớp, ngày. Lượt đón chưa hủy (UC-08). Người giám hộ chính và ủy quyền cố định còn hiệu lực (NV01, chỉ đọc). Quyền giao nhận và quyền duyệt ngoại lệ (`UC-01`, NV05). Khung trả có hiệu lực (`UC-03`). Người nhận được chọn, giờ giao thực tế. Căn cứ xác nhận của người giám hộ, kết quả đối chiếu CCCD. Quyết định duyệt hoặc từ chối. | Lượt trả: mã, trẻ, lớp, giờ giao thực tế, người thực tế nhận, người giao, mã căn cứ ủy quyền hoặc mã duyệt ngoại lệ, trạng thái hủy. Hồ sơ xác minh người nhận: trạng thái, người và thời điểm xác minh, người và thời điểm duyệt, lý do. Danh sách chưa trả kèm lý do chặn. Giờ về thực tế cho NV03, NV07. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 02](../../survey/forms/mau-02-diem-danh-giao-nhan.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Người có ủy quyền còn hiệu lực | HS2026-042, bà ngoại Lê Thị Lan (ND-042-01) nhận lúc 16:15 ngày 14/10/2026 | Ghi lượt trả 16:15, người nhận ND-042-01. |
| Nhận trẻ hộ có duyệt | HS2026-058, mẹ báo trước 16:55, giáo viên gọi lại và đối chiếu CCCD 17:03, hiệu trưởng duyệt DT05-MP-1014-058 lúc 17:05 | Cậu Trần Anh Tuấn xuất hiện để chọn. Lượt trả 17:10 có mã duyệt ngoại lệ. |
| Trả khi điều chỉnh chờ duyệt | HS2026-042 đón 09:00 ngày 16/10, trả 16:30, đề nghị duyệt lúc 16:45 | Lượt trả được ghi lúc 16:30. Đề nghị ở NV03 giữ nguyên tới khi duyệt. |
| Nhóm có dòng bị chặn | Ngày 01/12/2026 chọn HS2026-001 (mẹ) và HS2026-042 (Trần Văn Nam, ND-042-02, ủy quyền hết hạn từ 01/12) | Chỉ ghi trả HS2026-001 lúc 16:35. HS2026-042 báo lý do, yêu cầu nhận trẻ hộ `Chờ xác minh`, trẻ vẫn `Đã đón`. |
| Trả sau dòng bị chặn | Bà ngoại ND-042-01 đến 17:05 cùng ngày | Ghi lượt trả 17:05. Yêu cầu trước được lưu với kết quả chưa thực hiện. |

## Liên phân hệ và truy vết

- **NV01 → NV08:** Người giám hộ chính và ủy quyền cố định còn hiệu lực tại thời điểm trả từ `UC-16` (QT-HS-05, QT-DT-04). CCCD được che trên danh sách thông thường.
- **F1.0, NV05 → NV08:** Quyền trực và quyền duyệt ngoại lệ cấp riêng tại `UC-01`. Giáo viên phụ trách lớp theo ngày từ [UC-05](uc-05-lap-phan-cong-giao-vien.md) (QT-DT-05). Khung trả từ `UC-03`.
- **NV08 → NV03:** Giờ về thực tế. Lượt trả không làm mất `Có mặt` và không hủy đề nghị đang chờ tại `UC-14`.
- **NV08 → NV07:** Giờ về thực tế cho đối chiếu giao nhận suất ăn tại `UC-15`.
- **NV08 → NV09:** Số lượt trả theo ngoại lệ và trả muộn cho báo cáo lớp tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DT-02, YC-DT-03 | QT-DT-02, QT-DT-03 | NV08 · Danh sách trả gồm trẻ đã đón và chưa trả, xác nhận nhóm kiểm tra từng dòng. |
| YC-DT-07 | QT-DT-04, QT-DT-05, QT-HS-05 | NV01 → NV08 · Tra quyền nhận trẻ, xác minh, duyệt hoặc từ chối nhận trẻ hộ. |
| YC-DT-04 | QT-DT-04 | F1.0 → NV08 · Cảnh báo trẻ chưa trả khi hết khung trả. |
| YC-DT-06 | QT-DT-05 | NV08 → NV09 · Tra cứu lượt trả, ngoại lệ, trả muộn. |
| YC-DT-08 | QT-DT-07 | NV08 · Hủy lượt trả ghi nhầm có duyệt. |
