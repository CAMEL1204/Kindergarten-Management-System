# Khảo sát nghiệp vụ và phân chia hệ thống con

> Bản khảo sát sơ bộ. Cách phân chia hệ thống con dưới đây là phương án đề xuất, chờ cơ sở khảo sát xác nhận vai trò thực tế của từng bộ phận.

## 1. Danh sách nghiệp vụ khảo sát

| STT | Nghiệp vụ | Tài liệu |
| --- | --- | --- |
| 01 | Tiếp nhận và quản lý hồ sơ trẻ | `docs/khao-sat/01-...` (nhánh `survey/member-1`) |
| 02 | Xếp lớp và quản lý biến động sĩ số | `docs/khao-sat/02-...` (nhánh `survey/member-1`) |
| 03 | Điểm danh và theo dõi chuyên cần | `docs/khao-sat/03-...` (nhánh `survey/member-1`) |
| 04 | Quản lý hồ sơ giáo viên và nhân viên | [04-staff-management.md](04-staff-management.md) |
| 05 | Phân công giáo viên theo lớp và thời gian | [05-teacher-assignment.md](05-teacher-assignment.md) |
| 06 | Lập khoản phải thu và thu học phí | [06-tuition-management.md](06-tuition-management.md) |
| 07 | Quản lý suất ăn và chế độ ăn của trẻ | `docs/khao-sat/07-...` (nhánh `survey/member-1`) |
| 08 | Đón trẻ, trả trẻ và tự động ghi nhận có mặt | [08-child-drop-off-and-pick-up.md](08-child-drop-off-and-pick-up.md) |
| 09 | Lập, ghi chú và gửi báo cáo | [09-reporting.md](09-reporting.md) |

## 2. Nguyên tắc chia hệ thống con

Mỗi hệ thống con phục vụ **một bộ phận** và chỉ chứa những chức năng mà bộ phận đó thực sự làm trong công việc hằng ngày. Một chức năng không thuộc nhiệm vụ của bộ phận thì **không xuất hiện** trong hệ thống con của bộ phận đó, kể cả dưới dạng chỉ xem, trừ khi bộ phận cần dữ liệu đó làm căn cứ.

- **Làm chủ dữ liệu:** mỗi loại dữ liệu chỉ có một hệ thống con được tạo và sửa. Ví dụ, thực đơn chỉ do hệ thống con Bếp tạo, điểm danh chỉ do hệ thống con Giáo viên tạo.
- **Nhận căn cứ, không nhận chức năng:** khi cần dữ liệu của bộ phận khác, hệ thống con chỉ nhận **số liệu đã chốt** ở dạng chỉ đọc, không nhận màn hình nghiệp vụ của bộ phận đó. Ví dụ, Kế toán cần số ngày có mặt để tính tiền ăn, nên chỉ nhận con số ngày có mặt đã chốt. Kế toán không có màn hình điểm danh và không có màn hình thực đơn.
- **Báo cáo đi lên, không có sẵn ở trên:** mỗi hệ thống con tự lập báo cáo trong phạm vi của mình, ghi chú và gửi cho Ban giám hiệu. Ban giám hiệu không tự sinh báo cáo của bộ phận khác. Quy trình chi tiết ở nghiệp vụ 09.

## 3. Các hệ thống con và chức năng

### 3.1. Hệ thống con Ban giám hiệu (quản trị)

| Có | Không có |
| --- | --- |
| Quản lý tài khoản và phân quyền người dùng. Danh mục dùng chung: năm học, nhóm tuổi, lớp, ca làm việc. | Nhập điểm danh, đón trả trẻ. |
| Duyệt: tiếp nhận hồ sơ trẻ, phương án xếp lớp, phân công giáo viên, biểu phí và miễn giảm, thực đơn. | Lập bảng phải thu, ghi thu tiền, in biên lai. |
| Hộp báo cáo nhận được từ các bộ phận: xem, phản hồi, yêu cầu bổ sung, duyệt (nghiệp vụ 09). | Lập thực đơn, chốt suất ăn. |
| Bảng tổng hợp toàn trường lấy từ các báo cáo **đã duyệt**. | Tự sinh báo cáo thay cho bộ phận. |

### 3.2. Hệ thống con Văn phòng

| Có | Không có |
| --- | --- |
| Tiếp nhận và quản lý hồ sơ trẻ, danh sách người được đón trẻ (nghiệp vụ 01). | Thu tiền, công nợ. |
| Xếp lớp, chuyển lớp, thôi học (nghiệp vụ 02). | Thực đơn, suất ăn. |
| Hồ sơ giáo viên và nhân viên (nghiệp vụ 04). Phân công giáo viên (nghiệp vụ 05), nếu trường giao cho văn phòng. | Điểm danh từng trẻ. |
| Lập, ghi chú và gửi báo cáo tuyển sinh, sĩ số, nhân sự. | |

### 3.3. Hệ thống con Giáo viên

Chỉ thấy **lớp mình được phân công trong ngày** (nghiệp vụ 05).

| Có | Không có |
| --- | --- |
| Đón trẻ, trả trẻ, chọn nhiều trẻ để đón hoặc trả cùng lúc (nghiệp vụ 08). | Học phí, công nợ, biên lai. |
| Điểm danh được tự động ghi nhận từ đón trẻ. Xác minh vắng, chốt điểm danh (nghiệp vụ 03). | Lập thực đơn. Chỉ xem thực đơn của lớp mình và lưu ý dị ứng của trẻ. |
| Xác nhận danh sách ăn và số suất của lớp (nghiệp vụ 07). | Hồ sơ nhân sự, xếp lớp. |
| Lập, ghi chú và gửi báo cáo lớp: chuyên cần, tình hình trẻ. | |

### 3.4. Hệ thống con Kế toán

| Có | Không có |
| --- | --- |
| Biểu phí, danh mục miễn giảm (trình duyệt). | **Thực đơn, chế độ ăn, chốt suất ăn.** |
| Lập bảng phải thu theo kỳ, thông báo học phí (nghiệp vụ 06). | Điểm danh, đón trả trẻ. |
| Ghi thu tiền, biên lai, điều chỉnh, hoàn tiền, công nợ. | Hồ sơ trẻ, xếp lớp, phân công giáo viên. |
| Nhận **chỉ đọc**: danh sách trẻ theo lớp trong kỳ, số ngày có mặt đã chốt, số suất ăn đã chốt. | |
| Lập, ghi chú và gửi báo cáo thu, công nợ. | |

### 3.5. Hệ thống con Bếp và dinh dưỡng

| Có | Không có |
| --- | --- |
| Lập thực đơn theo ngày, bữa, trình duyệt (nghiệp vụ 07). | Tiền ăn, học phí, công nợ. |
| Tổng hợp số suất từ các lớp, giao nhận suất ăn. | Điểm danh, đón trả trẻ. |
| Xem danh sách trẻ dị ứng hoặc chế độ ăn riêng. | Hồ sơ nhân sự. |
| Lập, ghi chú và gửi báo cáo suất ăn. | |

## 4. Ma trận nghiệp vụ và hệ thống con

Ký hiệu: **T** tạo và sửa, **D** duyệt, **X** chỉ xem hoặc nhận số liệu đã chốt, **—** không có.

| Nghiệp vụ | Ban giám hiệu | Văn phòng | Giáo viên | Kế toán | Bếp |
| --- | --- | --- | --- | --- | --- |
| 01 Hồ sơ trẻ | D | T | X (lớp mình) | — | X (dị ứng) |
| 02 Xếp lớp | D | T | X (lớp mình) | X | — |
| 03 Điểm danh | X | X | T (lớp mình) | X (số ngày đã chốt) | — |
| 04 Hồ sơ nhân sự | D | T | — | — | — |
| 05 Phân công giáo viên | D | T | X (lịch mình) | — | — |
| 06 Học phí | D (biểu phí, miễn giảm) | — | — | T | — |
| 07 Suất ăn, thực đơn | D (thực đơn) | — | T (xác nhận suất lớp) | X (số suất đã chốt) | T |
| 08 Đón trả trẻ | — | X | T (lớp mình) | — | — |
| 09 Báo cáo | Nhận, D | T, gửi | T, gửi | T, gửi | T, gửi |

**Câu hỏi xác minh khi phỏng vấn ban giám hiệu**

- Trường có những bộ phận nào? Một người có kiêm nhiều vai trò không (ví dụ văn phòng kiêm kế toán)? Nếu có, người đó được cấp nhiều hệ thống con chứ không gộp chức năng vào một hệ thống con.
- Ai phân công giáo viên: văn phòng hay phó hiệu trưởng chuyên môn?
- Kế toán có cần xem chi tiết điểm danh từng ngày không, hay chỉ cần tổng số ngày có mặt đã chốt?
