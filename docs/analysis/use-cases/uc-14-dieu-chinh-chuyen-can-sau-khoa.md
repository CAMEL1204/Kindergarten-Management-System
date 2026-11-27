# UC-14 · Điều chỉnh chuyên cần sau khóa

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | NV03 · F1.3 |
| Tác nhân chính | Giáo viên |
| Tác nhân phụ | Ban giám hiệu hoặc người được phân quyền duyệt |
| Mục tiêu | Xử lý chứng cứ đến sau giờ khóa và thông báo thay đổi cho bên đã dùng kết quả. |
| Điều kiện trước | Dòng chuyên cần đã khóa. Có lượt đón phát sinh, hủy lượt đón, căn cứ xác minh mới hoặc ngày phải đối chiếu do xếp lớp hồi tố. |
| Điều kiện sau | Đề nghị được duyệt hoặc từ chối có lưu vết. Chỉ kết quả đã duyệt được dùng khi tính lại số ngày có mặt. |

## Luồng sự kiện chính

1. Giáo viên chọn dòng đã khóa và đối chiếu căn cứ. Với lượt đón hoặc hủy lượt đón từ giờ khóa, hệ thống tạo đề nghị gắn với sự kiện NV08.
2. Lưu kết quả trước và sau đề nghị, lý do, người đề nghị và thời điểm. Giữ kết quả đang có hiệu lực trong thời gian chờ.
3. Người duyệt đối chiếu lớp, ngày, chứng cứ và kết quả hiện hành. Chỉ duyệt Có mặt khi còn lượt đón hợp lệ.
4. Ghi quyết định, thời điểm, người duyệt. Nếu duyệt, cập nhật kết quả và tổng hợp kỳ, thông báo cho các bộ phận đã sử dụng số liệu.

## Luồng thay thế và ngoại lệ

- Bước 3, nếu chứng cứ không phù hợp, từ chối và lưu lý do cùng kết quả cũ.
- Bước 3, nếu kết quả gốc đã đổi do đề nghị khác, đối chiếu lại trước khi duyệt.
- Bước 2, nhắc xử lý trước ngày học đầu kỳ kế tiếp. Chỉ treo đối soát trẻ có đề nghị đang chờ có thể đổi số ngày có mặt.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Mã trẻ, lớp, ngày, mã lượt đón hoặc hủy, căn cứ xác minh, trạng thái cũ và đề xuất, lý do. | Trạng thái đề nghị, lịch sử trước/sau, người và thời điểm đề nghị/duyệt, số ngày có mặt cập nhật, cảnh báo đối soát. |

## Liên phân hệ và truy vết

NV08 tiếp tục ghi giao nhận thực tế. NV07 xử lý thay đổi suất theo tiến độ bếp. NV06 tính 0 ngày cho lượt Chờ xác minh chưa có lượt đón và vẫn đối soát những trẻ đủ căn cứ.

| Yêu cầu | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-DD-03, YC-DD-05 | QT-DD-03, QT-DD-05, QT-DD-07, QT-DT-07, QT-HP-05 | NV08 → NV03 → NV06 · Duyệt thay đổi và treo đối soát theo trẻ. |
