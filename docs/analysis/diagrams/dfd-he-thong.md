# Sơ đồ luồng dữ liệu hệ thống quản lý nhà trẻ

Hình chữ nhật biểu diễn tác nhân ngoài phần mềm, hình bo tròn biểu diễn tiến trình, hình trụ biểu diễn kho dữ liệu logic. Mũi tên chỉ chiều truyền của dữ liệu ghi trên cạnh. Cùng mã ở hai hình là cùng một đối tượng. Mã tiến trình DFD tăng từ 1.0 để dành số 0 cho toàn hệ thống.

[BFD và danh mục chức năng lá](bfd-toan-he-thong.md) · [Sơ đồ tác nhân và Use Case](use-case-toan-he-thong.md) · [Ma trận truy vết](../traceability.md) · [Danh mục khảo sát và biểu mẫu](../../survey/README.md)

## 1. Mức ngữ cảnh

Mỗi cạnh mang một mã gói. Bảng ngay dưới hình định nghĩa toàn bộ thành phần của gói theo đúng tên luồng mức 1. Phụ huynh cung cấp hoặc nhận dữ liệu qua nhân viên trường. Vai trò lập báo cáo là giáo viên, văn phòng, kế toán hoặc bếp trong phạm vi bộ phận. Hai quyền duyệt được giao có phạm vi độc lập.

```mermaid
flowchart TD
    BGH["Ban giám hiệu"]
    SYS(["0. Hệ thống Quản lý Nhà trẻ"])
    VP["Văn phòng"]
    PH["Phụ huynh / người nhận trẻ"]
    GV["Giáo viên"]
    DC["Người được phân quyền duyệt điều chỉnh chuyên cần"]
    KT["Kế toán"]
    BEP["Bộ phận bếp"]
    TR["Nhân sự trực được cấp quyền"]
    DX["Người được giao duyệt ngoại lệ giao nhận"]
    NL["Người lập báo cáo của bộ phận"]
    BGH -->|"BGH-vào"| SYS
    VP -->|"VP-vào"| SYS
    SYS -->|"BGH-ra"| BGH
    PH -->|"PH-vào"| SYS
    SYS -->|"PH-ra"| PH
    GV -->|"GV-vào"| SYS
    SYS -->|"VP-ra"| VP
    DC -->|"DC-vào"| SYS
    SYS -->|"GV-ra"| GV
    KT -->|"KT-vào"| SYS
    SYS -->|"KT-ra"| KT
    BEP -->|"BEP-vào"| SYS
    SYS -->|"BEP-ra"| BEP
    TR -->|"TR-vào"| SYS
    DX -->|"DX-vào"| SYS
    NL -->|"NL-vào"| SYS
    SYS -->|"NL-ra"| NL
```

| Tác nhân ngoài | Gói theo chiều hệ thống | Thành phần dữ liệu |
| --- | --- | --- |
| Ban giám hiệu | BGH-vào | Quyền theo vai trò, lớp và thời hạn, Danh mục trường và quyết định duyệt, Khung giờ và mốc chốt theo bữa, Quyết định duyệt hồ sơ, Quyết định sửa ngày sinh hoặc giám hộ, Xác nhận lưu ý chăm sóc, Quyết định xếp lớp, Quyết định chuyển lớp, Quyết định biến động sĩ số, Quyết định điều chỉnh ngày khóa, Xác nhận tiếp nhận nhân sự, Xác nhận biến động nhân sự, Quyết định duyệt phân công, Quyết định nghỉ và dạy thay, Quyết định duyệt bảng phải thu, Quyết định điều chỉnh hoặc hoàn tiền, Quyết định duyệt thực đơn, Quyết định duyệt ngoại lệ nhận hộ, Quyết định hủy lượt trả, Phản hồi, yêu cầu bổ sung và quyết định duyệt |
| Văn phòng | VP-vào | Thông tin nhân sự cần cấp tài khoản, Danh mục lớp đề xuất, Phiếu tiếp nhận và kết quả đối chiếu, Phiếu cập nhật hồ sơ, Giấy ủy quyền và thời hạn, Nội dung lưu ý và căn cứ, Phương án xếp lớp, Phương án chuyển lớp và ngày hiệu lực, Hồ sơ biến động diện đi học, Hồ sơ tiếp nhận nhân sự, Văn bằng và chứng chỉ, Phiếu biến động nhân sự, Phương án bố trí giáo viên, Phương án dạy thay, Phiếu báo nghỉ do văn phòng tiếp nhận |
| Ban giám hiệu | BGH-ra | Danh sách tài khoản cần khóa hoặc mở, Tổng hợp nhân sự, Tổng hợp công nợ, Tổng hợp giao nhận, Báo cáo tổng hợp toàn trường |
| Phụ huynh / người nhận trẻ | PH-vào | Hồ sơ nhập học và giấy tờ bổ sung, Thông tin thay đổi nhân thân và giám hộ, Xác nhận lịch đón về trước bữa, Thông tin báo nghỉ, Thông tin người nhận hộ đột xuất |
| Phụ huynh / người nhận trẻ | PH-ra | Danh sách giấy tờ cần bổ sung, Thông báo học phí kèm nợ kỳ cũ, Biên lai thu tiền, Thực đơn tuần và thông tin món cần lưu ý |
| Giáo viên | GV-vào | Đề nghị lưu ý chăm sóc, Kết quả xác minh trẻ vắng, Đề nghị điều chỉnh và chứng cứ, Đơn nghỉ ngắn ngày, Lịch về sớm do lớp ghi nhận, Bảng chốt suất theo lớp và bữa, Đề nghị đổi suất và số thực nhận, Phiếu báo nghỉ của lớp, Danh sách trẻ đến lớp, Kết quả xác minh người nhận hộ, Thông tin bàn giao tại lớp, Lý do hủy lượt sai |
| Văn phòng | VP-ra | Bảng sĩ số đầu kỳ, tăng, giảm và cuối kỳ, Danh sách nhân sự, Tổng hợp ca theo lớp, Sổ đón trả toàn trường |
| Người được phân quyền duyệt điều chỉnh chuyên cần | DC-vào | Quyết định theo quyền duyệt điều chỉnh |
| Giáo viên | GV-ra | Bảng chuyên cần theo kỳ, Lịch cá nhân, Sổ đón trả của lớp |
| Kế toán | KT-vào | Biểu phí và diện miễn giảm đề xuất, Kỳ thu và danh sách khoản thu, Chứng từ thu và phân bổ, Phiếu đề nghị điều chỉnh hoặc hoàn tiền |
| Kế toán | KT-ra | Sổ công nợ chi tiết |
| Bộ phận bếp | BEP-vào | Thực đơn theo ngày và bữa, Xác nhận điều chỉnh và số thực giao |
| Bộ phận bếp | BEP-ra | Số suất chốt theo lớp và bữa |
| Nhân sự trực được cấp quyền | TR-vào | Danh sách trẻ đến trong ca trực, Chứng cứ xác minh trong ca trực, Thông tin bàn giao trong ca trực, Lý do hủy lượt do nhân sự trực ghi |
| Người được giao duyệt ngoại lệ giao nhận | DX-vào | Quyết định nhận hộ theo quyền được giao, Quyết định hủy lượt trả theo ủy quyền |
| Người lập báo cáo của bộ phận | NL-vào | Kỳ báo cáo và ghi chú bộ phận, Thông tin xác nhận gửi phiên bản |
| Người lập báo cáo của bộ phận | NL-ra | Cảnh báo nguồn đổi và yêu cầu bổ sung |

## 2. Mức đỉnh

| Tiến trình chính | Nhánh BFD | Nghiệp vụ |
| --- | --- | --- |
| 1.0 Quản trị hệ thống | F1.0 | Nền tảng dùng chung |
| 2.0 Quản lý hồ sơ trẻ | F1.1 | NV01 |
| 3.0 Xếp lớp và theo dõi sĩ số | F1.2 | NV02 |
| 4.0 Chốt và điều chỉnh chuyên cần | F1.3 | NV03 |
| 5.0 Quản lý hồ sơ nhân sự | F1.4 | NV04 |
| 6.0 Phân công giáo viên | F1.5 | NV05 |
| 7.0 Lập và thu học phí | F1.6 | NV06 |
| 8.0 Quản lý thực đơn và suất ăn | F1.7 | NV07 |
| 9.0 Ghi nhận đón trả và báo nghỉ | F1.8 | NV08 |
| 10.0 Lập và duyệt báo cáo | F1.9 | NV09 |

| Kho | Nội dung |
| --- | --- |
| D1 | Tài khoản, quyền, danh mục và cấu hình hiệu lực |
| D2 | Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý |
| D3 | Lịch sử lớp, diện đi học và sĩ số |
| D4 | Chuyên cần, khóa ngày và điều chỉnh |
| D5 | Hồ sơ và biến động nhân sự |
| D6 | Phân công, nghỉ và dạy thay |
| D7 | Học phí, thanh toán và công nợ theo kỳ |
| D8 | Thực đơn, suất ăn và đối soát giao nhận |
| D9 | Báo nghỉ và sổ đón trả |
| D10 | Phiên bản, ảnh chụp và phản hồi báo cáo |

Các hình sau ghép thành một DFD mức 1. Cạnh giữa hai tiến trình xuất hiện ở cả hai hình để có thể đọc riêng từng nhánh.

### 1.0 Quản trị hệ thống

```mermaid
flowchart TD
    BGH["Ban giám hiệu"]
    P0(["1.0 Quản trị hệ thống"])
    VP["Văn phòng"]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    P6(["7.0 Lập và thu học phí"])
    BGH -->|"Quyền theo vai trò, lớp và thời hạn"| P0
    VP -->|"Thông tin nhân sự cần cấp tài khoản"| P0
    D5 -->|"Biến động nhân sự có hiệu lực"| P0
    P0 -->|"Tài khoản và phạm vi quyền trực"| D1
    P0 -->|"Danh sách tài khoản cần khóa hoặc mở"| BGH
    VP -->|"Danh mục lớp đề xuất"| P0
    BGH -->|"Danh mục trường và quyết định duyệt"| P0
    P6 -->|"Biểu phí và miễn giảm trình duyệt"| P0
    P0 -->|"Danh mục đã duyệt theo hiệu lực"| D1
    BGH -->|"Khung giờ và mốc chốt theo bữa"| P0
    P0 -->|"Cấu hình vận hành có hiệu lực"| D1
```

### 2.0 Quản lý hồ sơ trẻ

```mermaid
flowchart TD
    PH["Phụ huynh / người nhận trẻ"]
    P1(["2.0 Quản lý hồ sơ trẻ"])
    VP["Văn phòng"]
    BGH["Ban giám hiệu"]
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    P2(["3.0 Xếp lớp và theo dõi sĩ số"])
    GV["Giáo viên"]
    PH -->|"Hồ sơ nhập học và giấy tờ bổ sung"| P1
    VP -->|"Phiếu tiếp nhận và kết quả đối chiếu"| P1
    BGH -->|"Quyết định duyệt hồ sơ"| P1
    P1 -->|"Danh sách giấy tờ cần bổ sung"| PH
    P1 -->|"Danh sách trẻ chờ xếp lớp"| P2
    PH -->|"Thông tin thay đổi nhân thân và giám hộ"| P1
    VP -->|"Phiếu cập nhật hồ sơ"| P1
    BGH -->|"Quyết định sửa ngày sinh hoặc giám hộ"| P1
    P1 -->|"Lịch sử thay đổi hồ sơ"| D2
    VP -->|"Giấy ủy quyền và thời hạn"| P1
    P1 -->|"Danh sách ủy quyền cố định"| D2
    GV -->|"Đề nghị lưu ý chăm sóc"| P1
    VP -->|"Nội dung lưu ý và căn cứ"| P1
    BGH -->|"Xác nhận lưu ý chăm sóc"| P1
    P1 -->|"Lưu ý và trạng thái xác nhận"| D2
    D2 -->|"Hồ sơ hiện hành và lịch sử đối chiếu"| P1
    P1 -->|"Hồ sơ tiếp nhận và giấy tờ theo đợt"| D2
```

### 3.0 Xếp lớp và theo dõi sĩ số

```mermaid
flowchart TD
    P1(["2.0 Quản lý hồ sơ trẻ"])
    P2(["3.0 Xếp lớp và theo dõi sĩ số"])
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    VP["Văn phòng"]
    BGH["Ban giám hiệu"]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    P3(["4.0 Chốt và điều chỉnh chuyên cần"])
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    P1 -->|"Danh sách trẻ chờ xếp lớp"| P2
    D1 -->|"Danh mục lớp và định mức"| P2
    VP -->|"Phương án xếp lớp"| P2
    BGH -->|"Quyết định xếp lớp"| P2
    P2 -->|"Dòng xếp lớp ban đầu"| D3
    VP -->|"Phương án chuyển lớp và ngày hiệu lực"| P2
    BGH -->|"Quyết định chuyển lớp"| P2
    P2 -->|"Lịch sử lớp đi và lớp đến"| D3
    VP -->|"Hồ sơ biến động diện đi học"| P2
    BGH -->|"Quyết định biến động sĩ số"| P2
    P2 -->|"Khoảng thuộc diện đi học"| D3
    D3 -->|"Lịch sử hiệu lực theo trẻ và lớp"| P2
    P2 -->|"Bảng sĩ số đầu kỳ, tăng, giảm và cuối kỳ"| VP
    P2 -->|"Quyết định hồi tố ảnh hưởng ngày khóa"| P3
    D6 -->|"Phân công và cảnh báo định mức lớp"| P2
    D6 -->|"Phân công lớp đích theo ngày"| P2
    D6 -->|"Định mức giáo viên khi trẻ quay lại"| P2
```

### 4.0 Chốt và điều chỉnh chuyên cần

```mermaid
flowchart TD
    P2(["3.0 Xếp lớp và theo dõi sĩ số"])
    P3(["4.0 Chốt và điều chỉnh chuyên cần"])
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    GV["Giáo viên"]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    BGH["Ban giám hiệu"]
    DC["Người được phân quyền duyệt điều chỉnh chuyên cần"]
    P8(["9.0 Ghi nhận đón trả và báo nghỉ"])
    P2 -->|"Quyết định hồi tố ảnh hưởng ngày khóa"| P3
    D3 -->|"Danh sách lớp và khoảng nghỉ dài ngày"| P3
    D6 -->|"Giáo viên phụ trách theo ngày"| P3
    D1 -->|"Mốc bàn giao và khóa ngày"| P3
    D9 -->|"Lượt đón, báo nghỉ, lượt trả và giờ về"| P3
    GV -->|"Kết quả xác minh trẻ vắng"| P3
    P3 -->|"Bản bàn giao và bảng chuyên cần đã khóa"| D4
    GV -->|"Đề nghị điều chỉnh và chứng cứ"| P3
    BGH -->|"Quyết định điều chỉnh ngày khóa"| P3
    DC -->|"Quyết định theo quyền duyệt điều chỉnh"| P3
    D4 -->|"Chuyên cần gốc của ngày khóa"| P3
    P3 -->|"Phiếu điều chỉnh và kết quả duyệt"| D4
    D4 -->|"Ngày có mặt và điều chỉnh đã duyệt"| P3
    P3 -->|"Bảng chuyên cần theo kỳ"| GV
    P8 -->|"Lượt đón phát sinh sau khóa"| P3
    P8 -->|"Lượt đón bị hủy sau khóa"| P3
    P3 -->|"Kết quả khóa kỳ theo lớp"| D4
```

### 5.0 Quản lý hồ sơ nhân sự

```mermaid
flowchart TD
    VP["Văn phòng"]
    P4(["5.0 Quản lý hồ sơ nhân sự"])
    BGH["Ban giám hiệu"]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    VP -->|"Hồ sơ tiếp nhận nhân sự"| P4
    BGH -->|"Xác nhận tiếp nhận nhân sự"| P4
    P4 -->|"Hồ sơ nhân sự"| D5
    VP -->|"Văn bằng và chứng chỉ"| P4
    P4 -->|"Chi tiết trình độ"| D5
    VP -->|"Phiếu biến động nhân sự"| P4
    BGH -->|"Xác nhận biến động nhân sự"| P4
    P4 -->|"Lịch sử trạng thái làm việc"| D5
    D5 -->|"Hồ sơ và lịch sử công tác"| P4
    P4 -->|"Danh sách nhân sự"| VP
    P4 -->|"Tổng hợp nhân sự"| BGH
    D5 -->|"Hồ sơ đối chiếu trùng nhân sự"| P4
```

### 6.0 Phân công giáo viên

```mermaid
flowchart TD
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    P5(["6.0 Phân công giáo viên"])
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    VP["Văn phòng"]
    BGH["Ban giám hiệu"]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    GV["Giáo viên"]
    D5 -->|"Nhân sự đủ điều kiện đứng lớp"| P5
    D3 -->|"Sĩ số theo ngày hiệu lực"| P5
    VP -->|"Phương án bố trí giáo viên"| P5
    BGH -->|"Quyết định duyệt phân công"| P5
    P5 -->|"Lịch phân công có hiệu lực"| D6
    GV -->|"Đơn nghỉ ngắn ngày"| P5
    VP -->|"Phương án dạy thay"| P5
    BGH -->|"Quyết định nghỉ và dạy thay"| P5
    P5 -->|"Lịch nghỉ và ca thay thế"| D6
    D6 -->|"Lịch sử ca có hiệu lực"| P5
    P5 -->|"Lịch cá nhân"| GV
    P5 -->|"Tổng hợp ca theo lớp"| VP
    D1 -->|"Trạng thái lớp tại ngày hiệu lực"| P5
    D6 -->|"Lịch phân công và nghỉ đã duyệt"| P5
```

### 7.0 Lập và thu học phí

```mermaid
flowchart TD
    P6(["7.0 Lập và thu học phí"])
    P0(["1.0 Quản trị hệ thống"])
    KT["Kế toán"]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    D7[("D7 · Học phí, thanh toán và công nợ theo kỳ")]
    BGH["Ban giám hiệu"]
    PH["Phụ huynh / người nhận trẻ"]
    P6 -->|"Biểu phí và miễn giảm trình duyệt"| P0
    KT -->|"Biểu phí và diện miễn giảm đề xuất"| P6
    D1 -->|"Biểu phí và miễn giảm đã duyệt"| P6
    D2 -->|"Định danh trẻ và giám hộ chính"| P6
    D3 -->|"Khoảng đi học và biến động hồi tố"| P6
    D4 -->|"Ngày có mặt đã khóa và đề nghị còn chờ"| P6
    D7 -->|"Nợ kỳ cũ, số dư có và tạm thu kỳ trước"| P6
    KT -->|"Kỳ thu và danh sách khoản thu"| P6
    BGH -->|"Quyết định duyệt bảng phải thu"| P6
    P6 -->|"Bảng phải thu và bù trừ tham chiếu kỳ gốc"| D7
    P6 -->|"Thông báo học phí kèm nợ kỳ cũ"| PH
    KT -->|"Chứng từ thu và phân bổ"| P6
    D7 -->|"Khoản còn nợ theo kỳ phát sinh"| P6
    P6 -->|"Phiếu thu và chi tiết phân bổ"| D7
    P6 -->|"Biên lai thu tiền"| PH
    KT -->|"Phiếu đề nghị điều chỉnh hoặc hoàn tiền"| P6
    BGH -->|"Quyết định điều chỉnh hoặc hoàn tiền"| P6
    P6 -->|"Chứng từ điều chỉnh và hoàn tiền"| D7
    D7 -->|"Phát sinh và thanh toán theo kỳ"| P6
    P6 -->|"Sổ công nợ chi tiết"| KT
    P6 -->|"Tổng hợp công nợ"| BGH
```

### 8.0 Quản lý thực đơn và suất ăn

```mermaid
flowchart TD
    BEP["Bộ phận bếp"]
    P7(["8.0 Quản lý thực đơn và suất ăn"])
    BGH["Ban giám hiệu"]
    D8[("D8 · Thực đơn, suất ăn và đối soát giao nhận")]
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    PH["Phụ huynh / người nhận trẻ"]
    GV["Giáo viên"]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    BEP -->|"Thực đơn theo ngày và bữa"| P7
    BGH -->|"Quyết định duyệt thực đơn"| P7
    P7 -->|"Thực đơn đã duyệt"| D8
    D8 -->|"Thực đơn tuần có hiệu lực"| P7
    D2 -->|"Dị ứng, chế độ ăn và cảnh báo chờ xác nhận"| P7
    P7 -->|"Thực đơn tuần và thông tin món cần lưu ý"| PH
    PH -->|"Xác nhận lịch đón về trước bữa"| P7
    GV -->|"Lịch về sớm do lớp ghi nhận"| P7
    P7 -->|"Phản hồi theo trẻ và bữa, giờ về dự kiến và thời điểm ghi"| D8
    D8 -->|"Phản hồi còn hiệu lực theo trẻ và bữa, thực đơn"| P7
    D4 -->|"Danh sách có mặt theo mốc bữa và bản bàn giao sáng"| P7
    D9 -->|"Báo nghỉ và giờ về thực tế"| P7
    D1 -->|"Mốc chốt từng bữa"| P7
    D6 -->|"Phân công lớp phụ trách"| P7
    GV -->|"Bảng chốt suất theo lớp và bữa"| P7
    P7 -->|"Chi tiết suất đã chốt"| D8
    P7 -->|"Số suất chốt theo lớp và bữa"| BEP
    GV -->|"Đề nghị đổi suất và số thực nhận"| P7
    BEP -->|"Xác nhận điều chỉnh và số thực giao"| P7
    D8 -->|"Số suất đã chốt theo bữa"| P7
    P7 -->|"Chênh lệch giao nhận và kết quả đối soát"| D8
```

### 9.0 Ghi nhận đón trả và báo nghỉ

```mermaid
flowchart TD
    PH["Phụ huynh / người nhận trẻ"]
    P8(["9.0 Ghi nhận đón trả và báo nghỉ"])
    VP["Văn phòng"]
    GV["Giáo viên"]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    TR["Nhân sự trực được cấp quyền"]
    P3(["4.0 Chốt và điều chỉnh chuyên cần"])
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    BGH["Ban giám hiệu"]
    DX["Người được giao duyệt ngoại lệ giao nhận"]
    PH -->|"Thông tin báo nghỉ"| P8
    VP -->|"Phiếu báo nghỉ do văn phòng tiếp nhận"| P8
    GV -->|"Phiếu báo nghỉ của lớp"| P8
    P8 -->|"Báo nghỉ theo trẻ, khoảng ngày và lý do"| D9
    D3 -->|"Danh sách trẻ thuộc lớp theo ngày"| P8
    D6 -->|"Ca phụ trách và dạy thay"| P8
    D1 -->|"Quyền trực theo lớp và thời gian"| P8
    GV -->|"Danh sách trẻ đến lớp"| P8
    TR -->|"Danh sách trẻ đến trong ca trực"| P8
    P8 -->|"Lượt đón có giờ và người ghi"| D9
    P8 -->|"Lượt đón phát sinh sau khóa"| P3
    PH -->|"Thông tin người nhận hộ đột xuất"| P8
    D2 -->|"Giám hộ và ủy quyền còn hiệu lực"| P8
    GV -->|"Kết quả xác minh người nhận hộ"| P8
    TR -->|"Chứng cứ xác minh trong ca trực"| P8
    BGH -->|"Quyết định duyệt ngoại lệ nhận hộ"| P8
    DX -->|"Quyết định nhận hộ theo quyền được giao"| P8
    D2 -->|"Giám hộ và ủy quyền cố định theo ngày"| P8
    D9 -->|"Lượt đón còn hiệu lực"| P8
    D6 -->|"Giáo viên phụ trách tại thời điểm trả"| P8
    D1 -->|"Phạm vi quyền trực giao trả"| P8
    GV -->|"Thông tin bàn giao tại lớp"| P8
    TR -->|"Thông tin bàn giao trong ca trực"| P8
    P8 -->|"Lượt trả và giờ về thực tế"| D9
    D9 -->|"Nhật ký giao nhận theo phạm vi"| P8
    P8 -->|"Sổ đón trả toàn trường"| VP
    P8 -->|"Sổ đón trả của lớp"| GV
    P8 -->|"Tổng hợp giao nhận"| BGH
    GV -->|"Lý do hủy lượt sai"| P8
    TR -->|"Lý do hủy lượt do nhân sự trực ghi"| P8
    BGH -->|"Quyết định hủy lượt trả"| P8
    DX -->|"Quyết định hủy lượt trả theo ủy quyền"| P8
    P8 -->|"Dấu hủy và lịch sử lượt gốc"| D9
    P8 -->|"Lượt đón bị hủy sau khóa"| P3
    D9 -->|"Trạng thái lượt gốc và lượt liên quan"| P8
    P8 -->|"Kết quả xác minh và quyết định ngoại lệ kể cả từ chối"| D9
```

### 10.0 Lập và duyệt báo cáo

```mermaid
flowchart TD
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    NL["Người lập báo cáo của bộ phận"]
    P9(["10.0 Lập và duyệt báo cáo"])
    D10[("D10 · Phiên bản, ảnh chụp và phản hồi báo cáo")]
    BGH["Ban giám hiệu"]
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    D7[("D7 · Học phí, thanh toán và công nợ theo kỳ")]
    D8[("D8 · Thực đơn, suất ăn và đối soát giao nhận")]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    NL -->|"Kỳ báo cáo và ghi chú bộ phận"| P9
    NL -->|"Thông tin xác nhận gửi phiên bản"| P9
    BGH -->|"Phản hồi, yêu cầu bổ sung và quyết định duyệt"| P9
    P9 -->|"Phản hồi và trạng thái phiên bản"| D10
    P9 -->|"Cảnh báo nguồn đổi và yêu cầu bổ sung"| NL
    P9 -->|"Báo cáo tổng hợp toàn trường"| BGH
    D10 -->|"Bản nháp của người lập"| P9
    D1 -->|"Loại báo cáo, chỉ tiêu, bộ phận phải nộp và hạn gửi"| P9
    D2 -->|"Số liệu và thay đổi nguồn D2"| P9
    D3 -->|"Số liệu và thay đổi nguồn D3"| P9
    D4 -->|"Số liệu và thay đổi nguồn D4"| P9
    D5 -->|"Số liệu và thay đổi nguồn D5"| P9
    D6 -->|"Số liệu và thay đổi nguồn D6"| P9
    D7 -->|"Số liệu và thay đổi nguồn D7"| P9
    D8 -->|"Số liệu và thay đổi nguồn D8"| P9
    D9 -->|"Số liệu và thay đổi nguồn D9"| P9
    P9 -->|"Nội dung phiên báo cáo"| D10
    D10 -->|"Phiên báo cáo theo quyền và trạng thái"| P9
```

## 3. Mức dưới

Mỗi hình phân rã đúng một tiến trình cha. Bảng kèm theo ánh xạ từng xử lý sang chức năng lá và đặc tả.

### Phân rã 1.0 Quản trị hệ thống

```mermaid
flowchart TD
    BGH["Ban giám hiệu"]
    L01(["1.1 Quản lý tài khoản, vai trò và phân quyền trực"])
    VP["Văn phòng"]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    L02(["1.2 Quản lý danh mục dùng chung"])
    P6(["7.0 Lập và thu học phí"])
    L03(["1.3 Cấu hình khung giờ vận hành, bữa ăn và mốc chốt"])
    BGH -->|"Quyền theo vai trò, lớp và thời hạn"| L01
    VP -->|"Thông tin nhân sự cần cấp tài khoản"| L01
    D5 -->|"Biến động nhân sự có hiệu lực"| L01
    L01 -->|"Tài khoản và phạm vi quyền trực"| D1
    L01 -->|"Danh sách tài khoản cần khóa hoặc mở"| BGH
    VP -->|"Danh mục lớp đề xuất"| L02
    BGH -->|"Danh mục trường và quyết định duyệt"| L02
    P6 -->|"Biểu phí và miễn giảm trình duyệt"| L02
    L02 -->|"Danh mục đã duyệt theo hiệu lực"| D1
    BGH -->|"Khung giờ và mốc chốt theo bữa"| L03
    L03 -->|"Cấu hình vận hành có hiệu lực"| D1
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 1.1 | F1.0.1 | UC-01 |
| 1.2 | F1.0.2 | UC-02 |
| 1.3 | F1.0.3 | UC-03 |

### Phân rã 2.0 Quản lý hồ sơ trẻ

```mermaid
flowchart TD
    PH["Phụ huynh / người nhận trẻ"]
    L11(["2.1 Tiếp nhận, đối chiếu trùng và duyệt hồ sơ trẻ"])
    VP["Văn phòng"]
    BGH["Ban giám hiệu"]
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    L12(["2.2 Theo dõi bổ sung giấy tờ và bàn giao danh sách chờ xếp lớp"])
    P2(["3.0 Xếp lớp và theo dõi sĩ số"])
    L13(["2.3 Cập nhật thông tin nhân thân, giám hộ chính và lịch sử hồ sơ"])
    L14(["2.4 Quản lý ủy quyền cố định nhận trẻ"])
    GV["Giáo viên"]
    L15(["2.5 Đề nghị, ghi nhận và xác nhận lưu ý chăm sóc"])
    PH -->|"Hồ sơ nhập học và giấy tờ bổ sung"| L11
    VP -->|"Phiếu tiếp nhận và kết quả đối chiếu"| L11
    BGH -->|"Quyết định duyệt hồ sơ"| L11
    L11 -->|"Mã trẻ, đợt tiếp nhận và quyết định duyệt"| D2
    L11 -->|"Tình trạng giấy tờ"| L12
    L12 -->|"Danh sách giấy tờ cần bổ sung"| PH
    L12 -->|"Danh sách trẻ chờ xếp lớp"| P2
    PH -->|"Thông tin thay đổi nhân thân và giám hộ"| L13
    VP -->|"Phiếu cập nhật hồ sơ"| L13
    BGH -->|"Quyết định sửa ngày sinh hoặc giám hộ"| L13
    L13 -->|"Lịch sử thay đổi hồ sơ"| D2
    VP -->|"Giấy ủy quyền và thời hạn"| L14
    L14 -->|"Danh sách ủy quyền cố định"| D2
    GV -->|"Đề nghị lưu ý chăm sóc"| L15
    VP -->|"Nội dung lưu ý và căn cứ"| L15
    BGH -->|"Xác nhận lưu ý chăm sóc"| L15
    L15 -->|"Lưu ý và trạng thái xác nhận"| D2
    D2 -->|"Hồ sơ hiện hành và lịch sử đối chiếu"| L11
    D2 -->|"Hồ sơ hiện hành và lịch sử đối chiếu"| L13
    L12 -->|"Giấy tờ từng đợt và lịch hẹn bổ sung"| D2
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 2.1 | F1.1.1 | UC-11 |
| 2.2 | F1.1.2 | UC-11 |
| 2.3 | F1.1.3 | UC-16 |
| 2.4 | F1.1.4 | UC-16 |
| 2.5 | F1.1.5 | UC-16 |

### Phân rã 3.0 Xếp lớp và theo dõi sĩ số

```mermaid
flowchart TD
    P1(["2.0 Quản lý hồ sơ trẻ"])
    L21(["3.1 Xếp lớp ban đầu và quản lý danh sách chờ"])
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    VP["Văn phòng"]
    BGH["Ban giám hiệu"]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    L22(["3.2 Chuyển lớp và giữ lịch sử hai đầu"])
    L23(["3.3 Quản lý nghỉ dài ngày, bảo lưu, thôi học và quay lại"])
    L24(["3.4 Tra cứu danh sách theo ngày, đối chiếu hồi tố và tổng hợp sĩ số"])
    P3(["4.0 Chốt và điều chỉnh chuyên cần"])
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    P1 -->|"Danh sách trẻ chờ xếp lớp"| L21
    D1 -->|"Danh mục lớp và định mức"| L21
    VP -->|"Phương án xếp lớp"| L21
    BGH -->|"Quyết định xếp lớp"| L21
    L21 -->|"Dòng xếp lớp ban đầu"| D3
    VP -->|"Phương án chuyển lớp và ngày hiệu lực"| L22
    BGH -->|"Quyết định chuyển lớp"| L22
    L22 -->|"Lịch sử lớp đi và lớp đến"| D3
    VP -->|"Hồ sơ biến động diện đi học"| L23
    BGH -->|"Quyết định biến động sĩ số"| L23
    L23 -->|"Khoảng thuộc diện đi học"| D3
    D3 -->|"Lịch sử hiệu lực theo trẻ và lớp"| L24
    L24 -->|"Bảng sĩ số đầu kỳ, tăng, giảm và cuối kỳ"| VP
    L24 -->|"Quyết định hồi tố ảnh hưởng ngày khóa"| P3
    D6 -->|"Phân công và cảnh báo định mức lớp"| L21
    D6 -->|"Phân công lớp đích theo ngày"| L22
    D6 -->|"Định mức giáo viên khi trẻ quay lại"| L23
    D3 -->|"Lịch sử hiệu lực theo trẻ và lớp"| L21
    D3 -->|"Lịch sử hiệu lực theo trẻ và lớp"| L22
    D3 -->|"Lịch sử hiệu lực theo trẻ và lớp"| L23
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 3.1 | F1.2.1 | UC-17 |
| 3.2 | F1.2.2 | UC-12 |
| 3.3 | F1.2.3 | UC-17 |
| 3.4 | F1.2.4 | UC-12, UC-17 |

### Phân rã 4.0 Chốt và điều chỉnh chuyên cần

```mermaid
flowchart TD
    P2(["3.0 Xếp lớp và theo dõi sĩ số"])
    L33(["4.3 Đề nghị và duyệt điều chỉnh chuyên cần sau khóa"])
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    L31(["4.1 Tạo bảng điểm danh và tự động ghi nhận nghỉ dài ngày"])
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    L32(["4.2 Tiếp nhận lượt đón/trả, xác minh vắng và chốt theo các mốc có hiệu lực (mặc định 07:30, 08:30, 09:00)"])
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    GV["Giáo viên"]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    BGH["Ban giám hiệu"]
    DC["Người được phân quyền duyệt điều chỉnh chuyên cần"]
    L34(["4.4 Tổng hợp, khóa kỳ và xuất chuyên cần"])
    P8(["9.0 Ghi nhận đón trả và báo nghỉ"])
    P2 -->|"Quyết định hồi tố ảnh hưởng ngày khóa"| L33
    D3 -->|"Danh sách lớp và khoảng nghỉ dài ngày"| L31
    D6 -->|"Giáo viên phụ trách theo ngày"| L31
    L31 -->|"Bảng điểm danh đầu ngày"| L32
    D1 -->|"Mốc bàn giao và khóa ngày"| L32
    D9 -->|"Lượt đón, báo nghỉ, lượt trả và giờ về"| L32
    GV -->|"Kết quả xác minh trẻ vắng"| L32
    L32 -->|"Bản bàn giao và bảng chuyên cần đã khóa"| D4
    GV -->|"Đề nghị điều chỉnh và chứng cứ"| L33
    BGH -->|"Quyết định điều chỉnh ngày khóa"| L33
    DC -->|"Quyết định theo quyền duyệt điều chỉnh"| L33
    D4 -->|"Chuyên cần gốc của ngày khóa"| L33
    L33 -->|"Phiếu điều chỉnh và kết quả duyệt"| D4
    D4 -->|"Ngày có mặt và điều chỉnh đã duyệt"| L34
    L34 -->|"Bảng chuyên cần theo kỳ"| GV
    P8 -->|"Lượt đón phát sinh sau khóa"| L33
    P8 -->|"Lượt đón bị hủy sau khóa"| L33
    L34 -->|"Kết quả khóa kỳ theo lớp"| D4
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 4.1 | F1.3.1 | UC-13 |
| 4.2 | F1.3.2 | UC-13 |
| 4.3 | F1.3.3 | UC-14 |
| 4.4 | F1.3.4 | UC-13 |

### Phân rã 5.0 Quản lý hồ sơ nhân sự

```mermaid
flowchart TD
    VP["Văn phòng"]
    L41(["5.1 Lập hồ sơ nhân sự"])
    BGH["Ban giám hiệu"]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    L42(["5.2 Quản lý bằng cấp, chứng chỉ, giấy tờ"])
    L43(["5.3 Ghi nhận biến động nhân sự"])
    L44(["5.4 Tra cứu và xuất danh sách nhân sự"])
    VP -->|"Hồ sơ tiếp nhận nhân sự"| L41
    BGH -->|"Xác nhận tiếp nhận nhân sự"| L41
    L41 -->|"Hồ sơ nhân sự"| D5
    VP -->|"Văn bằng và chứng chỉ"| L42
    L42 -->|"Chi tiết trình độ"| D5
    VP -->|"Phiếu biến động nhân sự"| L43
    BGH -->|"Xác nhận biến động nhân sự"| L43
    L43 -->|"Lịch sử trạng thái làm việc"| D5
    D5 -->|"Hồ sơ và lịch sử công tác"| L44
    L44 -->|"Danh sách nhân sự"| VP
    L44 -->|"Tổng hợp nhân sự"| BGH
    D5 -->|"Hồ sơ đối chiếu trùng nhân sự"| L41
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 5.1 | F1.4.1 | UC-04 |
| 5.2 | F1.4.2 | UC-04 |
| 5.3 | F1.4.3 | UC-04 |
| 5.4 | F1.4.4 | UC-04 |

### Phân rã 6.0 Phân công giáo viên

```mermaid
flowchart TD
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    L51(["6.1 Lập phương án phân công"])
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    VP["Văn phòng"]
    L52(["6.2 Duyệt phân công"])
    BGH["Ban giám hiệu"]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    GV["Giáo viên"]
    L53(["6.3 Đơn nghỉ ngắn ngày và phân công dạy thay"])
    L54(["6.4 Tra cứu lịch sử và tổng hợp ca"])
    D5 -->|"Nhân sự đủ điều kiện đứng lớp"| L51
    D3 -->|"Sĩ số theo ngày hiệu lực"| L51
    VP -->|"Phương án bố trí giáo viên"| L51
    L51 -->|"Dự thảo phân công và cảnh báo"| L52
    BGH -->|"Quyết định duyệt phân công"| L52
    L52 -->|"Lịch phân công có hiệu lực"| D6
    GV -->|"Đơn nghỉ ngắn ngày"| L53
    VP -->|"Phương án dạy thay"| L53
    BGH -->|"Quyết định nghỉ và dạy thay"| L53
    L53 -->|"Lịch nghỉ và ca thay thế"| D6
    D6 -->|"Lịch sử ca có hiệu lực"| L54
    L54 -->|"Lịch cá nhân"| GV
    L54 -->|"Tổng hợp ca theo lớp"| VP
    D1 -->|"Trạng thái lớp tại ngày hiệu lực"| L51
    D6 -->|"Lịch phân công và nghỉ đã duyệt"| L51
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 6.1 | F1.5.1 | UC-05 |
| 6.2 | F1.5.2 | UC-05 |
| 6.3 | F1.5.3 | UC-05 |
| 6.4 | F1.5.4 | UC-05 |

### Phân rã 7.0 Lập và thu học phí

```mermaid
flowchart TD
    L61(["7.1 Quản lý biểu phí và miễn giảm"])
    P0(["1.0 Quản trị hệ thống"])
    KT["Kế toán"]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    L62(["7.2 Lập bảng phải thu"])
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    D7[("D7 · Học phí, thanh toán và công nợ theo kỳ")]
    BGH["Ban giám hiệu"]
    PH["Phụ huynh / người nhận trẻ"]
    L63(["7.3 Thu tiền và in biên lai"])
    L64(["7.4 Điều chỉnh và hoàn tiền"])
    L65(["7.5 Theo dõi công nợ"])
    L61 -->|"Biểu phí và miễn giảm trình duyệt"| P0
    KT -->|"Biểu phí và diện miễn giảm đề xuất"| L61
    D1 -->|"Biểu phí và miễn giảm đã duyệt"| L62
    D2 -->|"Định danh trẻ và giám hộ chính"| L62
    D3 -->|"Khoảng đi học và biến động hồi tố"| L62
    D4 -->|"Ngày có mặt đã khóa và đề nghị còn chờ"| L62
    D7 -->|"Nợ kỳ cũ, số dư có và tạm thu kỳ trước"| L62
    KT -->|"Kỳ thu và danh sách khoản thu"| L62
    BGH -->|"Quyết định duyệt bảng phải thu"| L62
    L62 -->|"Bảng phải thu và bù trừ tham chiếu kỳ gốc"| D7
    L62 -->|"Thông báo học phí kèm nợ kỳ cũ"| PH
    KT -->|"Chứng từ thu và phân bổ"| L63
    D7 -->|"Khoản còn nợ theo kỳ phát sinh"| L63
    L63 -->|"Phiếu thu và chi tiết phân bổ"| D7
    L63 -->|"Biên lai thu tiền"| PH
    KT -->|"Phiếu đề nghị điều chỉnh hoặc hoàn tiền"| L64
    BGH -->|"Quyết định điều chỉnh hoặc hoàn tiền"| L64
    L64 -->|"Chứng từ điều chỉnh và hoàn tiền"| D7
    D7 -->|"Phát sinh và thanh toán theo kỳ"| L65
    L65 -->|"Sổ công nợ chi tiết"| KT
    L65 -->|"Tổng hợp công nợ"| BGH
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 7.1 | F1.6.1 | UC-02 |
| 7.2 | F1.6.2 | UC-06 |
| 7.3 | F1.6.3 | UC-07 |
| 7.4 | F1.6.4 | UC-07 |
| 7.5 | F1.6.5 | UC-07 |

### Phân rã 8.0 Quản lý thực đơn và suất ăn

```mermaid
flowchart TD
    BEP["Bộ phận bếp"]
    L71(["8.1 Lập và duyệt thực đơn theo ngày, bữa"])
    BGH["Ban giám hiệu"]
    D8[("D8 · Thực đơn, suất ăn và đối soát giao nhận")]
    L72(["8.2 Thông báo thực đơn tuần, đối chiếu dị ứng và ghi lịch về sớm"])
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    PH["Phụ huynh / người nhận trẻ"]
    GV["Giáo viên"]
    L73(["8.3 Chốt danh sách và số suất ăn theo bữa"])
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    L74(["8.4 Điều chỉnh suất sau chốt và đối chiếu giao nhận"])
    BEP -->|"Thực đơn theo ngày và bữa"| L71
    BGH -->|"Quyết định duyệt thực đơn"| L71
    L71 -->|"Thực đơn đã duyệt"| D8
    D8 -->|"Thực đơn tuần có hiệu lực"| L72
    D2 -->|"Dị ứng, chế độ ăn và cảnh báo chờ xác nhận"| L72
    L72 -->|"Thực đơn tuần và thông tin món cần lưu ý"| PH
    PH -->|"Xác nhận lịch đón về trước bữa"| L72
    GV -->|"Lịch về sớm do lớp ghi nhận"| L72
    L72 -->|"Phản hồi theo trẻ và bữa, giờ về dự kiến và thời điểm ghi"| D8
    D8 -->|"Phản hồi còn hiệu lực theo trẻ và bữa, thực đơn"| L73
    D4 -->|"Danh sách có mặt theo mốc bữa và bản bàn giao sáng"| L73
    D9 -->|"Báo nghỉ và giờ về thực tế"| L73
    D1 -->|"Mốc chốt từng bữa"| L73
    D6 -->|"Phân công lớp phụ trách"| L73
    GV -->|"Bảng chốt suất theo lớp và bữa"| L73
    L73 -->|"Chi tiết suất đã chốt"| D8
    L73 -->|"Số suất chốt theo lớp và bữa"| BEP
    GV -->|"Đề nghị đổi suất và số thực nhận"| L74
    BEP -->|"Xác nhận điều chỉnh và số thực giao"| L74
    D8 -->|"Số suất đã chốt theo bữa"| L74
    L74 -->|"Chênh lệch giao nhận và kết quả đối soát"| D8
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 8.1 | F1.7.1 | UC-18 |
| 8.2 | F1.7.2 | UC-18 |
| 8.3 | F1.7.3 | UC-15 |
| 8.4 | F1.7.4 | UC-15 |

### Phân rã 9.0 Ghi nhận đón trả và báo nghỉ

```mermaid
flowchart TD
    PH["Phụ huynh / người nhận trẻ"]
    L81(["9.1 Ghi nhận báo nghỉ"])
    VP["Văn phòng"]
    GV["Giáo viên"]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    L82(["9.2 Đón trẻ và chuyển lượt đón sang NV03"])
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    TR["Nhân sự trực được cấp quyền"]
    P3(["4.0 Chốt và điều chỉnh chuyên cần"])
    L83(["9.3 Xác minh và duyệt nhận trẻ hộ"])
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    BGH["Ban giám hiệu"]
    DX["Người được giao duyệt ngoại lệ giao nhận"]
    L84(["9.4 Trả trẻ"])
    L85(["9.5 Tra cứu đón trả"])
    L86(["9.6 Hủy lượt giao nhận ghi nhầm"])
    PH -->|"Thông tin báo nghỉ"| L81
    VP -->|"Phiếu báo nghỉ do văn phòng tiếp nhận"| L81
    GV -->|"Phiếu báo nghỉ của lớp"| L81
    L81 -->|"Báo nghỉ theo trẻ, khoảng ngày và lý do"| D9
    D3 -->|"Danh sách trẻ thuộc lớp theo ngày"| L82
    D6 -->|"Ca phụ trách và dạy thay"| L82
    D1 -->|"Quyền trực theo lớp và thời gian"| L82
    GV -->|"Danh sách trẻ đến lớp"| L82
    TR -->|"Danh sách trẻ đến trong ca trực"| L82
    L82 -->|"Lượt đón có giờ và người ghi"| D9
    L82 -->|"Lượt đón phát sinh sau khóa"| P3
    PH -->|"Thông tin người nhận hộ đột xuất"| L83
    D2 -->|"Giám hộ và ủy quyền còn hiệu lực"| L83
    GV -->|"Kết quả xác minh người nhận hộ"| L83
    TR -->|"Chứng cứ xác minh trong ca trực"| L83
    BGH -->|"Quyết định duyệt ngoại lệ nhận hộ"| L83
    DX -->|"Quyết định nhận hộ theo quyền được giao"| L83
    L83 -->|"Ngoại lệ đã duyệt cho lượt trả"| L84
    D2 -->|"Giám hộ và ủy quyền cố định theo ngày"| L84
    D9 -->|"Lượt đón còn hiệu lực"| L84
    D6 -->|"Giáo viên phụ trách tại thời điểm trả"| L84
    D1 -->|"Phạm vi quyền trực giao trả"| L84
    GV -->|"Thông tin bàn giao tại lớp"| L84
    TR -->|"Thông tin bàn giao trong ca trực"| L84
    L84 -->|"Lượt trả và giờ về thực tế"| D9
    D9 -->|"Nhật ký giao nhận theo phạm vi"| L85
    L85 -->|"Sổ đón trả toàn trường"| VP
    L85 -->|"Sổ đón trả của lớp"| GV
    L85 -->|"Tổng hợp giao nhận"| BGH
    GV -->|"Lý do hủy lượt sai"| L86
    TR -->|"Lý do hủy lượt do nhân sự trực ghi"| L86
    BGH -->|"Quyết định hủy lượt trả"| L86
    DX -->|"Quyết định hủy lượt trả theo ủy quyền"| L86
    L86 -->|"Dấu hủy và lịch sử lượt gốc"| D9
    L86 -->|"Lượt đón bị hủy sau khóa"| P3
    D9 -->|"Trạng thái lượt gốc và lượt liên quan"| L82
    D9 -->|"Trạng thái lượt gốc và lượt liên quan"| L86
    L83 -->|"Kết quả xác minh và quyết định ngoại lệ kể cả từ chối"| D9
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 9.1 | F1.8.1 | UC-08 |
| 9.2 | F1.8.2 | UC-08 |
| 9.3 | F1.8.3 | UC-09 |
| 9.4 | F1.8.4 | UC-09 |
| 9.5 | F1.8.5 | UC-08, UC-09 |
| 9.6 | F1.8.6 | UC-08, UC-09 |

### Phân rã 10.0 Lập và duyệt báo cáo

```mermaid
flowchart TD
    D1[("D1 · Tài khoản, quyền, danh mục và cấu hình hiệu lực")]
    NL["Người lập báo cáo của bộ phận"]
    L91(["10.1 Lập và ghi chú báo cáo"])
    L92(["10.2 Gửi báo cáo"])
    D10[("D10 · Phiên bản, ảnh chụp và phản hồi báo cáo")]
    L93(["10.3 Xem, phản hồi, duyệt"])
    BGH["Ban giám hiệu"]
    L94(["10.4 Tổng hợp toàn trường"])
    D2[("D2 · Hồ sơ trẻ, giám hộ, ủy quyền và lưu ý")]
    D3[("D3 · Lịch sử lớp, diện đi học và sĩ số")]
    D4[("D4 · Chuyên cần, khóa ngày và điều chỉnh")]
    D5[("D5 · Hồ sơ và biến động nhân sự")]
    D6[("D6 · Phân công, nghỉ và dạy thay")]
    D7[("D7 · Học phí, thanh toán và công nợ theo kỳ")]
    D8[("D8 · Thực đơn, suất ăn và đối soát giao nhận")]
    D9[("D9 · Báo nghỉ và sổ đón trả")]
    NL -->|"Kỳ báo cáo và ghi chú bộ phận"| L91
    L91 -->|"Bản nháp và nguồn số liệu theo kỳ"| L92
    NL -->|"Thông tin xác nhận gửi phiên bản"| L92
    L92 -->|"Ảnh chụp số liệu tại lúc gửi"| D10
    D10 -->|"Phiên bản đã nộp và trạng thái"| L93
    BGH -->|"Phản hồi, yêu cầu bổ sung và quyết định duyệt"| L93
    L93 -->|"Phản hồi và trạng thái phiên bản"| D10
    L93 -->|"Cảnh báo nguồn đổi và yêu cầu bổ sung"| NL
    D10 -->|"Phiên bản đã duyệt mới nhất và cờ cập nhật"| L94
    L94 -->|"Báo cáo tổng hợp toàn trường"| BGH
    D2 -->|"Số liệu tuyển sinh đã chốt theo kỳ"| L91
    D2 -->|"Thay đổi nguồn tuyển sinh ảnh hưởng phiên bản"| L93
    D3 -->|"Số liệu sĩ số đã chốt theo kỳ"| L91
    D3 -->|"Thay đổi nguồn sĩ số ảnh hưởng phiên bản"| L93
    D4 -->|"Số liệu chuyên cần đã chốt theo kỳ"| L91
    D4 -->|"Thay đổi nguồn chuyên cần ảnh hưởng phiên bản"| L93
    D5 -->|"Số liệu nhân sự đã chốt theo kỳ"| L91
    D5 -->|"Thay đổi nguồn nhân sự ảnh hưởng phiên bản"| L93
    D6 -->|"Số liệu ca phân công đã chốt theo kỳ"| L91
    D6 -->|"Thay đổi nguồn ca phân công ảnh hưởng phiên bản"| L93
    D7 -->|"Số liệu thu phí và công nợ đã chốt theo kỳ"| L91
    D7 -->|"Thay đổi nguồn thu phí và công nợ ảnh hưởng phiên bản"| L93
    D8 -->|"Số liệu suất ăn đã chốt theo kỳ"| L91
    D8 -->|"Thay đổi nguồn suất ăn ảnh hưởng phiên bản"| L93
    D9 -->|"Số liệu giao nhận trẻ đã chốt theo kỳ"| L91
    D9 -->|"Thay đổi nguồn giao nhận trẻ ảnh hưởng phiên bản"| L93
    L91 -->|"Bản nháp và ghi chú theo dòng"| D10
    D10 -->|"Bản nháp của người lập"| L91
    D1 -->|"Loại báo cáo, chỉ tiêu, bộ phận phải nộp và hạn gửi"| L91
    D1 -->|"Loại báo cáo, chỉ tiêu, bộ phận phải nộp và hạn gửi"| L94
```

| Tiến trình con | Lá BFD | Use Case |
| --- | --- | --- |
| 10.1 | F1.9.1 | UC-10 |
| 10.2 | F1.9.2 | UC-10 |
| 10.3 | F1.9.3 | UC-10 |
| 10.4 | F1.9.4 | UC-10 |

## 4. Quy tắc tại năm điểm bàn giao

| Điểm bàn giao | Căn cứ và tác động khi dữ liệu đổi |
| --- | --- |
| Đón trả → chuyên cần | Lượt đón hợp lệ là nguồn duy nhất của Có mặt. Báo nghỉ hỗ trợ xác minh. Ghi hoặc hủy lượt đón sau khóa phát sinh đề nghị UC-14. Bản bàn giao 08:30 và khóa ngày 09:00 theo cấu hình hiệu lực (QT-DT-01, QT-DD-03). |
| Hồ sơ → trả trẻ | Kiểm tra giám hộ hoặc thời hạn ủy quyền tại ngày trả. Nhận hộ đột xuất cần xác minh và quyết định duyệt cho chính lượt đó (QT-HS-05, QT-DT-04). |
| Xếp lớp ↔ phân công | Đọc lớp hoạt động và sĩ số tại ngày hiệu lực. Lớp đóng hoặc đổi sĩ số làm phát sinh cảnh báo phân công. Chiều ngược lại cung cấp căn cứ kiểm tra định mức lớp đích (QT-PC-01, QT-PC-03). |
| Xếp lớp, chuyên cần, suất ăn và công nợ | NV07 dùng danh sách bàn giao, báo nghỉ và giờ về để vận hành bữa ăn. NV06 tạm thu theo ngày dự kiến thuộc diện đi học, quyết toán theo ngày Có mặt đã khóa của NV03. Số suất bếp giao chỉ dùng để đối soát vận hành. Treo khi còn ngày chưa khóa hoặc đề nghị có thể đổi Có mặt. Hồi tố dẫn đến bù trừ tham chiếu kỳ gốc, hoặc điều chỉnh/hoàn khi thôi học. Nợ cũ vẫn thuộc bảng cũ và chỉ hiển thị trên thông báo mới (QT-DD-07, QT-HP-02, QT-HP-05, QT-HP-06). |
| Nguồn → báo cáo | D1 xác định nghĩa vụ nộp và hạn gửi. Báo cáo đột xuất có mã và lịch sử phiên riêng theo sự việc. Khi nguồn đổi, dùng phiên nguồn và đối chiếu phạm vi cũ/mới với điều kiện lọc đã lưu, kể cả chỉ tiêu ban đầu bằng 0. Giữ ảnh chụp lúc gửi. Với Đã gửi, cảnh báo người lập và chuyển BGH xử lý yêu cầu bổ sung. Với Đã duyệt, chuyển Cần cập nhật. Tổng hợp dùng phiên bản đã duyệt mới nhất và đánh dấu đang cập nhật (QT-BC-02, QT-BC-04, QT-BC-05). |

## 5. Kiểm tra cân bằng và chuyển sang ERD

Ở mức đỉnh, các thành phần cùng nguồn, đích và chiều sau đây đi chung một gói. Khi đối chiếu mức dưới, thay nhãn gói bằng từng thành phần trong bảng.

| Gói mức đỉnh | Thành phần ở mức dưới |
| --- | --- |
| Số liệu và thay đổi nguồn D2 | Số liệu tuyển sinh đã chốt theo kỳ · Thay đổi nguồn tuyển sinh ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D3 | Số liệu sĩ số đã chốt theo kỳ · Thay đổi nguồn sĩ số ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D4 | Số liệu chuyên cần đã chốt theo kỳ · Thay đổi nguồn chuyên cần ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D5 | Số liệu nhân sự đã chốt theo kỳ · Thay đổi nguồn nhân sự ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D6 | Số liệu ca phân công đã chốt theo kỳ · Thay đổi nguồn ca phân công ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D7 | Số liệu thu phí và công nợ đã chốt theo kỳ · Thay đổi nguồn thu phí và công nợ ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D8 | Số liệu suất ăn đã chốt theo kỳ · Thay đổi nguồn suất ăn ảnh hưởng phiên bản |
| Số liệu và thay đổi nguồn D9 | Số liệu giao nhận trẻ đã chốt theo kỳ · Thay đổi nguồn giao nhận trẻ ảnh hưởng phiên bản |
| Hồ sơ tiếp nhận và giấy tờ theo đợt | Mã trẻ, đợt tiếp nhận và quyết định duyệt · Giấy tờ từng đợt và lịch hẹn bổ sung |
| Nội dung phiên báo cáo | Ảnh chụp số liệu tại lúc gửi · Bản nháp và ghi chú theo dòng |
| Phiên báo cáo theo quyền và trạng thái | Phiên bản đã nộp và trạng thái · Phiên bản đã duyệt mới nhất và cờ cập nhật |

Mức 0 có 17 gói theo tác nhân và chiều, tương ứng 86 luồng chi tiết với tác nhân ở mức 1. Khi thay mỗi tiến trình con bằng tiến trình cha, bỏ cạnh nội bộ và gộp thành phần theo bảng trên, mức 2 khôi phục đủ 179 cạnh phân biệt của mức 1. Cả 43 lá thuộc 10 tiến trình cha đều có đầu vào và đầu ra.

Các thực thể ứng viên gồm Trẻ, Người giám hộ, Giấy ủy quyền, Lưu ý chăm sóc, Lớp, Lịch sử xếp lớp, Nhân sự, Phân công, Ca dạy thay, Tài khoản, Quyền theo phạm vi, Báo nghỉ, Lượt giao nhận, Phiếu duyệt ngoại lệ, Chuyên cần ngày, Đề nghị điều chỉnh, Thực đơn, Bữa ăn, Chi tiết suất, Phiếu giao nhận suất, Kỳ thu, Khoản phải thu, Phiếu thu, Phân bổ thanh toán, Chứng từ điều chỉnh, Báo cáo và Phiên bản báo cáo. Khi dựng quan hệ cần giữ ngày hiệu lực của xếp lớp, kỳ gốc của khoản nợ và phiên bản nguồn tại lúc gửi.
