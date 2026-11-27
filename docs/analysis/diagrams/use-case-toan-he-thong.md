# Sơ đồ Use Case toàn hệ thống

Ba hình dưới đây biểu diễn cùng một hệ thống. Mỗi khung là một nghiệp vụ. Đường liền nối tác nhân với giao dịch. Việc đọc dữ liệu do nghiệp vụ khác cung cấp được ghi trong bảng truy vết. BFD và danh mục chức năng hiện có tại [mục 8.1 của khảo sát](../../survey/README.md).

## Hồ sơ, lớp và nhân sự

```mermaid
flowchart TD
    subgraph N01["NV01 · Hồ sơ trẻ"]
        U11(["UC-11 · Tiếp nhận hồ sơ trẻ"])
    end
    subgraph N02["NV02 · Xếp lớp"]
        U12(["UC-12 · Chuyển lớp"])
    end
    subgraph N04["NV04 · Nhân sự"]
        U04(["UC-04 · Lập và cập nhật hồ sơ nhân sự"])
    end
    subgraph N05["NV05 · Phân công giáo viên"]
        U05(["UC-05 · Lập phân công giáo viên"])
    end
    A1["Văn phòng"]
    A2["Ban giám hiệu"]
    A1 --- U11
    A2 --- U11
    A1 --- U12
    A2 --- U12
    A1 --- U04
    A1 --- U05
    A2 --- U05
```

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
    end
    A1["Giáo viên"]
    A2["Nhân sự trực được cấp quyền"]
    A3["Ban giám hiệu"]
    A4["Người được giao quyền duyệt ngoại lệ"]
    A5["Ban giám hiệu hoặc người được phân quyền duyệt"]
    A6["Bếp"]
    A1 --- U08
    A1 --- U09
    A2 --- U09
    A3 --- U09
    A4 --- U09
    A1 --- U13
    A1 --- U14
    A5 --- U14
    A1 --- U15
    A6 --- U15
    U14 -. "&lt;&lt;extend&gt;&gt;" .-> U13
```

UC-14 mở rộng UC-13 khi có căn cứ thay đổi kết quả đã khóa. Quyền duyệt điều chỉnh chuyên cần và quyền duyệt ngoại lệ trả trẻ được xét riêng.

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
    A2 --- U10
    A3 --- U10
```
