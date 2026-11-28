# Sơ đồ Use Case toàn hệ thống

Bốn hình dưới đây biểu diễn toàn bộ **18 use case** của hệ thống (`UC-01` đến `UC-03` cho nền tảng `F1.0`; `UC-04` đến `UC-10` của TV2; `UC-11` đến `UC-18` của TV1). Mỗi khung là một nghiệp vụ hoặc phân hệ. Đường liền nối tác nhân với giao dịch. Việc đọc dữ liệu và luồng liên phân hệ được đặc tả chi tiết trong [ma trận truy vết](../traceability.md). BFD và danh mục chức năng lá `F1.x.y` đầy đủ nằm tại [mục 8.1 của khảo sát](../../survey/README.md).

## Quản trị hệ thống (F1.0)

```mermaid
flowchart TD
    subgraph F10["F1.0 · Quản trị hệ thống"]
        U01(["UC-01 · Quản lý tài khoản và phân quyền trực"])
        U02(["UC-02 · Quản lý danh mục dùng chung"])
        U03(["UC-03 · Cấu hình khung giờ vận hành, bữa ăn và mốc chốt"])
    end
    A1["Ban giám hiệu"]
    A2["Văn phòng"]
    A3["Kế toán"]
    A1 --- U01
    A2 --- U01
    A1 --- U02
    A2 --- U02
    A3 --- U02
    A1 --- U03
```

Trong `UC-01`, hệ thống tự động nhắc Ban giám hiệu khóa hoặc mở tài khoản theo ngày hiệu lực của biến động nhân sự tại `NV04`.

## Hồ sơ, lớp và nhân sự

```mermaid
flowchart TD
    subgraph N01["NV01 · Hồ sơ trẻ"]
        U11(["UC-11 · Tiếp nhận hồ sơ trẻ"])
        U16(["UC-16 · Cập nhật hồ sơ trẻ, ủy quyền nhận trẻ và lưu ý chăm sóc"])
    end
    subgraph N02["NV02 · Xếp lớp"]
        U12(["UC-12 · Chuyển lớp"])
        U17(["UC-17 · Xếp lớp ban đầu và quản lý biến động sĩ số"])
    end
    subgraph N04["NV04 · Nhân sự"]
        U04(["UC-04 · Lập và cập nhật hồ sơ nhân sự"])
    end
    subgraph N05["NV05 · Phân công giáo viên"]
        U05(["UC-05 · Lập phân công giáo viên"])
    end
    A1["Văn phòng"]
    A2["Ban giám hiệu"]
    A3["Giáo viên"]
    A1 --- U11
    A2 --- U11
    A1 --- U16
    A2 --- U16
    A3 --- U16
    A1 --- U12
    A2 --- U12
    A1 --- U17
    A2 --- U17
    A1 --- U04
    A1 --- U05
    A2 --- U05
    A3 --- U05
```

Trong `UC-05`, `Giáo viên` là tác nhân phụ: gửi đơn nghỉ ngắn ngày của bản thân và xem lịch phân công của mình (`QT-PC-06`).

## Giao nhận, chuyên cần và suất ăn

```mermaid
flowchart TD
    subgraph N08["NV08 · Đón trả trẻ"]
        U08(["UC-08 · Đón trẻ theo nhóm"])
        U09(["UC-09 · Trả trẻ theo nhóm"])
    end
    subgraph N03["NV03 · Chuyên cần"]
        U13(["UC-13 · Chốt chuyên cần theo lớp và ngày"])
        U14(["UC-14 · Điều chỉnh chuyên cần sau khóa"])
    end
    subgraph N07["NV07 · Suất ăn"]
        U15(["UC-15 · Chốt và đối chiếu suất ăn theo bữa"])
        U18(["UC-18 · Lập, duyệt và thông báo thực đơn tuần"])
    end
    A1["Giáo viên"]
    A2["Nhân sự trực được cấp quyền"]
    A3["Văn phòng"]
    A4["Ban giám hiệu"]
    A5["Người được giao quyền duyệt ngoại lệ"]
    A6["Người được phân quyền duyệt điều chỉnh"]
    A7["Bếp"]
    A1 --- U08
    A2 --- U08
    A3 --- U08
    A1 --- U09
    A2 --- U09
    A4 --- U09
    A5 --- U09
    A1 --- U13
    A1 --- U14
    A4 --- U14
    A6 --- U14
    A1 --- U15
    A7 --- U15
    A7 --- U18
    A4 --- U18
    A1 --- U18
```

- `UC-08` đồng bộ đầy đủ ba vai trò theo `QT-DT-05` và `YC-DT-05`: `Giáo viên` hoặc `Nhân sự trực được cấp quyền` ghi lượt đón; `Giáo viên` hoặc `Văn phòng` tiếp nhận báo nghỉ.
- `UC-14` là luồng độc lập thực hiện trên **ngày đã khóa** từ `UC-13` (được kích hoạt bất đồng bộ khi phát sinh/hủy lượt đón từ mốc khóa tại `UC-08`, có chứng cứ xác minh mới hoặc có quyết định hồi tố từ `UC-12`/`UC-17`). Liên hệ dữ liệu giữa `UC-13` và `UC-14` thể hiện qua tiền điều kiện `Đã khóa` và bảng truy vết, thay cho mũi tên `«extend»`.
- `Ban giám hiệu` (`A4`) là một tác nhân thống nhất; `A5` và `A6` là các tác nhân phân quyền tách riêng theo `QT-DT-05` và `QT-DD-03`. Trong `UC-13`, hệ thống tự động chụp danh sách bàn giao và khóa chuyên cần theo các mốc có hiệu lực từ `UC-03`.

## Học phí và báo cáo

```mermaid
flowchart TD
    subgraph N06["NV06 · Học phí"]
        U06(["UC-06 · Lập bảng phải thu theo kỳ"])
        U07(["UC-07 · Ghi thu tiền và in biên lai"])
    end
    subgraph N09["NV09 · Báo cáo"]
        U10(["UC-10 · Gửi, phản hồi và duyệt báo cáo"])
    end
    A1["Kế toán"]
    A2["Mọi bộ phận"]
    A3["Ban giám hiệu"]
    A1 --- U06
    A1 --- U07
    A3 --- U07
    A2 --- U10
    A3 --- U10
```

Trong `UC-07`, `Ban giám hiệu` là tác nhân phụ: duyệt phiếu điều chỉnh và hoàn tiền (`QT-HP-04`).

`UC-10` nhận bảng tổng hợp sĩ số đầu kỳ/tăng/giảm/cuối kỳ từ `UC-17` (`YC-XL-04`), số liệu tuyển sinh từ `UC-11`, chuyên cần từ `UC-13` và suất ăn từ `UC-15`. Khi **bất kỳ thay đổi dữ liệu nguồn nào** ảnh hưởng đến báo cáo đã nộp (gồm xếp/chuyển lớp hồi tố ở `UC-12`/`UC-17`, duyệt điều chỉnh chuyên cần ở `UC-14`, hoặc điều chỉnh/đối chiếu suất ăn ở `UC-15`), `UC-10` giữ nguyên ảnh chụp số liệu và cảnh báo người lập theo `QT-BC-04`: nếu báo cáo đang ở `Đã gửi`, Ban giám hiệu xử lý yêu cầu bổ sung; nếu báo cáo ở `Đã duyệt`, chuyển sang trạng thái `Cần cập nhật`.
