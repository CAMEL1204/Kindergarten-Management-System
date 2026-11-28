# Truy vết nghiệp vụ – yêu cầu – quy tắc – Use Case

TV2 tổng hợp ma trận này theo phân công ở [README](README.md). Mỗi thành viên cung cấp các dòng truy vết cho phần mình. Phần 1–3 của TV1 (nền tảng `F1.0` với `UC-01`–`UC-03`, phân hệ TV1 với `UC-11`–`UC-18` và phần bàn giao cho TV2) nằm trên nhánh `docs/analysis/member-1` và được ghép vào file này khi hai nhánh vào `main`. Phần 4–6 dưới đây là phần của TV2.

## Phần 4 · Phân hệ TV2 (`NV04`, `NV05`, `NV06`, `NV08`, `NV09`, `UC-04` đến `UC-10`)

Cách ghi giống phần 2 của TV1. Cột **Quy tắc thực thi trực tiếp** là quy tắc của chính nghiệp vụ được kiểm tra trong giao dịch. Cột **Ràng buộc liên phân hệ** là quy tắc của nghiệp vụ khác được viện dẫn.

| NV | Chức năng | Yêu cầu | Quy tắc thực thi trực tiếp | Ràng buộc liên phân hệ | Use Case | Phạm vi và luồng liên phân hệ |
| --- | --- | --- | --- | --- | --- | --- |
| NV04 | F1.4.1 | YC-NS-01 | QT-NS-01 | — | [UC-04 · Lập và cập nhật hồ sơ nhân sự](use-cases/uc-04-lap-va-cap-nhat-ho-so-nhan-su.md) | Tra trùng, dùng lại hồ sơ cũ, lập hồ sơ kèm chi tiết trong một giao dịch, Ban giám hiệu xác nhận. |
| NV04 | F1.4.2 | YC-NS-02 | QT-NS-02, QT-NS-05 | — | [UC-04 · Lập và cập nhật hồ sơ nhân sự](use-cases/uc-04-lap-va-cap-nhat-ho-so-nhan-su.md) | Bằng cấp, chứng chỉ, giấy tờ trên một màn hình. Cảnh báo chứng chỉ từ 30 ngày trước hạn. |
| NV04 | F1.4.3 | YC-NS-03, YC-NS-05 | QT-NS-03, QT-NS-04 | QT-PC-06 | [UC-04 · Lập và cập nhật hồ sơ nhân sự](use-cases/uc-04-lap-va-cap-nhat-ho-so-nhan-su.md) | NV04 → NV05, F1.0 · Tạm nghỉ, nghỉ việc, chuyển vị trí theo ngày hiệu lực. Báo lớp bị ảnh hưởng cho `UC-05`, nhắc khóa và mở tài khoản tại `UC-01`. |
| NV04 | F1.4.4 | YC-NS-03, YC-NS-04 | QT-NS-02, QT-NS-04 | QT-PC-01 | [UC-04 · Lập và cập nhật hồ sơ nhân sự](use-cases/uc-04-lap-va-cap-nhat-ho-so-nhan-su.md) | NV04 → NV05, NV09 · Danh sách đủ điều kiện đứng lớp tại một ngày. Số liệu báo cáo nhân sự. |
| NV05 | F1.5.1 | YC-PC-01, YC-PC-02 | QT-PC-01, QT-PC-02, QT-PC-03, QT-PC-04 | QT-NS-02 | [UC-05 · Lập phân công giáo viên](use-cases/uc-05-lap-phan-cong-giao-vien.md) | NV02, NV04, F1.0 → NV05 · Lớp đang hoạt động từ `UC-12`, `UC-17`, nhân sự đủ điều kiện từ `UC-04`, ca và định mức từ `UC-02`. Thay phân công nguyên tử. |
| NV05 | F1.5.2 | YC-PC-02 | QT-PC-03, QT-PC-04 | — | [UC-05 · Lập phân công giáo viên](use-cases/uc-05-lap-phan-cong-giao-vien.md) | Ban giám hiệu duyệt, người lập khác người duyệt, chốt ngày hiệu lực. |
| NV05 | F1.5.3 | YC-PC-03 | QT-PC-02, QT-PC-05, QT-PC-06 | QT-NS-03, QT-DT-05 | [UC-05 · Lập phân công giáo viên](use-cases/uc-05-lap-phan-cong-giao-vien.md) | NV04 → NV05 → NV03, NV08 · Đơn nghỉ ngắn ngày, dạy thay. Danh sách giáo viên phụ trách lớp theo ngày cho `UC-08`, `UC-09`, `UC-13`, `UC-15`. |
| NV05 | F1.5.4 | YC-PC-04 | QT-PC-04 | — | [UC-05 · Lập phân công giáo viên](use-cases/uc-05-lap-phan-cong-giao-vien.md) | NV05 → NV09 · Lịch sử phân công, tổng hợp ca đứng lớp và dạy thay theo tháng. |
| NV06 | F1.6.1 | YC-HP-01 | QT-HP-01, QT-HP-03 | — | `UC-02` (TV1 đặc tả màn hình danh mục) | F1.0 → NV06 · Kế toán lập biểu phí, miễn giảm. Ban giám hiệu duyệt. `UC-06` chỉ dùng bản đã duyệt. |
| NV06 | F1.6.2 | YC-HP-02, YC-HP-05, YC-HP-06 | QT-HP-01, QT-HP-02, QT-HP-03, QT-HP-05, QT-HP-06 | QT-DD-07, QT-XL-05 | [UC-06 · Lập bảng phải thu theo kỳ](use-cases/uc-06-lap-bang-phai-thu-theo-ky.md) | NV01, NV02, NV03 → NV06 · Khoản tháng theo tỷ lệ, tạm thu tiền ăn, đối soát và bù trừ có tham chiếu kỳ phát sinh, treo khi còn đề nghị chờ duyệt, đối soát lại khi NV02 ghi hồi tố. |
| NV06 | F1.6.3 | YC-HP-03 | QT-HP-04, QT-HP-07 | — | [UC-07 · Ghi thu tiền và in biên lai](use-cases/uc-07-ghi-thu-tien-va-in-bien-lai.md) | Phân bổ kỳ cũ trước, số dư có, một biên lai duy nhất cho mỗi lần thu. |
| NV06 | F1.6.4 | YC-HP-04 | QT-HP-04, QT-HP-05, QT-HP-07 | — | [UC-07 · Ghi thu tiền và in biên lai](use-cases/uc-07-ghi-thu-tien-va-in-bien-lai.md) | NV02 → NV06 · Phiếu điều chỉnh, hoàn tiền có duyệt. Trẻ `Thôi học` từ `UC-17` kích hoạt xử lý số dư có và chênh lệch tiền ăn. |
| NV06 | F1.6.5 | YC-HP-04 | QT-HP-04 | — | [UC-07 · Ghi thu tiền và in biên lai](use-cases/uc-07-ghi-thu-tien-va-in-bien-lai.md) | NV06 → NV09 · Công nợ theo trẻ, lớp, kỳ, nhắc nợ, số liệu báo cáo thu. |
| NV08 | F1.8.1 | YC-DT-05 | QT-DT-06 | QT-DD-02, QT-SA-04 | [UC-08 · Đón trẻ theo nhóm](use-cases/uc-08-don-tre-theo-nhom.md) | NV08 → NV03, NV07 · Báo nghỉ tại Giáo viên và Văn phòng, làm căn cứ xác minh vắng và danh sách ăn dự kiến. |
| NV08 | F1.8.2 | YC-DT-01, YC-DT-02, YC-DT-04 | QT-DT-01, QT-DT-03, QT-DT-05, QT-DT-06 | QT-DD-03, QT-DD-05 | [UC-08 · Đón trẻ theo nhóm](use-cases/uc-08-don-tre-theo-nhom.md) | NV02, NV05, F1.0 → NV08 → NV03 · Đón nhiều trẻ một lần. Trước mốc khóa cập nhật `UC-13`, từ mốc khóa chuyển `UC-14`. |
| NV08 | F1.8.3 | YC-DT-07 | QT-DT-04, QT-DT-05 | QT-HS-05 | [UC-09 · Trả trẻ theo nhóm](use-cases/uc-09-tra-tre-theo-nhom.md) | NV01 → NV08 · Xác minh và duyệt nhận trẻ hộ cho một lượt, quyền duyệt cấp riêng tại `UC-01`. |
| NV08 | F1.8.4 | YC-DT-02, YC-DT-03, YC-DT-04 | QT-DT-02, QT-DT-03, QT-DT-04, QT-DT-05 | QT-HS-05 | [UC-09 · Trả trẻ theo nhóm](use-cases/uc-09-tra-tre-theo-nhom.md) | NV01 → NV08 → NV03, NV07 · Kiểm tra từng dòng, ghi dòng hợp lệ, giữ dòng bị chặn. Giờ về thực tế cho `UC-13`, `UC-15`. |
| NV08 | F1.8.5 | YC-DT-06 | QT-DT-05 | — | [UC-08](use-cases/uc-08-don-tre-theo-nhom.md), [UC-09](use-cases/uc-09-tra-tre-theo-nhom.md) | NV08 → NV09 · Tra cứu theo trẻ, lớp, ngày, người nhận. Số lượt đến muộn, trả theo ngoại lệ, trả muộn. |
| NV08 | F1.8.6 | YC-DT-08 | QT-DT-07 | QT-DD-03, QT-SA-04 | [UC-08](use-cases/uc-08-don-tre-theo-nhom.md), [UC-09](use-cases/uc-09-tra-tre-theo-nhom.md) | NV08 → NV03, NV07 · Hủy lượt đón theo mốc khóa, hủy lượt trả có duyệt, không xóa cứng. |
| NV09 | F1.9.1 | YC-BC-01 | QT-BC-01, QT-BC-02, QT-BC-06 | QT-DD-07 | [UC-10 · Gửi, phản hồi và duyệt báo cáo](use-cases/uc-10-gui-phan-hoi-va-duyet-bao-cao.md) | Mọi phân hệ → NV09 · Lập từ số liệu đã chốt trong phạm vi, đánh dấu kỳ còn ngày chưa khóa hoặc đề nghị chờ duyệt. |
| NV09 | F1.9.2 | YC-BC-01, YC-BC-03 | QT-BC-03, QT-BC-04, QT-BC-06 | — | [UC-10 · Gửi, phản hồi và duyệt báo cáo](use-cases/uc-10-gui-phan-hoi-va-duyet-bao-cao.md) | Ảnh chụp số liệu tại lúc gửi, khóa nội dung, phiên bản, cờ trễ hạn. |
| NV09 | F1.9.3 | YC-BC-02, YC-BC-03 | QT-BC-01, QT-BC-03, QT-BC-04 | QT-XL-05, QT-DD-03, QT-SA-04 | [UC-10 · Gửi, phản hồi và duyệt báo cáo](use-cases/uc-10-gui-phan-hoi-va-duyet-bao-cao.md) | NV02, NV03, NV07 → NV09 · Phản hồi, yêu cầu bổ sung, duyệt. Hồi tố, điều chỉnh sau khóa hoặc điều chỉnh suất ăn chuyển báo cáo đã duyệt sang `Cần cập nhật`. |
| NV09 | F1.9.4 | YC-BC-04 | QT-BC-05, QT-BC-06 | — | [UC-10 · Gửi, phản hồi và duyệt báo cáo](use-cases/uc-10-gui-phan-hoi-va-duyet-bao-cao.md) | Tổng hợp toàn trường từ phiên bản đã duyệt mới nhất, đánh dấu kỳ chưa đầy đủ hoặc đang cập nhật. |

## Phần 5 · Các luồng liên phân hệ đã chốt chung

Mỗi luồng ghi rõ nghiệp vụ cung cấp, nghiệp vụ sử dụng, điều kiện dữ liệu hợp lệ và cách xử lý khi dữ liệu thay đổi, theo bước 2 ở [README](README.md). Bên sử dụng chỉ đọc dữ liệu đã chốt, không sửa (README khảo sát §4, §6).

| Luồng | Cung cấp | Sử dụng | Điều kiện dữ liệu hợp lệ | Xử lý khi dữ liệu thay đổi |
| --- | --- | --- | --- | --- |
| NV08 Đón trẻ → NV03 Chuyên cần | `UC-08` (TV2) | `UC-13`, `UC-14` (TV1) | Lượt đón chưa hủy, ghi bởi người có quyền trên lớp trong ngày. Phân loại theo thời điểm ghi nhận so với mốc khóa có hiệu lực (mặc định 09:00). Cờ `Đến muộn` theo giờ đến thực tế so với mốc bàn giao (mặc định 08:30). | Trước mốc khóa: cập nhật trực tiếp hoặc tính lại khi hủy. Từ mốc khóa: phát sinh hoặc hủy lượt đón tạo đề nghị điều chỉnh tại `UC-14`, kết quả đã khóa giữ tới khi duyệt (QT-DT-01, QT-DT-07, QT-DD-03). |
| NV01 Hồ sơ trẻ → NV08 Trả trẻ | `UC-16` (TV1) | `UC-09` (TV2) | Người giám hộ chính đã xác nhận hoặc ủy quyền cố định còn hiệu lực tại giờ giao (lưu `Đến ngày + 1`). Người ngoài danh sách cần xác nhận của người giám hộ, đối chiếu CCCD và duyệt ngoại lệ cho từng lượt. | Thu hồi ủy quyền có hiệu lực ngay, dòng trả đang mở bị chặn khi lưu. Ủy quyền hết hạn chuyển sang luồng nhận trẻ hộ (QT-HS-05, QT-DT-04). |
| NV02 Xếp lớp → NV05 Phân công | `UC-12`, `UC-17` (TV1) | `UC-05` (TV2) | Lớp đang hoạt động và sĩ số tại ngày hiệu lực. Phân công chỉ vào lớp đang hoạt động (QT-PC-01). | Khi lớp đóng hoặc sĩ số đổi, `UC-05` đánh dấu phân công bị ảnh hưởng. NV02 kiểm tra cảnh báo định mức của lớp đích khi xếp, quay lại hoặc chuyển lớp (QT-PC-03). |
| NV05 Phân công → NV03, NV08 | `UC-05` (TV2) | `UC-13` (TV1), `UC-08`, `UC-09` (TV2) | Giáo viên có dòng phân công hoặc dạy thay có hiệu lực trong ngày, không có đơn nghỉ đã duyệt, không tạm nghỉ. Nhân sự trực có quyền NV08 theo lớp và thời gian (QT-DT-05). | Thay phân công theo ngày hiệu lực: ngày trước giữ quyền giáo viên cũ, từ ngày hiệu lực chuyển giáo viên mới (QT-PC-04). Đơn nghỉ được duyệt rút quyền trong những ngày nghỉ (QT-PC-06). |
| NV04 Nhân sự → NV05, F1.0 | `UC-04` (TV2) | `UC-05` (TV2), `UC-01` (TV1) | Nhân sự `Đang làm việc`, được giao đứng lớp hoặc dạy thay, có trình độ chuẩn (QT-NS-02). | Tạm nghỉ, nghỉ việc loại nhân sự khỏi danh sách đủ điều kiện, báo lớp bị ảnh hưởng và nhắc khóa tài khoản. Hết tạm nghỉ nhắc mở lại (QT-NS-03, YC-NS-05). |
| NV02, NV03 → NV06 Học phí | `UC-12`, `UC-17`, `UC-13`, `UC-14` (TV1) | `UC-06`, `UC-07` (TV2) | Khoảng thuộc diện đi học theo ngày. Số ngày `Có mặt` đã khóa, gồm điều chỉnh đã duyệt. `Chờ xác minh` không có lượt đón tính 0 ngày (QT-HP-02, QT-DD-07). | Treo đối soát của trẻ khi còn đề nghị chờ duyệt có thể đổi `Có mặt`, gỡ treo khi không còn. Hồi tố ở NV02 tạo đối soát lại. Chênh lệch bù trừ ở kỳ sau, trẻ thôi học chuyển điều chỉnh hoặc hoàn tiền (QT-HP-05, QT-XL-05). |
| Các phân hệ → NV09 Báo cáo | Mọi UC có số liệu đã chốt | `UC-10` (TV2) | Số liệu đã chốt trong phạm vi người lập, chụp tại lúc gửi (QT-BC-02, QT-BC-04). Tổng hợp toàn trường chỉ dùng phiên bản đã duyệt mới nhất (QT-BC-05). | Báo cáo `Đã gửi` không tự đổi, Ban giám hiệu xử lý như yêu cầu bổ sung. Báo cáo `Đã duyệt` chuyển sang `Cần cập nhật`, bảng tổng hợp đánh dấu đang cập nhật (QT-BC-04). |

## Phần 6 · Đối chiếu checklist phần TV2

| Tiêu chí | Yêu cầu của checklist | Phần TV2 |
| --- | --- | --- |
| Số use case | ≥8 cho nhóm, mỗi thành viên ≥4 | 7 use case: `UC-04` đến `UC-10`. |
| Giao dịch phức hợp | ≥4 cho nhóm | 7: hồ sơ kèm chi tiết (`UC-04`), kết thúc phân công cũ và mở mới (`UC-05`), lập phải thu nhiều trẻ (`UC-06`), phân bổ thanh toán và biên lai (`UC-07`), đón theo nhóm (`UC-08`), trả theo nhóm kiểm tra từng dòng (`UC-09`), phiên bản và ảnh chụp số liệu (`UC-10`). Các giao dịch ghi nhiều bản ghi tuân theo NFR-05. |
| Use case liên phân hệ TV1↔TV2 | ≥2 | 5: `UC-05`, `UC-06`, `UC-08`, `UC-09`, `UC-10`. |
| Quy tắc nghiệp vụ | ≥8 cho nhóm, mỗi thành viên ≥4 | 31 quy tắc: QT-NS-01–05, QT-PC-01–06, QT-HP-01–07, QT-DT-01–07, QT-BC-01–06. |
| Quy tắc liên phân hệ TV1↔TV2 | ≥2 | QT-PC-01, QT-HP-02, QT-HP-05, QT-DT-01, QT-DT-04, QT-DT-05. |
| Nhánh cấp 2 của BFD | ≥6 cho nhóm, mỗi thành viên ≥3 | 5 nhánh: F1.4, F1.5, F1.6, F1.8, F1.9 ([BFD phần TV2](diagrams/bfd-tv2.md)). |
