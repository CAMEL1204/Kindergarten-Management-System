# UC-06 · Lập bảng phải thu theo kỳ

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV06 · F1.6.2 (đọc F1.6.1) |
| Tác nhân chính | Kế toán |
| Tác nhân phụ | Ban giám hiệu (duyệt bảng phải thu) |
| Mục tiêu | Lập một bảng phải thu cho mỗi trẻ trong kỳ thu: khoản tháng, tạm thu tiền ăn, bù trừ tiền ăn kỳ trước, miễn giảm và số dư có. Sau khi duyệt, phát hành thông báo học phí cho phụ huynh, kèm nợ kỳ trước còn lại. |
| Điều kiện trước | Kế toán đăng nhập hệ thống con Kế toán. Biểu phí có hiệu lực tại kỳ thu và miễn giảm đã duyệt (F1.6.1, đặc tả màn hình tại `UC-02`). Lịch học của kỳ đã có. NV02 cung cấp khoảng thời gian trẻ thuộc diện đi học. Với phần đối soát, hệ thống đọc số ngày `Có mặt` đã khóa của kỳ trước từ NV03. Trẻ còn ngày chưa khóa được xử lý ở luồng thay thế bước 6 (QT-DD-07). |
| Điều kiện sau | Mỗi trẻ thuộc diện đi học trong kỳ có tối đa một bảng phải thu (QT-HP-01). Mỗi dòng lưu căn cứ tính: số ngày, đơn giá, kỳ phát sinh. Khoản đối soát của trẻ còn ngày chưa khóa hoặc còn đề nghị chờ duyệt có thể đổi kết quả `Có mặt` ở trạng thái chờ xử lý. Bảng đã duyệt được phát hành thông báo và là căn cứ cho [UC-07](uc-07-ghi-thu-tien-va-in-bien-lai.md). |

## Luồng sự kiện chính

1. Kế toán chọn kỳ thu và bấm "Lập bảng phải thu". Hệ thống chỉ lập cho trẻ chưa có bảng phải thu của kỳ này (QT-HP-01).
2. Hệ thống lấy danh sách trẻ thuộc diện đi học trong kỳ từ NV02, kèm lớp, nhóm tuổi và khoảng hiệu lực. Hệ thống đọc định danh trẻ và người giám hộ chính từ NV01 ở chế độ chỉ đọc (YC-HP-05).
3. Với từng trẻ, hệ thống chọn biểu phí có hiệu lực tại kỳ thu theo nhóm tuổi hoặc lớp (QT-HP-01).
4. Hệ thống tính khoản tháng. Trẻ thuộc diện đi học cả kỳ thu đủ. Trẻ vào học, thôi học, bắt đầu hoặc kết thúc bảo lưu trong kỳ tính theo tỷ lệ: đơn giá tháng × số ngày có lịch học thuộc diện đi học ÷ số ngày có lịch học của kỳ, làm tròn đến nghìn đồng. Nghỉ dài ngày thu đủ. Số ngày dùng để tính được lưu trên dòng (QT-HP-06, YC-HP-06).
5. Hệ thống tạm thu tiền ăn theo số ngày có lịch học mà trẻ thuộc diện đi học trong kỳ (QT-HP-02).
6. Hệ thống đối soát tiền ăn kỳ trước của từng trẻ. Chênh lệch bằng số ngày `Có mặt` đã khóa của NV03 nhân đơn giá kỳ phát sinh, trừ tiền đã tạm tính và các khoản bù trừ đã phát hành. Số âm giảm trừ, số dương thu bổ sung. Dòng bù trừ ghi tham chiếu kỳ phát sinh (QT-HP-05).
7. Hệ thống áp miễn giảm đã duyệt còn hiệu lực trong kỳ. Tổng phải thu sau miễn giảm không âm (QT-HP-03). Hệ thống trừ số dư có của trẻ vào bảng mới (QT-HP-07). Nợ kỳ trước vẫn thuộc bảng phải thu của kỳ đó, không thành dòng của bảng mới, để một khoản nợ không bị thu hai lần (QT-HP-04).
8. Hệ thống lưu toàn bộ bảng phải thu của kỳ ở trạng thái `Nháp` trong một giao dịch (NFR-05). Màn hình hiển thị tổng theo lớp, toàn trường và danh sách trẻ có khoản chờ xử lý.
9. Kế toán rà soát, sửa ghi chú hoặc chạy lại cho trẻ có căn cứ thay đổi, rồi trình duyệt.
10. Ban giám hiệu xem tổng phải thu và các trường hợp đặc biệt, rồi duyệt. Hệ thống chuyển bảng sang `Đã duyệt` và khóa nội dung.
11. Kế toán phát hành thông báo học phí (HP-03) cho từng trẻ: chi tiết khoản thu, tổng tiền, nợ kỳ trước còn lại nếu có, hạn nộp, mã nội dung chuyển khoản theo trẻ và kỳ (YC-HP-03). Nợ kỳ trước chỉ hiển thị để phụ huynh biết tổng cần nộp. Tiền nộp được phân bổ vào bảng kỳ cũ trước tại [UC-07](uc-07-ghi-thu-tien-va-in-bien-lai.md) (QT-HP-07). Thông báo được in hoặc gửi qua kênh liên lạc của trường.

## Luồng thay thế và ngoại lệ

- **Bước 1 (trẻ đã có bảng kỳ này):** Hệ thống không tạo bảng thứ hai. Thay đổi sau duyệt đi qua phiếu điều chỉnh tại `UC-07` (QT-HP-04).
- **Bước 3 (không có biểu phí hiệu lực):** Hệ thống chặn lập cho nhóm tuổi hoặc lớp đó và báo khoản thu còn thiếu.
- **Bước 6 (đối soát bị treo):** Khoản đối soát của trẻ để ở trạng thái chờ xử lý khi có một trong hai điều kiện: trẻ còn ngày chưa khóa trong kỳ phát sinh, hoặc trẻ còn ít nhất một đề nghị điều chỉnh chờ duyệt tại `UC-14` có thể đổi kết quả `Có mặt` (QT-HP-05, QT-DD-07). Hệ thống cảnh báo Kế toán. Phần còn lại của bảng vẫn được lập.
- **Bước 6 (gỡ treo):** Hệ thống chỉ gỡ treo khi cả hai điều kiện cùng thỏa: mọi ngày liên quan của trẻ trong kỳ phát sinh đã khóa, và không còn đề nghị chờ duyệt nào có thể đổi kết quả `Có mặt`. Sau khi gỡ treo, Kế toán chạy lại đối soát cho trẻ đó. Lượt `Chờ xác minh` không có lượt đón tính 0 ngày và không làm treo.
- **Bước 6 (chạy lại đối soát):** Hệ thống chỉ ghi phần chênh lệch còn lại sau các khoản bù trừ đã phát hành. Chạy lại trên cùng dữ liệu không sinh thêm dòng.
- **Bước 6 (trẻ đã thôi học):** Nếu trẻ không còn kỳ thu tiếp theo, chênh lệch chuyển sang luồng điều chỉnh hoặc hoàn tiền tại `UC-07`.
- **Bước 10 (trả lại):** Ban giám hiệu trả lại kèm ý kiến. Bảng về `Nháp`.
- **Thay đổi nguồn hồi tố:** Khi NV02 ghi hồi tố hoặc đổi nhóm tuổi ảnh hưởng kỳ đã lập, hệ thống thông báo đối soát lại cho Kế toán. Chênh lệch được bù trừ ở kỳ sau, không sửa bảng đã duyệt.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Kỳ thu. Biểu phí và miễn giảm đã duyệt. Lịch học của kỳ. Trẻ, lớp, nhóm tuổi, khoảng thuộc diện đi học (NV02). Mã trẻ, họ tên, người giám hộ chính, quan hệ, số điện thoại (NV01, chỉ đọc). Số ngày `Có mặt` đã khóa và cờ đề nghị chờ duyệt theo trẻ (NV03). Tiền ăn đã tạm tính, khoản bù trừ đã phát hành, nợ cũ, số dư có (NV06). | Bảng phải thu theo trẻ và kỳ, có trạng thái `Nháp`, `Chờ duyệt`, `Đã duyệt`. Dòng khoản thu kèm số ngày, đơn giá, thành tiền. Dòng bù trừ tiền ăn kèm kỳ phát sinh. Danh sách khoản chờ xử lý. Thông báo học phí. Tổng phải thu theo lớp và toàn trường. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Khoản tháng theo tỷ lệ | Trẻ vào học 07/09, kỳ 09 có 20 ngày học, trẻ thuộc diện 18 ngày | Học phí cơ bản 2.500.000 × 18 ÷ 20 = 2.250.000 đồng. Phụ phí 300.000 × 18 ÷ 20 = 270.000 đồng (QT-HP-06). |
| Bù trừ tiền ăn | HS2026-042, tạm tính 18 ngày tháng 09, 16 ngày có mặt, đơn giá 35.000 đồng ([Mẫu 04](../../survey/forms/mau-04-hoc-phi-va-bien-lai.md)) | Dòng giảm 70.000 đồng ở kỳ 10, tham chiếu kỳ phát sinh 09/2026. |
| Bảng phải thu tháng 10 | PT202610-042: học phí 2.500.000, tạm thu 22 ngày 770.000, phụ phí 300.000, bù trừ −70.000 | Tổng phải nộp 3.500.000 đồng. |
| Chạy lại đối soát | Chạy lại kỳ 09 trên cùng dữ liệu | Không ghi thêm khoản giảm 70.000 đồng. |
| Treo do đề nghị chờ duyệt | Trẻ có đề nghị đổi `Chờ xác minh` sang `Có mặt` còn chờ duyệt | Khoản đối soát của trẻ ở trạng thái chờ xử lý, các trẻ khác vẫn lập bình thường. |
| Treo do ngày chưa khóa | Trẻ không có đề nghị nào nhưng ngày học cuối kỳ phát sinh chưa qua mốc khóa | Khoản đối soát của trẻ vẫn ở trạng thái chờ xử lý, không gỡ treo. |
| Gỡ treo | Đề nghị của trẻ đã được duyệt và mọi ngày trong kỳ phát sinh đã khóa | Gỡ treo. Đối soát lại chỉ ghi phần chênh lệch còn lại. |
| Miễn giảm vượt tổng | Trẻ có hai loại miễn giảm cộng lại lớn hơn khoản thu | Tổng phải thu sau miễn giảm bằng 0, không âm (QT-HP-03). |

## Liên phân hệ và truy vết

- **NV01 → NV06:** Định danh trẻ và người giám hộ chính từ `UC-16`, chỉ đọc (YC-HS-04). Kế toán không xem lưu ý chăm sóc và ủy quyền nhận trẻ.
- **NV02 → NV06:** Khoảng thuộc diện đi học từ `UC-12`, `UC-17`. Nghỉ dài ngày thu đủ khoản tháng. Bảo lưu và thôi học tính tỷ lệ (QT-HP-02, QT-HP-06).
- **NV03 → NV06:** Số ngày `Có mặt` đã khóa, gồm điều chỉnh đã duyệt, từ `UC-13`, `UC-14`. Treo và gỡ treo đối soát theo QT-DD-07, QT-HP-05.
- **F1.0 → NV06:** Lịch học, kỳ thu, biểu phí và miễn giảm đã duyệt từ `UC-02` (QT-HP-01, QT-HP-03).
- **NV06 → NV09:** Tổng phải thu theo kỳ cho báo cáo thu của Kế toán tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HP-02 | QT-HP-02, QT-HP-05, QT-DD-07 | NV02, NV03 → NV06 · Tạm thu tiền ăn, đối soát và bù trừ có tham chiếu kỳ phát sinh, treo khi còn ngày chưa khóa hoặc đề nghị chờ duyệt, gỡ treo khi đã khóa đủ và hết đề nghị. |
| YC-HP-06 | QT-HP-06 | NV02 → NV06 · Khoản tháng theo tỷ lệ khi vào học, thôi học, bảo lưu trong kỳ. |
| YC-HP-01, YC-HP-05 | QT-HP-01, QT-HP-03 | F1.0, NV01 → NV06 · Biểu phí hiệu lực, miễn giảm đã duyệt, dữ liệu chỉ đọc. |
| YC-HP-04 | QT-HP-04, QT-HP-07 | NV06 → NV09 · Hiển thị nợ cũ trên thông báo, trừ số dư có và tổng hợp phải thu. |
