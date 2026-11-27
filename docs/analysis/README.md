# Kế hoạch phân tích hệ thống quản lý nhà trẻ

Giai đoạn này chuyển kết quả khảo sát của 9 nghiệp vụ thành đặc tả use case và mô hình phân tích để làm đầu vào cho thiết kế. Phạm vi, mã yêu cầu `YC-*`, mã quy tắc `QT-*` và danh mục chức năng `F1.x.y` lấy từ [hồ sơ khảo sát](../survey/README.md).

## 1. Phân công

| Đầu việc | Phụ trách | Review | Đầu ra | Trạng thái |
| --- | --- | --- | --- | --- |
| Phân tích NV01, NV02, NV03, NV07 | TV1 | TV2 | Năm đặc tả UC-11 đến UC-15 tại mục 5. | TV1 đã hoàn thành, chờ TV2 rà soát |
| Phân tích NV04 Nhân sự, NV05 Phân công, NV06 Học phí, NV08 Đón trả trẻ, NV09 Báo cáo | TV2 | TV1 | Đặc tả các UC đã liệt kê tại mục 8.2 của hồ sơ khảo sát. Làm trước UC-05, UC-06, UC-08 và UC-09 vì các luồng này cần dữ liệu từ TV1. Tiếp theo là UC-04, UC-07 và UC-10. | Chưa bắt đầu |
| Tổng hợp sơ đồ Use Case toàn hệ thống | TV1 | TV2 | [Sơ đồ 12 UC thuộc 9 nghiệp vụ](diagrams/use-case-toan-he-thong.md) | TV1 đã hoàn thành, chờ TV2 rà soát |
| Tổng hợp ma trận truy vết và đối chiếu checklist | TV2 | TV1 | [Ma trận truy vết](traceability.md) | Đã có phần TV1, chờ TV2 tổng hợp |

## 2. Các luồng cần chốt chung

| Nghiệp vụ | Hai bên cần thống nhất |
| --- | --- |
| NV08 Đón trẻ → NV03 Chuyên cần | Lượt đón tạo kết quả có mặt. Xác định thời điểm khóa và cách chuyển phát sinh sau khóa sang luồng đề nghị điều chỉnh. |
| NV01 Hồ sơ trẻ → NV08 Trả trẻ | Kiểm tra người được nhận trẻ và hiệu lực ủy quyền tại thời điểm trả. Xác định cách ghi và duyệt ngoại lệ. |
| NV02 Xếp lớp → NV05 Phân công | Xác định danh sách lớp đủ điều kiện theo ngày hiệu lực và dữ liệu phân công cần kiểm tra khi thao tác trên lớp. |
| NV02 Xếp lớp và NV03 Chuyên cần → NV06 Học phí | Thống nhất dữ liệu tạm thu và dữ liệu đối soát cuối kỳ. Khoản tiền ăn đối soát theo số ngày có mặt đã xác nhận. |
| Các phân hệ → NV09 Báo cáo | Chốt nguồn, kỳ và trạng thái dữ liệu được dùng để tổng hợp báo cáo. |

## 3. Thứ tự thực hiện

1. **Chốt danh mục use case và mẫu đặc tả.** Giữ mã `UC-04` đến `UC-10` đã có trong khảo sát. Sau khi đối chiếu danh mục, cấp mã còn trống cho các use case mới. Mỗi đặc tả có tác nhân, mục tiêu, điều kiện trước và sau, luồng chính, luồng thay thế hoặc ngoại lệ, dữ liệu vào và ra, mã `YC-*` và `QT-*` liên quan.
2. **Thống nhất các luồng liên phân hệ rồi viết song song.** Với mỗi luồng, ghi rõ nghiệp vụ cung cấp dữ liệu, nghiệp vụ sử dụng, điều kiện dữ liệu hợp lệ và cách xử lý khi dữ liệu thay đổi. TV1 và TV2 làm trên `analysis/member-1` và `analysis/member-2`, review chéo qua pull request.
3. **Tổng hợp và rà soát.** Đối chiếu đặc tả với sơ đồ, ma trận truy vết và checklist của giảng viên. Theo mục 8 của [hồ sơ khảo sát](../survey/README.md), nhóm cần tối thiểu 8 use case, mỗi người ít nhất 4, có ít nhất 4 giao dịch phức hợp và 2 luồng liên phân hệ. Với quy tắc nghiệp vụ, kiểm tra tối thiểu 8 quy tắc, mỗi người ít nhất 4 và có ít nhất 2 quy tắc liên phân hệ.

## 4. Vị trí tài liệu

| Đường dẫn tính từ `docs/analysis/` | Nội dung |
| --- | --- |
| `README.md` | Phân công, tiến độ và mục lục. |
| `use-cases/` | Mỗi use case một file Markdown. Ví dụ `use-cases/uc-08-don-tre-theo-nhom.md`. |
| `diagrams/` | Sơ đồ dùng chung. Sơ đồ riêng của một use case có thể đặt ngay trong đặc tả. |
| `traceability.md` | Ma trận nối mã NV, YC, QT và UC, kèm đánh dấu các luồng liên phân hệ. |

## 5. Danh mục đặc tả TV1

| Nghiệp vụ | Tài liệu |
| --- | --- |
| NV01 | [UC-11 · Tiếp nhận hồ sơ trẻ](use-cases/uc-11-tiep-nhan-ho-so-tre.md) |
| NV02 | [UC-12 · Chuyển lớp](use-cases/uc-12-chuyen-lop.md) |
| NV03 | [UC-13 · Chốt chuyên cần theo lớp và ngày](use-cases/uc-13-chot-chuyen-can.md) |
| NV03 | [UC-14 · Điều chỉnh chuyên cần sau khóa](use-cases/uc-14-dieu-chinh-chuyen-can-sau-khoa.md) |
| NV07 | [UC-15 · Chốt và đối chiếu suất ăn theo bữa](use-cases/uc-15-chot-va-doi-chieu-suat-an.md) |

TV1 dùng dải mã kế tiếp UC-10. Danh mục TV2 giữ nguyên theo mục 8.2 của khảo sát. Các mã F1.1, F1.2, F1.3 và F1.7 trong đặc tả là nhánh cấp 2 đã có tại mục 8.1.
