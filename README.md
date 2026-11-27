# Kindergarten-Management-System

Hệ thống thông tin quản lý trường mầm non: hồ sơ trẻ, xếp lớp, điểm danh, đón trả trẻ, nhân sự, phân công giáo viên, học phí, suất ăn và báo cáo. Đề tài bài tập lớn HTTTQL, nhóm 02 người.

## Thành viên và phạm vi

| Thành viên | Nhánh khảo sát | Nghiệp vụ phụ trách |
| --- | --- | --- |
| TV1: *(MSSV – Họ tên)* | `survey/member-1` | 01 Hồ sơ trẻ, 02 Xếp lớp, 03 Điểm danh, 07 Suất ăn |
| TV2: *(MSSV – Họ tên)* | `survey/member-2` | 04 Nhân sự, 05 Phân công giáo viên, 06 Học phí, 08 Đón trả trẻ, 09 Báo cáo |

## Tài liệu

| Tài liệu | Vị trí |
| --- | --- |
| Tổng quan khảo sát, phân chia hệ thống con, BFD và ma trận phân quyền | [docs/survey/README.md](docs/survey/README.md) |
| Khảo sát nghiệp vụ 01 đến 09 | [docs/survey/](docs/survey/) |
| Hướng dẫn xây dựng HTTTQL và checklist chấm bài | `HuongdanXDHTTTQLv1_1 (1).pdf`, `Checklist_BTL_HTTTQL_v2026 (1).xlsx` |

## Quy ước làm việc

- Mỗi thành viên làm trên nhánh riêng, gộp vào `main` qua pull request có người còn lại xem xét.
- Mỗi tài liệu nghiệp vụ theo cùng một mẫu 9 mục: mục đích, đối tượng, quy trình hiện tại (4W+1H), dữ liệu vào, dữ liệu ra, biểu mẫu, quy tắc, hạn chế, yêu cầu.
- Mã quy tắc dùng dạng `QT-<nghiệp vụ>-<số>`, mã yêu cầu dùng `YC-<nghiệp vụ>-<số>`, mã chức năng BFD dùng `F1.x.y`.

Phần hướng dẫn cài đặt, cơ sở dữ liệu, dữ liệu mẫu và tài khoản demo sẽ được bổ sung khi bắt đầu giai đoạn lập trình.
