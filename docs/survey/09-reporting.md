# 09. Lập, ghi chú và gửi báo cáo

> Bản khảo sát sơ bộ. Loại báo cáo, hạn gửi và các trạng thái dưới đây là phương án đề xuất, chờ cơ sở khảo sát xác nhận.

## 1. Mục đích nghiệp vụ

Báo cáo đi **từ bộ phận lên Ban giám hiệu**. Mỗi bộ phận (giáo viên, văn phòng, kế toán, bếp) tự lập báo cáo trong phạm vi công việc của mình, thêm ghi chú để giải thích số liệu, rồi **gửi** cho Ban giám hiệu. Ban giám hiệu **không có báo cáo sẵn** của các bộ phận: Ban giám hiệu chỉ nhận các báo cáo đã gửi, xem, phản hồi, yêu cầu bổ sung hoặc duyệt. Số liệu toàn trường được tổng hợp từ các báo cáo đã duyệt.

## 2. Đối tượng tham gia

| Đối tượng | Công việc |
| --- | --- |
| Người lập báo cáo (giáo viên, văn phòng, kế toán, bếp) | Tạo báo cáo từ số liệu đã chốt, thêm ghi chú, đính kèm minh chứng, gửi và sửa khi được yêu cầu bổ sung. |
| Ban giám hiệu (admin) | Nhận báo cáo, xem, phản hồi, yêu cầu bổ sung hoặc duyệt, theo dõi báo cáo nộp muộn. |

## 3. Loại báo cáo theo hệ thống con

| Hệ thống con | Báo cáo | Kỳ |
| --- | --- | --- |
| Giáo viên | Chuyên cần của lớp. Tình hình trẻ trong lớp (sức khỏe, sự việc cần lưu ý). | Tuần hoặc tháng; đột xuất khi có sự việc |
| Văn phòng | Tuyển sinh, hồ sơ chờ bổ sung. Sĩ số và biến động. Nhân sự (vào làm, nghỉ việc, chứng chỉ hết hạn). Phân công và dạy thay. | Tháng |
| Kế toán | Thu học phí: phải thu, đã thu, còn nợ. Danh sách nợ quá hạn. Miễn giảm, hoàn tiền. | Tháng |
| Bếp | Số suất ăn đã giao theo lớp và bữa. Thực phẩm đã dùng nếu trường yêu cầu. | Tuần hoặc tháng |

Mỗi bộ phận chỉ thấy loại báo cáo của mình. Ví dụ, kế toán không có báo cáo suất ăn hay báo cáo chuyên cần.

## 4. Trạng thái của báo cáo

| Trạng thái | Ý nghĩa | Ai được sửa |
| --- | --- | --- |
| Nháp | Người lập đang soạn, Ban giám hiệu chưa thấy. | Người lập |
| Đã gửi | Đã gửi cho Ban giám hiệu, khóa nội dung. | Không ai |
| Yêu cầu bổ sung | Ban giám hiệu trả lại kèm phản hồi. | Người lập sửa rồi gửi lại |
| Đã duyệt | Ban giám hiệu chấp nhận, được dùng để tổng hợp toàn trường. | Không ai |

Luồng: **Nháp → Đã gửi → (Yêu cầu bổ sung → Đã gửi lại) → Đã duyệt.**

## 5. Quy trình nghiệp vụ

| Bước | Ai, khi nào | Công việc | Kết quả |
| --- | --- | --- | --- |
| 1. Tạo báo cáo | Người lập, cuối kỳ hoặc khi có sự việc | Chọn loại báo cáo và kỳ. Hệ thống lấy **số liệu đã chốt** trong phạm vi của người lập (ví dụ giáo viên chỉ lấy được lớp mình). Cảnh báo nếu kỳ còn dữ liệu chưa chốt. | Báo cáo nháp có số liệu. |
| 2. Ghi chú | Người lập | Thêm **ghi chú chung** cho cả báo cáo và **ghi chú theo từng dòng** (ví dụ giải thích vì sao một trẻ vắng nhiều, vì sao một khoản còn nợ). Đính kèm minh chứng nếu cần. | Báo cáo nháp có ghi chú. |
| 3. Gửi | Người lập | Xem lại rồi bấm "Gửi Ban giám hiệu". Hệ thống lưu **ảnh chụp số liệu tại lúc gửi**, ghi người gửi, thời điểm và khóa nội dung. | Báo cáo đã gửi, xuất hiện trong hộp báo cáo của Ban giám hiệu. |
| 4. Xem và phản hồi | Ban giám hiệu | Mở báo cáo trong hộp báo cáo nhận được, lọc theo bộ phận, loại, kỳ và trạng thái. Đọc số liệu và ghi chú, viết phản hồi. | Phản hồi gắn với báo cáo. |
| 5. Yêu cầu bổ sung hoặc duyệt | Ban giám hiệu | Nếu cần sửa, chọn "Yêu cầu bổ sung" kèm nội dung; người lập nhận thông báo, sửa và gửi lại thành **phiên bản mới**. Nếu đạt, chọn "Duyệt". | Báo cáo đã duyệt hoặc chờ bổ sung. |
| 6. Tổng hợp | Ban giám hiệu | Xem bảng tổng hợp toàn trường được ghép từ các báo cáo **đã duyệt**. Theo dõi bộ phận chưa gửi hoặc gửi trễ hạn. | Báo cáo toàn trường. |

## 6. Dữ liệu của một báo cáo

| Nhóm | Trường |
| --- | --- |
| Thông tin chung | Mã báo cáo, loại, kỳ, bộ phận, phạm vi (lớp, khối hoặc toàn trường), người lập |
| Số liệu | Các dòng số liệu lấy từ dữ liệu đã chốt, chụp lại tại thời điểm gửi |
| Ghi chú | Ghi chú chung; ghi chú theo dòng; tệp đính kèm |
| Luồng xử lý | Trạng thái, phiên bản, thời điểm gửi, hạn gửi, người duyệt, thời điểm duyệt |
| Phản hồi | Người phản hồi, nội dung, thời điểm, gắn với phiên bản nào |

## 7. Quy tắc nghiệp vụ

| Mã | Quy tắc cần xác nhận | Trường hợp cần thử |
| --- | --- | --- |
| QT-BC-01 | Ban giám hiệu chỉ thấy báo cáo ở trạng thái đã gửi trở đi. Báo cáo nháp chỉ người lập thấy. | Giáo viên lưu nháp, Ban giám hiệu không thấy trong hộp báo cáo. |
| QT-BC-02 | Người lập chỉ lấy được số liệu trong phạm vi của mình. | Giáo viên lớp A không tạo được báo cáo chuyên cần lớp B. |
| QT-BC-03 | Sau khi gửi, nội dung bị khóa. Muốn sửa phải được yêu cầu bổ sung và gửi lại thành phiên bản mới. Giữ đủ các phiên bản cũ và phản hồi. | Kế toán phát hiện sai sau khi gửi. |
| QT-BC-04 | Số liệu được chụp tại lúc gửi. Nếu dữ liệu gốc thay đổi sau đó (ví dụ điều chỉnh điểm danh), báo cáo đã gửi không tự đổi; hệ thống cảnh báo để người lập gửi lại nếu cần. | Điểm danh được sửa sau khi báo cáo chuyên cần đã gửi. |
| QT-BC-05 | Tổng hợp toàn trường chỉ dùng báo cáo đã duyệt. Kỳ còn bộ phận chưa duyệt được đánh dấu chưa đầy đủ. | Kế toán chưa gửi báo cáo tháng. |

## 8. Hạn chế của quy trình hiện tại

- Báo cáo được gửi qua tin nhắn hoặc giấy, Ban giám hiệu khó biết bộ phận nào đã gửi, gửi bản nào.
- Không có chỗ ghi chú nên số liệu bất thường phải giải thích riêng bằng lời.
- Nếu Ban giám hiệu tự xem dữ liệu thô thay cho báo cáo, không có xác nhận của bộ phận về số liệu.

## 9. Yêu cầu đối với hệ thống mới

| Mã | Yêu cầu đề xuất |
| --- | --- |
| YC-BC-01 | Ở mỗi hệ thống con: mục "Báo cáo của tôi" để tạo báo cáo theo loại và kỳ từ số liệu đã chốt, lưu nháp, ghi chú chung và theo dòng, đính kèm và gửi. |
| YC-BC-02 | Ở hệ thống con Ban giám hiệu: "Hộp báo cáo nhận được" có lọc theo bộ phận, loại, kỳ, trạng thái; xem, phản hồi, yêu cầu bổ sung, duyệt. |
| YC-BC-03 | Lưu phiên bản, lịch sử phản hồi và ảnh chụp số liệu tại lúc gửi. Thông báo cho người lập khi có phản hồi hoặc yêu cầu bổ sung. |
| YC-BC-04 | Theo dõi hạn gửi, cảnh báo báo cáo trễ hạn. Tổng hợp toàn trường từ báo cáo đã duyệt và xuất tệp. |
