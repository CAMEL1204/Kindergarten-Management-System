# UC-11 · Tiếp nhận hồ sơ trẻ

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV01 · F1.1.1, F1.1.2 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu |
| Mục tiêu | Tiếp nhận đăng ký, kiểm tra hồ sơ trùng/trẻ quay lại, duyệt đợt tiếp nhận, theo dõi hoàn thiện giấy tờ và bàn giao đúng hồ sơ cho xếp lớp. |
| Điều kiện trước | Văn phòng có quyền lập hồ sơ và tiếp nhận phiếu đăng ký kèm bản sao giấy khai sinh. Danh mục nhóm tuổi và chỉ tiêu chỗ trống của trường đã được thiết lập. Ban giám hiệu có quyền duyệt tiếp nhận. |
| Điều kiện sau | Hồ sơ được lưu vết kết quả xử lý. Nếu được duyệt và có ngày dự kiến bắt đầu học, hồ sơ xuất hiện trong danh sách chờ xếp lớp của NV02 với cùng mã hồ sơ. Giấy tờ mốc bắt đầu học còn thiếu được theo dõi hẹn bổ sung riêng. |

## Luồng sự kiện chính

1. Văn phòng ghi nhận kênh, ngày đăng ký và đối chiếu phiếu đăng ký với bản sao giấy khai sinh.
2. Hệ thống hỗ trợ tra cứu theo mã trẻ, số định danh (nếu có), họ tên, ngày sinh và thông tin người giám hộ để phát hiện nghi trùng hoặc trẻ quay lại; xác lập mã trẻ và đợt tiếp nhận sau khi xử lý đối chiếu.
3. Văn phòng ghi thông tin người giám hộ chính, danh mục giấy tờ đã nộp theo từng mốc (mốc đăng ký, mốc duyệt tiếp nhận), kiểm tra thỏa thuận nhập học đã ký rồi trình duyệt (trạng thái `Chờ duyệt`).
4. Ban giám hiệu kiểm tra điều kiện tiếp nhận, độ tuổi và chỗ trống còn lại của nhóm tuổi, rồi ghi quyết định duyệt (`Đã duyệt`).
5. Văn phòng thông báo kết quả, ghi ngày dự kiến bắt đầu học, lập lịch hẹn nộp giấy tờ của mốc bắt đầu học (sổ tiêm chủng, phiếu thông tin sức khỏe, danh sách người đưa đón) và hệ thống chuyển hồ sơ cùng mã sang danh sách chờ xếp lớp của NV02.
6. Khi phụ huynh nộp bổ sung giấy tờ trước ngày bắt đầu học, văn phòng cập nhật tình trạng hoàn thiện theo từng loại giấy tờ và chốt hoàn tất điều kiện nhập học.

## Luồng thay thế và ngoại lệ

- **Bước 2 (trẻ quay lại):** Nếu đối chiếu xác nhận là trẻ quay lại, mở đợt tiếp nhận mới trên **cùng mã trẻ** và giữ lịch sử cũ (`QT-HS-02`).
- **Bước 2 (nghi trùng trẻ khác):** Nếu trùng họ tên và ngày sinh nhưng đối chiếu giấy khai sinh, số định danh hoặc thông tin giám hộ xác nhận là hai trẻ khác nhau, hệ thống cho phép tạo mã trẻ mới và ghi chú kết quả đối chiếu (`QT-HS-02`).
- **Bước 3 (thiếu giấy tờ mốc duyệt):** Nếu thiếu phiếu đăng ký, bản sao giấy khai sinh hoặc thỏa thuận nhập học đã ký, hệ thống chặn trình duyệt và cho phép lưu lịch hẹn bổ sung để theo dõi ở danh sách hồ sơ chờ bổ sung (`QT-HS-01`, `YC-HS-02`).
- **Bước 4 (hết chỗ hoặc từ chối):** Nếu nhóm tuổi đã hết chỗ, trẻ không đạt điều kiện độ tuổi hoặc Ban giám hiệu không chấp thuận, Ban giám hiệu chọn `Từ chối` kèm lý do, lưu quyết định và không chuyển sang danh sách chờ NV02.
- **Bước 6 (chưa hoàn thiện giấy tờ mốc đi học):** Trẻ đã duyệt và có ngày dự kiến học vẫn được bàn giao sang NV02 để xếp lớp dự kiến (`QT-HS-03`), đồng thời tiếp tục xuất hiện trong bộ lọc hồ sơ còn thiếu giấy tờ để văn phòng nhắc bổ sung trước ngày học thực tế.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Kênh và ngày đăng ký, mã trẻ/số định danh (nếu có), họ tên, ngày sinh, thông tin người giám hộ chính, danh mục giấy tờ từng mốc, thỏa thuận nhập học, ngày dự kiến bắt đầu học, quyết định và lý do duyệt. | Mã trẻ, mã đợt tiếp nhận, trạng thái duyệt (`Chờ duyệt` / `Đã duyệt` / `Từ chối`), người và ngày duyệt, tình trạng hoàn thiện giấy tờ theo mốc, lịch hẹn bổ sung, danh sách chờ xếp lớp NV02, số liệu tuyển sinh và hồ sơ chờ bổ sung cho NV09. |

## Liên phân hệ và truy vết

- **NV01 → NV02:** Bàn giao trực tiếp danh sách hồ sơ ở trạng thái `Đã duyệt` và có ngày dự kiến bắt đầu học, giữ nguyên mã trẻ/mã hồ sơ và tách biệt kết quả duyệt với tình trạng hoàn thiện giấy tờ mốc đi học (`QT-HS-03`).
- **NV01 → NV09:** Cung cấp số liệu hồ sơ tiếp nhận mới, kết quả duyệt và danh sách hồ sơ chờ bổ sung theo kỳ để Văn phòng lập báo cáo tuyển sinh (`BC-02`).
- Việc sửa đổi thông tin nhân thân, quản lý người được ủy quyền cố định nhận trẻ (`NV01 → NV08`) và đề nghị/xác nhận lưu ý chăm sóc (`NV01 → NV07`) được thực hiện tại [UC-16](uc-16-cap-nhat-ho-so-va-luu-y-cham-soc.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HS-01, YC-HS-02 | QT-HS-01, QT-HS-02 | Đối chiếu trẻ trùng/quay lại, theo dõi giấy tờ theo mốc và lịch hẹn bổ sung. |
| YC-HS-03 | QT-HS-03 | NV01 → NV02 · Bàn giao hồ sơ đủ điều kiện cùng mã định danh; cung cấp số liệu cho NV09. |
