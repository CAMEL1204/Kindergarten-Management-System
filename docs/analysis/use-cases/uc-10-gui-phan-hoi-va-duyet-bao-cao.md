# UC-10 · Gửi, phản hồi và duyệt báo cáo

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV09 · F1.9.1, F1.9.2, F1.9.3, F1.9.4 |
| Tác nhân chính | Người lập báo cáo của bộ phận: Giáo viên, Văn phòng, Kế toán, Bộ phận bếp |
| Tác nhân phụ | Ban giám hiệu (xem, phản hồi, yêu cầu bổ sung, duyệt, tổng hợp toàn trường) |
| Mục tiêu | Mỗi bộ phận lập báo cáo trong phạm vi của mình từ số liệu đã chốt, ghi chú và gửi Ban giám hiệu. Ban giám hiệu duyệt và tổng hợp toàn trường từ các phiên bản đã duyệt, nên mỗi con số có bộ phận đứng tên. |
| Điều kiện trước | Người lập đăng nhập hệ thống con của mình. Danh mục loại báo cáo, bộ phận lập, kỳ, hạn gửi và chỉ tiêu đã có tại `UC-02` (YC-BC-05). Số liệu nguồn của kỳ đã được bộ phận sở hữu chốt. |
| Điều kiện sau | Báo cáo đã gửi có ảnh chụp số liệu tại lúc gửi, bị khóa nội dung và có phiên bản. Phản hồi gắn với phiên bản. Báo cáo đã duyệt được dùng cho bảng tổng hợp toàn trường. |

## Luồng sự kiện chính

1. Người lập mở "Báo cáo của tôi", chọn loại báo cáo và kỳ. Hệ thống chỉ hiển thị loại báo cáo của bộ phận người lập. Hệ thống kiểm tra bộ phận chưa có báo cáo cùng loại, phạm vi và kỳ, trừ báo cáo đột xuất (QT-BC-06).
2. Hệ thống lấy số liệu đã chốt trong phạm vi của người lập (QT-BC-02). Với chuyên cần, hệ thống hiển thị riêng lượt `Chờ xác minh` và đánh dấu kỳ còn ngày chưa khóa hoặc điều chỉnh chờ duyệt.
3. Người lập thêm ghi chú chung, ghi chú theo từng dòng và tệp đính kèm, rồi lưu `Nháp`. Ban giám hiệu chưa thấy báo cáo nháp (QT-BC-01).
4. Người lập xem lại và bấm "Gửi Ban giám hiệu". Hệ thống lưu ảnh chụp số liệu tại lúc gửi, ghi người gửi, thời điểm, phiên bản và khóa nội dung (QT-BC-03, QT-BC-04). Báo cáo gửi sau hạn được đánh dấu trễ hạn (QT-BC-06).
5. Ban giám hiệu mở "Hộp báo cáo nhận được", lọc theo bộ phận, loại, kỳ, trạng thái. Ban giám hiệu đọc số liệu, ghi chú và viết phản hồi gắn với phiên bản đang xem.
6. Ban giám hiệu chọn "Duyệt". Báo cáo chuyển sang `Đã duyệt`.
7. Ban giám hiệu mở bảng tổng hợp toàn trường. Hệ thống ghép chỉ tiêu từ phiên bản đã duyệt mới nhất của từng báo cáo, đánh dấu kỳ chưa đầy đủ và theo dõi bộ phận chưa gửi hoặc gửi trễ hạn (QT-BC-05). Ban giám hiệu xuất Excel hoặc PDF (YC-BC-04).

## Luồng thay thế và ngoại lệ

- **Bước 1 (đã có báo cáo cùng kỳ):** Hệ thống mở báo cáo đang có thay vì tạo mới.
- **Bước 2 (ngoài phạm vi):** Giáo viên lớp A không lấy được số liệu lớp B. Kế toán không có loại báo cáo suất ăn hay chuyên cần (QT-BC-02).
- **Bước 6 (yêu cầu bổ sung):** Ban giám hiệu chọn "Yêu cầu bổ sung" kèm nội dung. Báo cáo chuyển sang `Yêu cầu bổ sung`, người lập nhận thông báo (YC-BC-03). Người lập sửa, rồi gửi lại thành phiên bản mới. Phiên bản cũ và phản hồi được giữ (QT-BC-03).
- **Số liệu nguồn đổi sau khi gửi:** Báo cáo không tự đổi, hệ thống cảnh báo người lập. Nếu báo cáo đang `Đã gửi`, Ban giám hiệu xử lý như yêu cầu bổ sung khi cần (QT-BC-04).
- **Số liệu nguồn đổi sau khi duyệt:** Báo cáo chuyển sang `Cần cập nhật`. Phiên bản đã duyệt vẫn dùng cho tổng hợp, kỳ được đánh dấu số liệu đang cập nhật. Người lập tạo phiên bản mới từ số liệu hiện hành rồi gửi lại (QT-BC-04, QT-BC-05).
- **Báo cáo đột xuất:** Giáo viên lập báo cáo tình hình trẻ khi có sự việc, không bị giới hạn một báo cáo mỗi kỳ.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Loại báo cáo, kỳ, phạm vi (`UC-02`). Số liệu đã chốt của bộ phận: chuyên cần và đến muộn, trả muộn (NV03, NV08), tuyển sinh, sĩ số (NV01, NV02), nhân sự, phân công (NV04, NV05), thu học phí (NV06), suất ăn (NV07). Ghi chú chung, ghi chú theo dòng, tệp đính kèm. Phản hồi, quyết định duyệt. | Báo cáo có mã, loại, kỳ, bộ phận, phạm vi, người lập. Dòng số liệu chụp tại lúc gửi. Trạng thái `Nháp`, `Đã gửi`, `Yêu cầu bổ sung`, `Đã duyệt`, `Cần cập nhật`. Phiên bản, thời điểm gửi, cờ trễ hạn, người và thời điểm duyệt. Phản hồi gắn phiên bản. Bảng tổng hợp toàn trường kèm cờ chưa đầy đủ hoặc đang cập nhật. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Nháp không hiện với Ban giám hiệu | Giáo viên Mầm 1 lưu nháp báo cáo chuyên cần tháng 10 | Hộp báo cáo của Ban giám hiệu không có báo cáo này (QT-BC-01). |
| Gửi trễ hạn | Văn phòng gửi báo cáo sĩ số tháng 10 ngày 08/11, hạn ngày 05/11 | Báo cáo được nhận và đánh dấu trễ hạn (QT-BC-06). |
| Sửa sau khi gửi | Kế toán phát hiện sai sau khi gửi | Không sửa trực tiếp. Ban giám hiệu yêu cầu bổ sung, Kế toán gửi phiên bản 2, phiên bản 1 được giữ. |
| Nguồn đổi sau khi duyệt | Báo cáo chuyên cần Mầm 1 đã duyệt, sau đó một điều chỉnh tại `UC-14` được duyệt | Báo cáo chuyển `Cần cập nhật`. Bảng tổng hợp vẫn dùng phiên bản đã duyệt và đánh dấu đang cập nhật. |
| Tổng hợp tháng 09/2026 | 4 nhóm báo cáo nguồn đã duyệt ([Mẫu 05](../../survey/forms/mau-05-bao-cao-thang.md)) | Sĩ số cuối kỳ 135, chuyên cần 2.548 / 2.693 = 94,6%, còn phải thu 7.300.000 đồng. |
| Kỳ chưa đầy đủ | Kế toán chưa gửi báo cáo tháng | Bảng tổng hợp đánh dấu kỳ chưa đầy đủ (QT-BC-05). |

## Liên phân hệ và truy vết

- **F1.0 → NV09:** Danh mục loại báo cáo, kỳ, hạn gửi, chỉ tiêu từ `UC-02` (YC-BC-05, QT-BC-06).
- **TV1 → NV09:** Tuyển sinh từ `UC-11`, bảng sĩ số đầu kỳ, tăng, giảm, cuối kỳ từ `UC-17` (YC-XL-04), chuyên cần đã khóa từ `UC-13`, suất ăn từ `UC-15`.
- **TV2 → NV09:** Nhân sự từ [UC-04](uc-04-lap-va-cap-nhat-ho-so-nhan-su.md), phân công từ [UC-05](uc-05-lap-phan-cong-giao-vien.md), thu học phí từ [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md) và [UC-07](uc-07-ghi-thu-tien-va-in-bien-lai.md), đến muộn và trả muộn từ [UC-08](uc-08-don-tre-theo-nhom.md), [UC-09](uc-09-tra-tre-theo-nhom.md).
- **Thay đổi nguồn → NV09:** Hồi tố tại `UC-12`, `UC-17`, duyệt điều chỉnh sau khóa tại `UC-14`, điều chỉnh hoặc đối chiếu suất ăn tại `UC-15` kích hoạt cảnh báo và trạng thái `Cần cập nhật` (QT-BC-04).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-BC-01 | QT-BC-01, QT-BC-02, QT-BC-06 | Mọi phân hệ → NV09 · Báo cáo của tôi: lập từ số liệu đã chốt, ghi chú, gửi. |
| YC-BC-02 | QT-BC-01, QT-BC-03 | NV09 · Hộp báo cáo nhận được: xem, phản hồi, yêu cầu bổ sung, duyệt. |
| YC-BC-03 | QT-BC-03, QT-BC-04, QT-BC-05 | NV02, NV03, NV07 → NV09 · Phiên bản, ảnh chụp số liệu, `Cần cập nhật` khi nguồn đổi. |
| YC-BC-04 | QT-BC-05, QT-BC-06 | NV09 · Hạn gửi, trễ hạn, tổng hợp toàn trường từ bản đã duyệt. |
| YC-BC-05 | QT-BC-06 | F1.0 → NV09 · Danh mục loại báo cáo do Ban giám hiệu quản lý. |
