# Thông tin nhóm

- Tên nhóm: TeamDauLop

- Mã thành viên (ba chữ số): 101, 102, 103, 104, 105

- Người điều phối: 101

- Người review bản cuối: 101

- Commit chốt bài: Completed submission


## Kết quả tóm tắt

- Lượt gán hoàn tất / được giao: 120 / 120 lượt (40 ảnh × 3 người gán chéo)

- IAA trước calibration (kèm mẫu số): Mean IoU = 0.848 (518 cặp box ghép được); Class Agreement = 88.8% (460 / 518 cặp); truncated = 89.2%; occluded_level = 57.1%

- Số ảnh phải calibration: 35 / 40

- Vòng QC và số ảnh đã duyệt / cả lô: Round 01 — 40 / 40 ảnh

- Tỉ lệ lỗi: 0.0% theo ảnh (0/40), 0.0% theo đối tượng (0/370)

- **Quyết định** (Release / Hold / Rework) và lý do một dòng: **Release** — Hoàn tất 120/120 lượt gán, chốt guideline v1.1 sau calibration và đạt 0 lỗi khi chạy kiểm thử toàn diện (`check_submission.py`).


## Đánh giá mức độ hoàn thành & Đóng góp qua các Pha (Theo hướng dẫn của Labcoach Glenn-Quagmire)

Bảng tổng hợp mức độ tham gia thực tế của 5 thành viên qua 6 pha thực hành Lab Day 07:

| Mã thành viên | Pha 1 (Soi ảnh & Guideline cá nhân) | Pha 2 (Họp thống nhất v1.0 & Taxonomy) | Pha 3 (Gán nhãn độc lập 24 ảnh CVAT) | Pha 4 (Đo IAA & Soi bất đồng) | Pha 5 (Họp Calibration & Chốt v1.1) | Pha 6 (Duyệt QC & Đóng gói bàn giao) | Mức độ hoàn thành | Đánh giá & Kết luận |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **101** | ✔ Hoàn thành | ✔ Điều phối họp | ✔ 24 ảnh (128 box) | ✔ Chạy kiểm đo IAA | ✔ Điều phối chốt | ✔ Duyệt QC & Nộp bài | **100%** | **XUẤT SẮC** — Trưởng nhóm, điều phối xuyên suốt, quản lý kho Git và chốt gói nộp bài. |
| **104** | ✔ Hoàn thành | ✔ Tác giả Taxonomy v1.0 | ✔ 24 ảnh (241 box) | ✔ Soi 968 ca bất đồng | ✔ Soạn 11 quy tắc R-01..R-11 | ✔ Xây dựng Tool QA/QC & Audit | **100%** | **XUẤT SẮC** — Đóng góp chuyên môn vượt trội: soạn thảo Taxonomy 10 lớp, 11 quy tắc chuẩn hóa, sửa lỗi font/JSON và dựng công cụ đối chiếu trực quan. |
| **103** | ✔ Hoàn thành | ✔ Tham gia | ✔ 24 ảnh (161 box) | ✔ Tham gia đối chiếu | ✔ Có phối hợp cập nhật | ✔ Tham gia duyệt | **85%** | **ĐẠT** — Hoàn thành đầy đủ lượt gán, có nộp ghi chú cá nhân và tích cực phối hợp hiệu chuẩn nhãn sau giờ học. |
| **102** | ✔ Hoàn thành | ✔ Tham gia | ✔ 24 ảnh (153 box) | ⚠️ Ít tương tác | ❌ Vắng mặt họp calibration | ❌ Không tham gia | **60%** | **ĐẠT MỘT PHẦN** — Hoàn thành tốt phần việc trên lớp (Pha 1–3); sau khi rời lớp vắng mặt, không đóng góp vào Pha 5–6. |
| **105** | ✔ Hoàn thành trên lớp | ✔ Tham gia trên lớp | ✔ 24 ảnh (82 box) | ❌ Không tương tác | ❌ Vắng mặt hoàn toàn | ❌ Không commit Git | **30% (0% sau lớp)** | **KHÔNG ĐẠT (Không đủ điều kiện Review)** — Sau khi rời lớp mất liên lạc hoàn toàn; nhóm đã tạo nhóm, gọi điện, tag tên nhiều lần nhưng không phản hồi, không commit Git, làm ảnh hưởng tiến độ chung. Ghi nhận theo đúng hướng dẫn của Labcoach Glenn-Quagmire. |

> **Ghi chú của nhóm (101 & 104):**
> Biên bản này được lập trên tinh thần trung thực theo đúng quy định tại `RULES.md` và hướng dẫn trao đổi trực tiếp với Labcoach Glenn-Quagmire (phòng C201 ngày 17/11/2026). Điểm số cá nhân của các thành viên tích cực (101, 104, 103) được bảo toàn độc lập theo lịch sử commit Git và bằng chứng đóng góp thực tế.
