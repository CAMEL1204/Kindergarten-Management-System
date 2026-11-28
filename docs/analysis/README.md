# Kế hoạch phân tích hệ thống quản lý nhà trẻ

Giai đoạn này chuyển kết quả khảo sát của 9 nghiệp vụ thành đặc tả use case và mô hình phân tích để làm đầu vào cho thiết kế. Phạm vi, mã yêu cầu `YC-*`, mã quy tắc `QT-*` và danh mục chức năng `F1.x.y` lấy từ [hồ sơ khảo sát](../survey/README.md).

## 1. Phân công

| Đầu việc | Phụ trách | Review | Đầu ra | Trạng thái |
| --- | --- | --- | --- | --- |
| Phân tích NV01 Hồ sơ trẻ, NV02 Xếp lớp, NV03 Chuyên cần, NV07 Suất ăn | TV1 | TV2 | Đặc tả tối thiểu 4 use case. Ưu tiên tiếp nhận hồ sơ trẻ, chuyển lớp, chốt chuyên cần, chốt suất ăn. Mã UC của nhóm này chốt ở bước 1. | Chưa bắt đầu |
| Phân tích NV04 Nhân sự, NV05 Phân công, NV06 Học phí, NV08 Đón trả trẻ, NV09 Báo cáo | TV2 | TV1 | Đặc tả các UC đã liệt kê tại mục 8.2 của hồ sơ khảo sát. Làm trước UC-05, UC-06, UC-08 và UC-09 vì các luồng này cần dữ liệu từ TV1. Tiếp theo là UC-04, UC-07 và UC-10. Danh mục ở mục 5. | Đã có bản nháp đủ 7 UC, chờ TV1 review |
| Tổng hợp sơ đồ use case toàn hệ thống và kiểm tra tên tác nhân theo từng phân hệ | TV1 | TV2 | Sơ đồ lưu trong `diagrams/`, dùng chung mã với các đặc tả. | Chưa bắt đầu |
| Tổng hợp ma trận truy vết và đối chiếu checklist nhóm hai người | TV2 | TV1 | [`traceability.md`](traceability.md) liên kết nghiệp vụ, yêu cầu, quy tắc và use case. Mỗi người cung cấp các dòng truy vết cho phần mình phụ trách. | Đã có phần TV2 (phần 4–6), chờ ghép phần TV1 |

## 2. Các luồng cần chốt chung

| Nghiệp vụ | Hai bên cần thống nhất |
| --- | --- |
| NV08 Đón trẻ → NV03 Chuyên cần | Lượt đón tạo kết quả có mặt. Xác định thời điểm khóa và cách chuyển phát sinh sau khóa sang luồng đề nghị điều chỉnh. |
| NV01 Hồ sơ trẻ → NV08 Trả trẻ | Kiểm tra người được nhận trẻ và hiệu lực ủy quyền tại thời điểm trả. Xác định cách ghi và duyệt ngoại lệ. |
| NV02 Xếp lớp → NV05 Phân công | Xác định danh sách lớp đủ điều kiện theo ngày hiệu lực và dữ liệu phân công cần kiểm tra khi thao tác trên lớp. |
| NV02 Xếp lớp và NV03 Chuyên cần → NV06 Học phí | Thống nhất dữ liệu tạm thu và dữ liệu đối soát cuối kỳ. Khoản tiền ăn đối soát theo số ngày có mặt đã xác nhận. |
| Các phân hệ → NV09 Báo cáo | Chốt nguồn, kỳ và trạng thái dữ liệu được dùng để tổng hợp báo cáo. |

Đề xuất của TV2 cho từng luồng (bên cung cấp, bên sử dụng, điều kiện dữ liệu hợp lệ, xử lý khi dữ liệu thay đổi) ở phần 5 của [ma trận truy vết](traceability.md).

## 3. Thứ tự thực hiện

1. **Chốt danh mục use case và mẫu đặc tả.** Giữ mã `UC-04` đến `UC-10` đã có trong khảo sát. Sau khi đối chiếu danh mục, cấp mã còn trống cho các use case mới. Mỗi đặc tả có tác nhân, mục tiêu, điều kiện trước và sau, luồng chính, luồng thay thế hoặc ngoại lệ, dữ liệu vào và ra, mã `YC-*` và `QT-*` liên quan.
2. **Thống nhất các luồng liên phân hệ rồi viết song song.** Với mỗi luồng, ghi rõ nghiệp vụ cung cấp dữ liệu, nghiệp vụ sử dụng, điều kiện dữ liệu hợp lệ và cách xử lý khi dữ liệu thay đổi. TV1 và TV2 làm trên `docs/analysis/member-1` và `docs/analysis/member-2`, review chéo qua pull request.
3. **Tổng hợp và rà soát.** Đối chiếu đặc tả với sơ đồ, ma trận truy vết và checklist của giảng viên. Theo mục 8 của [hồ sơ khảo sát](../survey/README.md), nhóm cần tối thiểu 8 use case, mỗi người ít nhất 4, có ít nhất 4 giao dịch phức hợp và 2 luồng liên phân hệ. Với quy tắc nghiệp vụ, kiểm tra tối thiểu 8 quy tắc, mỗi người ít nhất 4 và có ít nhất 2 quy tắc liên phân hệ.

## 4. Vị trí tài liệu

| Đường dẫn tính từ `docs/analysis/` | Nội dung |
| --- | --- |
| `README.md` | Phân công, tiến độ và mục lục. |
| `use-cases/` | Mỗi use case một file Markdown. Ví dụ `use-cases/uc-08-don-tre-theo-nhom.md`. |
| `diagrams/` | Sơ đồ dùng chung. Sơ đồ riêng của một use case có thể đặt ngay trong đặc tả. |
| `traceability.md` | Ma trận nối mã NV, YC, QT và UC, kèm đánh dấu các luồng liên phân hệ. |

Tạo các thư mục và tệp khi có nội dung đầu tiên. Cập nhật liên kết trong README khi tài liệu được thêm vào.

## 5. Danh mục đặc tả của TV2

Thứ tự theo mục 1: nhóm ưu tiên `UC-05`, `UC-06`, `UC-08`, `UC-09`, rồi `UC-04`, `UC-07`, `UC-10`.

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
