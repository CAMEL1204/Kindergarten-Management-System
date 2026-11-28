# Kế hoạch phân tích hệ thống quản lý nhà trẻ

Giai đoạn này chuyển kết quả khảo sát của nền tảng `F1.0` và 9 nghiệp vụ thành đặc tả use case và mô hình phân tích để làm đầu vào cho thiết kế. Phạm vi, mã yêu cầu `YC-*`, mã quy tắc `QT-*` và danh mục chức năng lá `F1.x.y` lấy từ [hồ sơ khảo sát](../survey/README.md).

## 1. Phân công

| Đầu việc | Phụ trách | Review | Đầu ra | Trạng thái |
| --- | --- | --- | --- | --- |
| Đặc tả nền tảng `F1.0` (`UC-01`–`UC-03`) và `NV01`, `NV02`, `NV03`, `NV07` (`UC-11`–`UC-18`) | TV1 | TV2 | Mười một đặc tả `UC-01`–`UC-03` và `UC-11`–`UC-18` tại mục 5. | Hoàn thành |
| Phân tích `NV04` Nhân sự, `NV05` Phân công, `NV06` Học phí, `NV08` Đón trả trẻ, `NV09` Báo cáo (`UC-04`–`UC-10`) | TV2 | TV1 | Bảy đặc tả `UC-04`–`UC-10` tại mục 6 và [BFD phần TV2](diagrams/bfd-tv2.md). | Đủ 7 UC và BFD phần TV2, chờ TV1 review qua pull request |
| Tổng hợp sơ đồ Use Case toàn hệ thống | TV1 | TV2 | [Sơ đồ 18 UC thuộc F1.0 và 9 nghiệp vụ](diagrams/use-case-toan-he-thong.md) | Hoàn thành |
| Tổng hợp ma trận truy vết và đối chiếu checklist | TV1 (phần 1–3: `F1.0` và `UC-11`–`UC-18`), TV2 (phần 4–6: `UC-04`–`UC-10`) | Chéo | [Ma trận truy vết](traceability.md) | Đã ghép phần của cả hai thành viên |

## 2. Các luồng liên phân hệ đã chốt chung

| Nghiệp vụ | Quy tắc thống nhất cho cả TV1 và TV2 |
| --- | --- |
| `F1.0` Quản trị hệ thống → Tất cả | `UC-01` cấp tài khoản gắn hồ sơ nhân sự `NV04`, khóa/mở theo biến động `Tạm nghỉ`/`Nghỉ việc` (`QT-NS-03`), và cấp tách biệt quyền trực với quyền duyệt ngoại lệ `NV08` (`QT-DT-05`). `UC-02` quản trị năm học, kỳ học, lịch nghỉ, nhóm tuổi, lớp, ca, định mức, biểu phí, miễn giảm và loại báo cáo. `UC-03` cấu hình động các mốc giờ vận hành có hiệu lực (mặc định `07:30`, `08:30`, `09:00`, khung đón/trả), bữa ăn, định mức khẩu phần và giờ chốt bữa (`YC-SA-03`). |
| `NV08` Đón trẻ → `NV03` Chuyên cần | Trên `UC-08`, đồng bộ 3 vai trò theo `QT-DT-05` và `YC-DT-05`: `Giáo viên` hoặc `Nhân sự trực được cấp quyền` ghi lượt đón; `Giáo viên` hoặc `Văn phòng` tiếp nhận báo nghỉ. Lượt đón tạo kết quả `Có mặt` trực tiếp trước **mốc khóa có hiệu lực của ngày học** (mặc định `09:00`) theo **thời điểm ghi nhận**. Cờ `Đến muộn` tính từ **mốc bàn giao sáng có hiệu lực** (mặc định `08:30`) theo **giờ đến thực tế**. Từ mốc khóa có hiệu lực trở đi, phát sinh hoặc hủy lượt đón chuyển sang đề nghị điều chỉnh có duyệt tại `UC-14`. |
| `NV01` Hồ sơ trẻ → `NV08` Trả trẻ | `UC-09` tra người giám hộ chính và ủy quyền cố định còn hiệu lực (`Đến ngày + 1`, hoặc hết hiệu lực tức thời khi thu hồi sớm tại `UC-16`) tại thời điểm trả (`QT-HS-05`, `QT-DT-04`), che CCCD trên danh sách thông thường. |
| `NV02` Xếp lớp → `NV05` Phân công | `UC-12` và `UC-17` cung cấp danh sách lớp đang hoạt động và sĩ số theo ngày hiệu lực cho `UC-05` (`QT-PC-01`), đồng thời kiểm tra cảnh báo định mức giáo viên `NV05` của lớp đích khi xếp lớp, quay lại hoặc chuyển lớp (`QT-PC-03`). |
| `NV02` Xếp lớp và `NV03` Chuyên cần → `NV06` Học phí | `UC-17`/`UC-12` cung cấp diện đi học theo ngày (`Nghỉ dài ngày` thu đủ khoản tháng; `Bảo lưu`/`Thôi học` tính tỷ lệ theo `QT-HP-06`). `UC-13`/`UC-14` cung cấp số ngày `Có mặt` đã khóa cuối kỳ; `Chờ xác minh` tính 0 ngày (`QT-DD-07`). **Treo đối soát của trẻ trong kỳ khi trẻ còn ngày chưa khóa hoặc còn ít nhất một đề nghị chờ duyệt có thể đổi kết quả `Có mặt`. Chỉ gỡ treo khi mọi ngày liên quan của trẻ trong kỳ đã khóa và không còn đề nghị nào thỏa điều kiện đó** (`QT-DD-07`, `QT-HP-05`). Xếp lớp hồi tố hoặc đổi nhóm tuổi thông báo đối soát lại sang `NV06`. |
| Các phân hệ → `NV09` Báo cáo | `UC-10` nhận bảng tổng hợp sĩ số đầu kỳ/tăng/giảm/cuối kỳ từ `UC-17` (`YC-XL-04`), số liệu tuyển sinh từ `UC-11`, chuyên cần từ `UC-13` và suất ăn từ `UC-15`. Khi **bất kỳ thay đổi nguồn nào** ảnh hưởng đến báo cáo đã nộp (`UC-12`/`UC-17` hồi tố, `UC-14` duyệt sau khóa, hoặc điều chỉnh/đối chiếu suất ăn `UC-15`), hệ thống giữ nguyên ảnh chụp và cảnh báo người lập theo `QT-BC-04`: báo cáo ở trạng thái `Đã gửi` do Ban giám hiệu xử lý yêu cầu bổ sung; báo cáo ở trạng thái `Đã duyệt` chuyển sang `Cần cập nhật`. |

Bảng chi tiết từng luồng (bên cung cấp, bên sử dụng, điều kiện dữ liệu hợp lệ, xử lý khi dữ liệu thay đổi) ở phần 5 của [ma trận truy vết](traceability.md).

## 3. Thứ tự thực hiện

1. **Chốt danh mục use case và mẫu đặc tả.** Dùng đầy đủ dải mã khép kín `UC-01` đến `UC-18`: `UC-01`–`UC-03` cho `F1.0`, giữ nguyên `UC-04`–`UC-10` cho TV2 theo khảo sát và `UC-11`–`UC-18` cho TV1. Mỗi đặc tả có tác nhân, mục tiêu, điều kiện trước và sau, luồng chính, luồng thay thế hoặc ngoại lệ, dữ liệu vào và ra, mã `F1.x.y`, `YC-*` và `QT-*` liên quan.
2. **Thống nhất các luồng liên phân hệ rồi viết song song.** TV2 bám sát mục 2 và phần 3 của [ma trận truy vết](traceability.md) khi viết `UC-04`–`UC-10` (ưu tiên `UC-05`, `UC-06`, `UC-08`, `UC-09`, `UC-10`), review chéo qua pull request.
3. **Tổng hợp và rà soát.** Đối chiếu đặc tả với sơ đồ, ma trận truy vết và checklist của giảng viên. Theo mục 8 của [hồ sơ khảo sát](../survey/README.md), nhóm cần tối thiểu 8 use case, mỗi người ít nhất 4, có ít nhất 4 giao dịch phức hợp và 2 luồng liên phân hệ; quy tắc nghiệp vụ tối thiểu 8 quy tắc, mỗi người ít nhất 4 và có ít nhất 2 quy tắc liên phân hệ.

## 4. Vị trí tài liệu

| Đường dẫn tính từ `docs/analysis/` | Nội dung |
| --- | --- |
| `README.md` | Phân công, tiến độ, quy ước liên phân hệ và mục lục. |
| `use-cases/` | Mỗi use case một file Markdown (`uc-01-...md` đến `uc-18-...md`). |
| `diagrams/` | Sơ đồ use case toàn hệ thống, [BFD toàn hệ thống](diagrams/bfd-toan-he-thong.md) ba cấp và BFD chi tiết phần TV2. |
| `traceability.md` | Ma trận nối mã NV, chức năng `F1.x.y`, `YC`, `QT` và `UC`, kèm luồng liên phân hệ. |

## 5. Danh mục đặc tả nền tảng (`F1.0`) và TV1

| Phân hệ / Nghiệp vụ | Chức năng lá | Tài liệu |
| --- | --- | --- |
| F1.0 | F1.0.1 | [UC-01 · Quản lý tài khoản và phân quyền trực](use-cases/uc-01-quan-ly-tai-khoan-va-phan-quyen.md) |
| F1.0 | F1.0.2, F1.6.1 | [UC-02 · Quản lý danh mục dùng chung](use-cases/uc-02-quan-ly-danh-muc-dung-chung.md) |
| F1.0 | F1.0.3 | [UC-03 · Cấu hình khung giờ vận hành, bữa ăn và mốc chốt](use-cases/uc-03-cau-hinh-moc-gio-va-bua-an.md) |
| NV01 | F1.1.1, F1.1.2 | [UC-11 · Tiếp nhận hồ sơ trẻ](use-cases/uc-11-tiep-nhan-ho-so-tre.md) |
| NV01 | F1.1.3, F1.1.4, F1.1.5 | [UC-16 · Cập nhật hồ sơ trẻ, ủy quyền nhận trẻ và lưu ý chăm sóc](use-cases/uc-16-cap-nhat-ho-so-va-luu-y-cham-soc.md) |
| NV02 | F1.2.2, F1.2.4 | [UC-12 · Chuyển lớp](use-cases/uc-12-chuyen-lop.md) |
| NV02 | F1.2.1, F1.2.3, F1.2.4 | [UC-17 · Xếp lớp ban đầu và quản lý biến động sĩ số](use-cases/uc-17-xep-lop-va-bien-dong-si-so.md) |
| NV03 | F1.3.1, F1.3.2, F1.3.4 | [UC-13 · Chốt chuyên cần theo lớp và ngày](use-cases/uc-13-chot-chuyen-can.md) |
| NV03 | F1.3.3 | [UC-14 · Điều chỉnh chuyên cần sau khóa](use-cases/uc-14-dieu-chinh-chuyen-can-sau-khoa.md) |
| NV07 | F1.7.3, F1.7.4 | [UC-15 · Chốt và đối chiếu suất ăn theo bữa](use-cases/uc-15-chot-va-doi-chieu-suat-an.md) |
| NV07 | F1.7.1, F1.7.2 | [UC-18 · Lập, duyệt và thông báo thực đơn tuần](use-cases/uc-18-lap-duyet-va-thong-bao-thuc-don.md) |

## 6. Danh mục đặc tả của TV2

Thứ tự theo mục 3: nhóm ưu tiên `UC-05`, `UC-06`, `UC-08`, `UC-09`, rồi `UC-04`, `UC-07`, `UC-10`.

| Nghiệp vụ | Chức năng lá | Tài liệu |
| --- | --- | --- |
| NV05 | F1.5.1–F1.5.4 | [UC-05 · Lập phân công giáo viên](use-cases/uc-05-lap-phan-cong-giao-vien.md) |
| NV06 | F1.6.2 | [UC-06 · Lập bảng phải thu theo kỳ](use-cases/uc-06-lap-bang-phai-thu-theo-ky.md) |
| NV08 | F1.8.1, F1.8.2, F1.8.5, F1.8.6 | [UC-08 · Đón trẻ theo nhóm](use-cases/uc-08-don-tre-theo-nhom.md) |
| NV08 | F1.8.3–F1.8.6 | [UC-09 · Trả trẻ theo nhóm](use-cases/uc-09-tra-tre-theo-nhom.md) |
| NV04 | F1.4.1–F1.4.4 | [UC-04 · Lập và cập nhật hồ sơ nhân sự](use-cases/uc-04-lap-va-cap-nhat-ho-so-nhan-su.md) |
| NV06 | F1.6.3–F1.6.5 | [UC-07 · Ghi thu tiền và in biên lai](use-cases/uc-07-ghi-thu-tien-va-in-bien-lai.md) |
| NV09 | F1.9.1–F1.9.4 | [UC-10 · Gửi, phản hồi và duyệt báo cáo](use-cases/uc-10-gui-phan-hoi-va-duyet-bao-cao.md) |

Sơ đồ dùng chung của TV2: [BFD phần TV2](diagrams/bfd-tv2.md), chi tiết hóa 5 nhánh cấp 2 và 23 chức năng lá.
