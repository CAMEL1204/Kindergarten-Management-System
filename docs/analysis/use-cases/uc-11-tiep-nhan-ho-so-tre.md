# UC-11 · Tiếp nhận hồ sơ trẻ

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV01 · F1.1.1, F1.1.2 |
| Tác nhân chính | Văn phòng |
| Tác nhân phụ | Ban giám hiệu (duyệt tiếp nhận) |
| Mục tiêu | Tiếp nhận đăng ký, phát hiện hồ sơ trùng hoặc trẻ quay lại, duyệt đợt tiếp nhận, theo dõi giấy tờ còn thiếu và bàn giao đúng hồ sơ cho xếp lớp. |
| Điều kiện trước | Văn phòng có quyền lập hồ sơ. Phụ huynh nộp phiếu đăng ký kèm bản sao giấy khai sinh. Danh mục nhóm tuổi và chỉ tiêu chỗ trống đã có tại [UC-02](uc-02-quan-ly-danh-muc-dung-chung.md). Ban giám hiệu có quyền duyệt tiếp nhận. |
| Điều kiện sau | Hồ sơ có kết quả xử lý và lịch sử. Hồ sơ đã duyệt và có ngày dự kiến bắt đầu học vào danh sách chờ xếp lớp của NV02 với cùng mã. Giấy tờ của mốc bắt đầu học còn thiếu được theo dõi theo lịch hẹn bổ sung. |

## Luồng sự kiện chính

1. Văn phòng ghi kênh và ngày đăng ký, đối chiếu phiếu đăng ký với bản sao giấy khai sinh.
2. Hệ thống tra theo mã trẻ, số định danh nếu có, họ tên, ngày sinh và người giám hộ để phát hiện nghi trùng hoặc trẻ quay lại (YC-HS-01). Sau khi đối chiếu xong, hệ thống xác lập mã trẻ và đợt tiếp nhận.
3. Văn phòng ghi người giám hộ chính và giấy tờ đã nộp theo từng mốc: mốc đăng ký, mốc duyệt tiếp nhận. Văn phòng kiểm tra thỏa thuận nhập học đã ký, rồi trình duyệt. Hồ sơ ở trạng thái `Chờ duyệt`.
4. Ban giám hiệu kiểm tra điều kiện tiếp nhận, độ tuổi và chỗ trống của nhóm tuổi, rồi ghi quyết định `Đã duyệt`.
5. Văn phòng thông báo kết quả, ghi ngày dự kiến bắt đầu học và lập lịch hẹn nộp giấy tờ của mốc bắt đầu học: sổ tiêm chủng, phiếu thông tin sức khỏe, danh sách người đưa đón. Hệ thống chuyển hồ sơ cùng mã sang danh sách chờ xếp lớp của NV02 (QT-HS-03).
6. Khi phụ huynh nộp bổ sung trước ngày bắt đầu học, Văn phòng cập nhật tình trạng từng loại giấy tờ và chốt hoàn tất điều kiện nhập học (YC-HS-02).

## Luồng thay thế và ngoại lệ

- **Bước 2 (trẻ quay lại):** Đối chiếu xác nhận là trẻ quay lại. Hệ thống mở đợt tiếp nhận mới trên cùng mã trẻ và giữ lịch sử cũ (QT-HS-02).
- **Bước 2 (trùng tên và ngày sinh nhưng là trẻ khác):** Giấy khai sinh, số định danh hoặc người giám hộ xác nhận là hai trẻ khác nhau. Hệ thống cho tạo mã trẻ mới và lưu kết quả đối chiếu (QT-HS-02).
- **Bước 3 (thiếu giấy tờ của mốc duyệt):** Thiếu phiếu đăng ký, bản sao giấy khai sinh hoặc thỏa thuận nhập học đã ký. Hệ thống chặn trình duyệt, cho lưu lịch hẹn bổ sung và đưa hồ sơ vào danh sách chờ bổ sung (QT-HS-01, YC-HS-02).
- **Bước 4 (hết chỗ hoặc từ chối):** Nhóm tuổi hết chỗ, trẻ không đạt độ tuổi hoặc Ban giám hiệu không chấp thuận. Ban giám hiệu chọn `Từ chối` kèm lý do. Hồ sơ không vào danh sách chờ của NV02.
- **Bước 6 (chưa đủ giấy tờ của mốc bắt đầu học):** Trẻ đã duyệt và có ngày dự kiến học vẫn được bàn giao sang NV02 để xếp lớp dự kiến (QT-HS-03). Hồ sơ vẫn nằm trong bộ lọc còn thiếu giấy tờ để Văn phòng nhắc bổ sung.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Kênh và ngày đăng ký. Mã trẻ hoặc số định danh nếu có, họ tên, ngày sinh. Người giám hộ chính. Giấy tờ từng mốc, thỏa thuận nhập học. Ngày dự kiến bắt đầu học. Quyết định và lý do duyệt. | Mã trẻ, mã đợt tiếp nhận. Trạng thái `Chờ duyệt`, `Đã duyệt` hoặc `Từ chối`, người và ngày duyệt. Tình trạng giấy tờ theo mốc, lịch hẹn bổ sung. Danh sách chờ xếp lớp cho NV02. Số liệu tuyển sinh và hồ sơ chờ bổ sung cho NV09. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu ([Mẫu 01](../../survey/forms/mau-01-ho-so-tre.md)) | Kết quả mong đợi |
| --- | --- | --- |
| Tiếp nhận đủ điều kiện | HS2026-042 Nguyễn Minh Khôi, đăng ký 28/08/2026, dự kiến học 07/09/2026, người giám hộ chính Trần Thị Mai | Lê Minh Tâm duyệt ngày 04/09/2026. Hồ sơ vào danh sách chờ xếp lớp của NV02 với mã HS2026-042. |
| Trùng tên và ngày sinh | Hai hồ sơ cùng họ tên và ngày sinh, giấy khai sinh khác nhau | Hệ thống cảnh báo nghi trùng. Văn phòng xác nhận hai trẻ khác nhau, hệ thống cấp mã mới và lưu kết quả đối chiếu. |
| Trẻ quay lại | Trẻ đã có hồ sơ cũ quay lại với số liên hệ đã đổi | Dùng lại mã trẻ cũ, mở đợt tiếp nhận mới, giữ lịch sử (QT-HS-02). |
| Thiếu thỏa thuận nhập học | Hồ sơ có phiếu đăng ký và giấy khai sinh, chưa có thỏa thuận đã ký | Chặn trình duyệt. Hồ sơ vào danh sách chờ bổ sung kèm lịch hẹn. |
| Đã duyệt nhưng còn thiếu giấy tờ | Hồ sơ đã duyệt, chưa nộp sổ tiêm chủng | Vẫn bàn giao NV02. Hồ sơ nằm trong bộ lọc còn thiếu giấy tờ (QT-HS-01, QT-HS-03). |
| Hết chỗ | Nhóm tuổi của trẻ đã đủ chỉ tiêu | Ban giám hiệu từ chối kèm lý do. Hồ sơ không vào danh sách chờ xếp lớp. |

## Liên phân hệ và truy vết

- **NV01 → NV02:** Bàn giao hồ sơ `Đã duyệt` có ngày dự kiến bắt đầu học, giữ nguyên mã trẻ. Kết quả duyệt được tách khỏi tình trạng giấy tờ của mốc đi học (QT-HS-03). Xếp lớp tiếp tục tại [UC-17](uc-17-xep-lop-va-bien-dong-si-so.md).
- **NV01 → NV09:** Số hồ sơ tiếp nhận mới, kết quả duyệt và hồ sơ chờ bổ sung theo kỳ cho báo cáo tuyển sinh của Văn phòng (BC-02) tại [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md).
- Sửa nhân thân, quản lý ủy quyền nhận trẻ (NV01 → NV08) và lưu ý chăm sóc (NV01 → NV07) thực hiện tại [UC-16](uc-16-cap-nhat-ho-so-va-luu-y-cham-soc.md).

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-HS-01, YC-HS-02 | QT-HS-01, QT-HS-02 | NV01 · Đối chiếu trẻ trùng hoặc quay lại, theo dõi giấy tờ theo mốc và lịch hẹn bổ sung. |
| YC-HS-03 | QT-HS-03 | NV01 → NV02, NV09 · Bàn giao hồ sơ đủ điều kiện cùng mã, cung cấp số liệu tuyển sinh. |
