# UC-07 · Ghi thu tiền và in biên lai

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV06 · F1.6.3, F1.6.4, F1.6.5 |
| Tác nhân chính | Kế toán |
| Tác nhân phụ | Ban giám hiệu (duyệt phiếu điều chỉnh và hoàn tiền) |
| Mục tiêu | Ghi mỗi lần phụ huynh nộp tiền, phân bổ vào các bảng phải thu còn nợ, in biên lai và cập nhật công nợ. Xử lý sai sót bằng phiếu điều chỉnh hoặc hoàn tiền có duyệt, không xóa biên lai. |
| Điều kiện trước | Kế toán đăng nhập hệ thống con Kế toán. Trẻ có ít nhất một bảng phải thu `Đã duyệt` từ [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md), hoặc có số dư có cần xử lý. |
| Điều kiện sau | Mỗi lần thu có một biên lai duy nhất, ghi số tiền phân bổ cho từng bảng phải thu. Phần thừa được ghi thành số dư có. Công nợ theo trẻ, lớp, kỳ được cập nhật. Phiếu điều chỉnh, hoàn tiền có người duyệt và được lưu lịch sử. |

## Luồng sự kiện chính

1. Kế toán tìm trẻ theo mã trẻ, họ tên hoặc mã nội dung chuyển khoản theo trẻ và kỳ (YC-HP-03).
2. Hệ thống hiển thị các bảng phải thu còn nợ theo thứ tự kỳ, số dư có hiện có và tổng còn phải thu.
3. Kế toán nhập số tiền, ngày thu, hình thức (tiền mặt hoặc chuyển khoản), mã giao dịch nếu chuyển khoản và người nộp. Người nộp mặc định là người giám hộ chính đọc từ NV01.
4. Hệ thống phân bổ số tiền vào bảng phải thu còn nợ của kỳ cũ nhất trước, rồi đến kỳ kế tiếp. Phần còn thừa sau khi trả hết các bảng còn nợ được ghi thành số dư có của trẻ (QT-HP-07).
5. Kế toán xem lại phân bổ và xác nhận. Hệ thống ghi lần thu, các dòng phân bổ, số dư có và biên lai trong một giao dịch (NFR-05). Biên lai có số duy nhất và không xóa được (QT-HP-04).
6. Hệ thống cập nhật còn nợ của từng bảng phải thu bằng tổng phải thu trừ tổng đã thu (QT-HP-04).
7. Kế toán in biên lai (HP-04) cho phụ huynh: số biên lai, trẻ, kỳ, số tiền phân bổ cho từng bảng, ngày thu, hình thức, người thu.

## Luồng con: điều chỉnh và hoàn tiền (F1.6.4)

1. Kế toán lập phiếu điều chỉnh khi biên lai hoặc bảng phải thu có sai sót, hoặc phiếu hoàn tiền khi phụ huynh đề nghị hoàn số dư có hoặc trẻ thôi học. Phiếu ghi số tiền, lý do và tham chiếu biên lai hoặc bảng phải thu gốc.
2. Ban giám hiệu duyệt hoặc từ chối kèm lý do.
3. Khi duyệt, hệ thống ghi phiếu, cập nhật công nợ và số dư có. Biên lai gốc giữ nguyên, có liên kết đến phiếu điều chỉnh (QT-HP-04, NFR-04).

## Luồng con: theo dõi công nợ (F1.6.5)

1. Kế toán mở danh sách công nợ, lọc theo lớp, kỳ, trạng thái quá hạn.
2. Hệ thống hiển thị phải thu, đã thu, còn nợ và số ngày quá hạn theo trẻ. Còn nợ được chuyển sang kỳ sau khi lập bảng phải thu tiếp theo.
3. Kế toán xuất danh sách nhắc nợ. Tổng phải thu, đã thu, còn nợ, miễn giảm, hoàn tiền trong kỳ là số liệu cho báo cáo thu tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md) (YC-HP-04).

## Luồng thay thế và ngoại lệ

- **Bước 1 (mã chuyển khoản không khớp):** Nội dung chuyển khoản không đúng mẫu. Kế toán đối chiếu thủ công theo sao kê và tìm trẻ theo họ tên. Chưa xác định được trẻ thì chưa ghi lần thu.
- **Bước 3 (nộp thiếu):** Hệ thống phân bổ hết số tiền, bảng phải thu còn nợ phần còn lại.
- **Bước 3 (nộp gộp nhiều kỳ):** Hệ thống phân bổ lần lượt theo kỳ, biên lai ghi rõ số tiền cho từng bảng.
- **Bước 3 (số tiền không hợp lệ):** Số tiền bằng 0 hoặc âm bị chặn ở giao diện và tầng nghiệp vụ (NFR-06).
- **Bước 5 (sai sót sau khi ghi):** Không sửa hoặc xóa biên lai. Kế toán lập phiếu điều chỉnh theo luồng con.
- **Số dư có ở kỳ sau:** Khi lập bảng phải thu kỳ kế tiếp tại `UC-06`, số dư có được tự trừ. Số dư có chỉ được hoàn khi phụ huynh đề nghị hoặc trẻ thôi học, có người duyệt (QT-HP-07).
- **Trẻ thôi học:** Khi NV02 ghi thôi học tại `UC-17`, hệ thống nhắc Kế toán xử lý số dư có và chênh lệch tiền ăn chưa bù trừ bằng phiếu điều chỉnh hoặc hoàn tiền (QT-HP-05).

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Trẻ, mã nội dung chuyển khoản. Bảng phải thu `Đã duyệt` còn nợ, số dư có (UC-06). Người giám hộ chính (NV01, chỉ đọc). Số tiền, ngày thu, hình thức, mã giao dịch, người nộp. Lý do, số tiền điều chỉnh hoặc hoàn tiền. Quyết định duyệt. | Lần thu và các dòng phân bổ theo bảng phải thu. Biên lai có số duy nhất. Số dư có của trẻ. Còn nợ theo bảng phải thu. Phiếu điều chỉnh, phiếu hoàn tiền có người duyệt. Danh sách công nợ và nhắc nợ. Tổng thu trong kỳ. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Thu đủ một kỳ | PT202610-042 phải nộp 3.500.000 đồng, chuyển khoản CK-MP-20261005-042 ngày 05/10/2026 ([Mẫu 04](../../survey/forms/mau-04-hoc-phi-va-bien-lai.md)) | Biên lai BL202610-042 thực thu 3.500.000 đồng. Còn phải thu của bảng bằng 0. |
| Nộp thừa, phân bổ kỳ cũ trước | Còn nợ tháng 09 là 200.000 đồng, thông báo tháng 10 là 3.500.000 đồng, phụ huynh chuyển 4.000.000 đồng | Phân bổ 200.000 cho tháng 09, 3.500.000 cho tháng 10, ghi số dư có 300.000 đồng (QT-HP-07). |
| Nộp thiếu | Phải nộp 3.500.000 đồng, nộp 3.000.000 đồng | Còn nợ 500.000 đồng, chuyển sang kỳ sau. |
| Sửa biên lai | Kế toán ghi nhầm số tiền trên một biên lai | Không xóa được. Lập phiếu điều chỉnh, Ban giám hiệu duyệt, công nợ cập nhật. |
| Hoàn số dư có khi thôi học | Trẻ thôi học còn số dư có 300.000 đồng | Phiếu hoàn tiền 300.000 đồng có duyệt, số dư có về 0. |

## Liên phân hệ và truy vết

- **NV06 nội bộ:** Bảng phải thu `Đã duyệt` và số dư có từ [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md).
- **NV01 → NV06:** Người giám hộ chính để ghi người nộp trên biên lai, chỉ đọc từ `UC-16`.
- **NV02 → NV06:** Trẻ `Thôi học` từ `UC-17` kích hoạt xử lý số dư có và chênh lệch tiền ăn (QT-HP-05, QT-HP-07).
- **NV06 → NV09:** Tổng thu, công nợ, miễn giảm, hoàn tiền cho báo cáo thu của Kế toán tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HP-03 | QT-HP-04, QT-HP-07 | NV06 · Ghi thu nhiều hình thức, phân bổ kỳ cũ trước, số dư có, biên lai. |
| YC-HP-04 | QT-HP-04, QT-HP-05 | NV02 → NV06 → NV09 · Điều chỉnh, hoàn tiền có duyệt, công nợ và báo cáo thu. |
| YC-HP-05 | QT-HP-04 | NV01 → NV06 · Dữ liệu người giám hộ chỉ đọc. |
