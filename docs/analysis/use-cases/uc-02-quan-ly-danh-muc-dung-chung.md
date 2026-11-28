# UC-02 · Quản lý danh mục dùng chung

| Thuộc tính | Đặc tả |
| --- | --- |
| Nghiệp vụ / nhánh chức năng | F1.0 · F1.0.2. NV06 · F1.6.1 (biểu phí và miễn giảm) |
| Tác nhân chính | Ban giám hiệu (lập danh mục của trường, duyệt mọi danh mục). Văn phòng (lập danh mục lớp NV02). Kế toán (lập biểu phí và danh mục miễn giảm NV06, chức năng F1.6.1) |
| Tác nhân phụ | Không có |
| Mục tiêu | Lập, duyệt và duy trì các danh mục dùng chung của trường. Không có danh mục trùng lặp. Không sửa trực tiếp dữ liệu đã phát sinh giao dịch. |
| Điều kiện trước | Người dùng có tài khoản thuộc hệ thống con tương ứng (Ban giám hiệu, Văn phòng, Kế toán). Quy định nền của trường đã có: Điều 15 Điều lệ Trường mầm non, kế hoạch năm học. |
| Điều kiện sau | Danh mục ở trạng thái `Đã duyệt` hoặc `Đang áp dụng` được cung cấp chỉ đọc cho các phân hệ NV01–NV09 (NFR-08). |

## Luồng sự kiện chính

1. Ban giám hiệu lập và ban hành các danh mục thuộc quyền của mình: năm học, kỳ học, lịch nghỉ của trường, nhóm tuổi kèm sĩ số tối đa theo Điều 15, ca làm việc và khung giờ, định mức giáo viên theo lớp hoặc nhóm tuổi, danh mục loại báo cáo gồm bộ phận lập, kỳ, hạn gửi, chỉ tiêu (YC-BC-05).
2. Văn phòng lập hoặc cập nhật danh mục lớp: mã lớp, tên lớp, năm học, nhóm tuổi, mốc tính tuổi, chỉ tiêu vận hành, tình trạng hoạt động. Văn phòng trình Ban giám hiệu duyệt (YC-XL-01, QT-XL-01).
3. Kế toán lập biểu phí mới (khoản thu, đơn giá, đơn vị tính, nhóm tuổi hoặc lớp áp dụng, ngày hiệu lực) và danh mục miễn giảm, rồi trình Ban giám hiệu duyệt (YC-HP-01, QT-HP-01). Bước này là chức năng F1.6.1 của NV06. Kế toán là chủ dữ liệu biểu phí và miễn giảm.
4. Ban giám hiệu kiểm tra, ghi quyết định duyệt và phát hành danh mục cho các phân hệ liên quan. Với biểu phí và miễn giảm, quyết định duyệt hoàn tất F1.6.1 và là căn cứ cho [UC-06](uc-06-lap-bang-phai-thu-theo-ky.md) (QT-HP-01, QT-HP-03).

## Luồng thay thế và ngoại lệ

- **Bước 2 (chỉ tiêu vượt trần Điều 15):** Hệ thống chặn lưu và yêu cầu điều chỉnh (QT-XL-01).
- **Bước 3 (sửa biểu phí đã dùng):** Biểu phí đã dùng để lập bảng phải thu không được sửa. Kế toán ban hành biểu phí mới với ngày hiệu lực mới (QT-HP-01).
- **Bước 1–4 (ngừng dùng danh mục đã có dữ liệu):** Không xóa lớp, năm học, nhóm tuổi hay loại báo cáo đã có dữ liệu lịch sử (NFR-04). Chỉ chuyển trạng thái `Ngừng hoạt động` hoặc `Hết hiệu lực`.
- **Bước 1 (đổi lịch nghỉ):** Nếu thay đổi lịch nghỉ ảnh hưởng ngày đã khóa chuyên cần hoặc ngày đã tạm thu, hệ thống cảnh báo để đối chiếu.

## Dữ liệu giao dịch

| Dữ liệu vào | Dữ liệu ra |
| --- | --- |
| Năm học, kỳ học, lịch nghỉ. Nhóm tuổi kèm trần Điều 15. Ca làm việc, định mức giáo viên (NV05). Danh mục lớp (NV02). Biểu phí và danh mục miễn giảm (NV06). Danh mục loại báo cáo (NV09). Quyết định duyệt. | Danh mục dùng chung ở trạng thái `Hoạt động` hoặc `Ngừng hoạt động`. Lịch sử phiên bản biểu phí và thông tin duyệt. |

## Ca kiểm thử từ kịch bản

| Ca | Dữ liệu | Kết quả mong đợi |
| --- | --- | --- |
| Danh mục lớp năm học 2026–2027 | Nhà trẻ 1 (20), Mầm 1 (25), Mầm 2 (25), Chồi 1 (30), Lá 1 (35) (README khảo sát §1) | Mọi chỉ tiêu nằm trong trần Điều 15. Ban giám hiệu duyệt, 5 lớp ở trạng thái `Hoạt động`. |
| Chỉ tiêu vượt trần | Văn phòng nhập chỉ tiêu Chồi 1 (4–5 tuổi) là 31 | Chặn lưu, vì trần của nhóm 4–5 tuổi là 30 (QT-XL-01). |
| Tăng tiền ăn giữa năm học | Biểu phí tiền ăn 35.000 đồng đã dùng cho PT202610-042 ([Mẫu 04](../../survey/forms/mau-04-hoc-phi-va-bien-lai.md)) | Không sửa được dòng cũ. Kế toán ban hành biểu phí mới có ngày hiệu lực mới, bảng phải thu cũ giữ đơn giá cũ (QT-HP-01). |
| Ngừng lớp có dữ liệu | Một lớp đã có lịch sử xếp lớp và điểm danh | Không xóa được. Lớp chuyển sang `Ngừng hoạt động` (NFR-04). |
| Loại báo cáo có hạn gửi | Báo cáo sĩ số tháng của Văn phòng, hạn ngày 05 tháng sau | [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md) đọc được hạn gửi và đánh dấu trễ hạn khi gửi sau ngày 05 (QT-BC-06). |

## Liên phân hệ và truy vết

- **F1.0 → NV02, NV03, NV05, NV06:** Năm học, kỳ học, lịch học và lịch nghỉ, nhóm tuổi, lớp, ca làm việc và định mức giáo viên (QT-XL-01, QT-PC-03, QT-DD-01, QT-HP-02).
- **F1.0 → NV06:** Biểu phí và danh mục miễn giảm đã duyệt là căn cứ lập bảng phải thu (YC-HP-01, QT-HP-01, QT-HP-03).
- **F1.0 → NV09:** Danh mục loại báo cáo, kỳ và hạn gửi cho [UC-10](uc-10-gui-phan-hoi-va-duyet-bao-cao.md) (YC-BC-05, QT-BC-06).

| Yêu cầu / NFR | Quy tắc áp dụng | Phạm vi |
| --- | --- | --- |
| YC-XL-01, YC-HP-01, YC-BC-05, NFR-04, NFR-08 | QT-XL-01, QT-HP-01, QT-HP-02, QT-HP-03, QT-PC-03, QT-DD-01, QT-BC-06 | F1.0 → NV02, NV03, NV05, NV06, NV09 · Lập, duyệt và giữ lịch sử các danh mục dùng chung của trường. |
