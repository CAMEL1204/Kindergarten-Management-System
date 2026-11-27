# Truy vết nghiệp vụ – yêu cầu – quy tắc – Use Case

## Phần TV1 bàn giao

Mỗi dòng nêu phần yêu cầu được thực hiện trong giao dịch tương ứng. Mã quy tắc khác nghiệp vụ chỉ điều kiện trao đổi với phân hệ liên quan.

| NV | Yêu cầu | Quy tắc | Use Case | Phạm vi và luồng liên phân hệ |
| --- | --- | --- | --- | --- |
| NV01 | YC-HS-01, YC-HS-02 | QT-HS-01, QT-HS-02 | [UC-11 · Tiếp nhận hồ sơ trẻ](use-cases/uc-11-tiep-nhan-ho-so-tre.md) | Đối chiếu trẻ, giấy tờ và từng mốc tiếp nhận. |
| NV01 | YC-HS-03 | QT-HS-03 | [UC-11 · Tiếp nhận hồ sơ trẻ](use-cases/uc-11-tiep-nhan-ho-so-tre.md) | NV01 → NV02 · Bàn giao hồ sơ đủ điều kiện. |
| NV01 | YC-HS-04, YC-HS-05 | QT-HS-04, QT-HS-05, QT-DT-04 | [UC-11 · Tiếp nhận hồ sơ trẻ](use-cases/uc-11-tiep-nhan-ho-so-tre.md) | NV01 → NV08 · Tra quyền nhận trẻ theo hiệu lực. |
| NV02 | YC-XL-01, YC-XL-02 | QT-XL-01, QT-XL-02, QT-XL-03, QT-XL-04, QT-PC-01 | [UC-12 · Chuyển lớp](use-cases/uc-12-chuyen-lop.md) | NV02 → NV05 · Kiểm tra lớp hoạt động, ghi hai đầu chuyển lớp. |
| NV02 | YC-XL-03 | QT-XL-05, QT-HP-02 | [UC-12 · Chuyển lớp](use-cases/uc-12-chuyen-lop.md) | NV02 → NV03, NV06 · Diện đi học theo ngày và đối chiếu hồi tố. |
| NV03 | YC-DD-01, YC-DD-05 | QT-DD-01, QT-DD-06 | [UC-13 · Chốt chuyên cần theo lớp và ngày](use-cases/uc-13-chot-chuyen-can.md) | NV02, NV05 → NV03 · Lớp, quyền, nghỉ dài ngày. |
| NV03 | YC-DD-02 | QT-DD-02, QT-DD-05, QT-DT-01, QT-DT-05, QT-DT-07 | [UC-13 · Chốt chuyên cần theo lớp và ngày](use-cases/uc-13-chot-chuyen-can.md) | NV08 → NV03 → NV07 · Giao nhận, xác minh và bàn giao theo mốc. |
| NV03 | YC-DD-04, YC-DD-05 | QT-DD-04, QT-DD-07, QT-HP-05 | [UC-13 · Chốt chuyên cần theo lớp và ngày](use-cases/uc-13-chot-chuyen-can.md) | NV03 → NV06, NV09 · Khóa kỳ và tổng hợp. |
| NV03 | YC-DD-03, YC-DD-05 | QT-DD-03, QT-DD-05, QT-DD-07, QT-DT-07, QT-HP-05 | [UC-14 · Điều chỉnh chuyên cần sau khóa](use-cases/uc-14-dieu-chinh-chuyen-can-sau-khoa.md) | NV08 → NV03 → NV06 · Duyệt thay đổi và treo đối soát theo trẻ. |
| NV07 | YC-SA-01, YC-SA-02, YC-SA-03 | QT-SA-01, QT-SA-02, QT-SA-03, QT-HS-06 | [UC-15 · Chốt và đối chiếu suất ăn theo bữa](use-cases/uc-15-chot-va-doi-chieu-suat-an.md) | NV01, NV03 → NV07 · Chọn trẻ ăn theo mốc và lưu ý dị ứng. |
| NV07 | YC-SA-04, YC-SA-05 | QT-SA-04, QT-SA-05, QT-SA-06 | [UC-15 · Chốt và đối chiếu suất ăn theo bữa](use-cases/uc-15-chot-va-doi-chieu-suat-an.md) | NV08 → NV07 → NV09 · Giờ về, điều chỉnh sau chốt và giao nhận. |
