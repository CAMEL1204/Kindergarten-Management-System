# 08. Đón trẻ, trả trẻ và tự động ghi nhận có mặt

> Bản khảo sát sơ bộ. Giờ chốt, trạng thái và cách chọn trẻ để đón trả dưới đây là phương án đề xuất, chờ cơ sở khảo sát xác nhận.

## 1. Mục đích nghiệp vụ

Ghi nhận lúc giáo viên nhận trẻ từ phụ huynh buổi sáng (đón trẻ) và giao trẻ cho người đón buổi chiều (trả trẻ). **Đón trẻ và điểm danh là một luồng dữ liệu**, không phải hai việc ghi riêng:

- Trẻ đã được đón vào lớp thì **có mặt**. Hệ thống tự ghi có mặt cho nghiệp vụ 03, giáo viên không phải điểm danh lại.
- Trẻ chưa được đón thì **chưa có mặt**. Hết giờ chốt mà vẫn chưa đón thì trẻ được ghi **vắng**.
- Trẻ vắng không ở trong lớp nên **không có trong danh sách trả trẻ**. Ở màn hình trả trẻ, các trẻ này hiển thị là vắng và không thao tác được.

## 2. Đối tượng tham gia

| Đối tượng | Công việc |
| --- | --- |
| Giáo viên được phân công lớp trong ngày (nghiệp vụ 05) | Đón trẻ, trả trẻ bằng cách chọn trẻ rồi xác nhận, xác minh trẻ vắng và chốt điểm danh. |
| Phụ huynh hoặc người được ủy quyền đón | Đưa trẻ đến, báo nghỉ, đón trẻ về. |
| Văn phòng | Cập nhật danh sách người được đón trẻ (nghiệp vụ 01), xem tình hình đón trả toàn trường. |

## 3. Trạng thái của trẻ trong ngày

| Trạng thái | Khi nào | Kết quả điểm danh (nghiệp vụ 03) | Có trong danh sách trả trẻ |
| --- | --- | --- | --- |
| Chưa đón | Mặc định đầu ngày, trước giờ chốt | Tạm tính vắng, chờ xác minh | Không |
| Báo nghỉ | Phụ huynh báo nghỉ trước hoặc trong ngày | Vắng có phép | Không |
| Đã đón | Giáo viên xác nhận đón | **Có mặt (tự động)**, ghi giờ đến | **Có** |
| Vắng | Hết giờ chốt mà chưa đón | Vắng (có phép hoặc không phép theo xác minh) | Không, hiển thị nhóm "Vắng hôm nay", không có nút trả |
| Đã trả | Giáo viên xác nhận trả | Vẫn có mặt, ghi giờ về và người đón | Đã xong, không trả lại lần nữa |

## 4. Quy trình nghiệp vụ

| Bước | Ai, ở đâu và khi nào | Công việc và cách thực hiện | Kết quả |
| --- | --- | --- | --- |
| 1. Mở danh sách đón | Giáo viên tại lớp, đầu giờ đón | Hệ thống lấy danh sách trẻ thuộc lớp trong ngày (nghiệp vụ 02). Trẻ đã báo nghỉ được tách sẵn sang trạng thái báo nghỉ. | Danh sách trẻ chưa đón. |
| 2. Đón trẻ | Giáo viên, khi phụ huynh đưa trẻ đến | Tích chọn các trẻ vừa được đưa đến (một hay nhiều trẻ đều làm cùng một cách) rồi bấm "Đón các trẻ đã chọn" (mục 5). Không có nút đón riêng trên từng dòng. Hệ thống ghi giờ đón và tự chuyển điểm danh của trẻ sang có mặt. | Trẻ ở trạng thái đã đón, có mặt. |
| 3. Chốt đón | Hệ thống và giáo viên, đến giờ chốt của trường | Trẻ còn chưa đón chuyển sang vắng. Giáo viên xác minh lý do theo nghiệp vụ 03 rồi chốt điểm danh. | Điểm danh đã chốt, lấy từ kết quả đón. |
| 4. Đón muộn | Giáo viên, khi trẻ đến sau giờ chốt | Nếu bảng chưa chốt, đón bình thường, trẻ có mặt và được đánh dấu đến muộn. Nếu bảng đã chốt, ghi đề nghị điều chỉnh theo quy tắc QT-DD-03 của nghiệp vụ 03. | Trẻ có mặt (đến muộn) hoặc đề nghị điều chỉnh. |
| 5. Trả trẻ | Giáo viên, giờ trả trẻ hoặc khi phụ huynh đón sớm | Danh sách trả **chỉ gồm trẻ đã đón và chưa trả**. Tích chọn các trẻ được đón về rồi bấm "Trả các trẻ đã chọn". Ghi giờ trả và người đón, đối chiếu với danh sách người được đón trẻ. | Trẻ ở trạng thái đã trả. |
| 6. Kết thúc ngày | Giáo viên, sau giờ trả | Hệ thống cảnh báo nếu còn trẻ đã đón mà chưa trả. Giáo viên xử lý trước khi kết thúc ca. | Không còn trẻ ở lớp mà chưa được ghi trả. |

## 5. Chọn trẻ để đón hoặc trả

Đây là **cách duy nhất** để đón và trả trẻ. Không có nút đón hoặc trả riêng trên từng dòng. Muốn đón một trẻ thì chỉ chọn một trẻ, muốn đón cả nhóm thì chọn cả nhóm, nên giáo viên chỉ cần học một thao tác.

1. Mỗi dòng trẻ có **ô chọn**. Đầu danh sách có ô **"Chọn tất cả"**, chọn tất cả các trẻ **đang hiển thị** theo bộ lọc (ví dụ lọc "Chưa đón").
2. Khi đã chọn, thanh thao tác hiện **"Đã chọn n trẻ"** và nút **"Đón n trẻ đã chọn"** (hoặc **"Trả n trẻ đã chọn"** ở màn hình trả).
3. Bấm nút, hộp xác nhận liệt kê tên các trẻ. Xác nhận một lần thì hệ thống ghi đón (hoặc trả) cho tất cả trẻ đã chọn, cùng giờ và cùng người thực hiện.
4. Trẻ không hợp lệ được **bỏ qua và báo lại**, không làm hỏng cả nhóm. Ví dụ: trẻ đã đón rồi, trẻ đã báo nghỉ khi đón; trẻ vắng hoặc đã trả khi trả.
5. Khi trả trẻ, hộp xác nhận hiện người đón của từng trẻ, mặc định là người giám hộ chính đã đăng ký. Nếu trẻ do người khác đón, giáo viên đổi người đón ngay trong hộp xác nhận rồi mới xác nhận.

## 6. Dữ liệu đầu vào và đầu ra

| Loại | Nội dung | Nguồn hoặc nơi dùng |
| --- | --- | --- |
| Vào | Danh sách trẻ thuộc lớp trong ngày | Nghiệp vụ 02 |
| Vào | Giáo viên được phân công lớp trong ngày | Nghiệp vụ 05 |
| Vào | Báo nghỉ của phụ huynh | Phụ huynh, giáo viên |
| Vào | Danh sách người được đón trẻ | Nghiệp vụ 01 |
| Ra | Lượt đón: trẻ, ngày, giờ đón, người nhận trẻ | Nghiệp vụ 03 tự tạo kết quả có mặt |
| Ra | Lượt trả: trẻ, ngày, giờ trả, người đón, người trả | Văn phòng tra cứu, đối chiếu an toàn |
| Ra | Kết quả điểm danh theo ngày | Nghiệp vụ 03 chốt, sau đó nghiệp vụ 06 và 07 dùng số đã chốt |

## 7. Quy tắc nghiệp vụ

| Mã | Quy tắc cần xác nhận | Trường hợp cần thử |
| --- | --- | --- |
| QT-DT-01 | Đón trẻ là nguồn duy nhất tạo kết quả có mặt. Không có màn hình điểm danh có mặt riêng song song với đón trẻ. | Giáo viên đón trẻ xong, mở điểm danh thấy trẻ đã có mặt. |
| QT-DT-02 | Chỉ trẻ ở trạng thái đã đón mới được trả. Trẻ vắng hoặc báo nghỉ không có trong danh sách trả và không thể ghi trả. | Thử trả một trẻ vắng: trẻ không có trong danh sách trả nên không chọn được. |
| QT-DT-03 | Đón và trả trẻ chỉ thực hiện qua thao tác chọn trẻ rồi xác nhận, không có nút riêng trên từng dòng. Các trẻ đã chọn được ghi trong một lần xác nhận. Trẻ không hợp lệ được bỏ qua và liệt kê lại cho giáo viên. | Chọn 5 trẻ, trong đó 1 trẻ đã đón trước đó. Kết quả: đón 4 trẻ, báo 1 trẻ bị bỏ qua. |
| QT-DT-04 | Người đón phải thuộc danh sách người được đón trẻ. Trường hợp khác cần xác nhận của phụ huynh hoặc người có thẩm quyền và được ghi chú. | Ông bà chưa đăng ký đến đón trẻ. |
| QT-DT-05 | Chỉ giáo viên được phân công lớp trong ngày mới đón trả trẻ của lớp đó. | Giáo viên dạy thay đón trẻ lớp được giao dạy thay. |

## 8. Hạn chế của quy trình hiện tại

- Đón trẻ ghi một nơi, điểm danh ghi một nơi khác, nên trẻ đã đón vẫn có thể bị ghi vắng hoặc ngược lại.
- Danh sách trả trẻ in sẵn gồm cả trẻ vắng, dễ ghi nhầm là đã trả.
- Đón từng trẻ một rất chậm vào giờ cao điểm buổi sáng.

## 9. Yêu cầu đối với hệ thống mới

| Mã | Yêu cầu đề xuất |
| --- | --- |
| YC-DT-01 | Màn hình đón trẻ theo lớp và ngày, có bộ lọc theo trạng thái. Đón trẻ tự động tạo kết quả có mặt ở nghiệp vụ 03. |
| YC-DT-02 | Đón và trả trẻ bằng một thao tác duy nhất: chọn trẻ bằng ô chọn hoặc "Chọn tất cả", rồi bấm "Đón/Trả các trẻ đã chọn" và xác nhận một lần. Không có nút đón trả riêng trên từng dòng. Báo lại các trẻ bị bỏ qua. |
| YC-DT-03 | Danh sách trả trẻ chỉ gồm trẻ đã đón và chưa trả. Trẻ vắng hiển thị trong nhóm riêng, không thao tác được. |
| YC-DT-04 | Tự chuyển trẻ chưa đón sang vắng khi đến giờ chốt. Cảnh báo trẻ đã đón mà chưa trả khi kết thúc ngày. |
