# Báo cáo Nghiệm thu Chất lượng Dữ liệu (QC Report) — Nhóm TeamDauLop

- Mục đích và giới hạn sử dụng: Bộ dữ liệu 40 ảnh giao thông đô thị (372 annotations, 10 lớp) phục vụ huấn luyện và đánh giá mô hình Phát hiện Vật thể 2D (ADAS / giám sát giao thông); không dùng cho nhận diện danh tính cá nhân.
- Manifest và checksum nhãn cuối: `submission/qc/labels_calibrated.json` (SHA-256: `279cb687f7ef7cb2bd3a0e2c73bc718542c5e6a09d5affcb2941114b654659c1`), đối chiếu `submission/qc/batch_manifest.json` (SHA-256: `558de76d3dc6b6e01a24c32d01a4398754652db6cf4cdc485ba092b0279c8fb7`).
- Guideline và taxonomy khi gán / khi bàn giao: Gán chéo theo `taxonomy.json` v1.0 (10 lớp) và `guideline_v1.0.md`; bàn giao theo `guideline_v1.1_calibrated.md` (bổ sung sau Calibration, `kind: editorial`).
- Lượt hoàn thành / được giao: 120/120 lượt gán chéo hoàn thành (5 thành viên `101`–`105`, mỗi thành viên 24/40 ảnh; đạt 100% phân công).
- Vòng QC: 1
- Người review và ngày: 101 (TeamDauLop), ngày 2026-11-17.
- Đã duyệt / cả lô: Duyệt toàn bộ 40/40 ảnh (`full`), tỉ lệ ảnh lỗi 0/40 (0.0% <= ngưỡng 2.0%), IoU trung bình trước calibration 0.848 (518 cặp ghép) và đồng thuận lớp 88.8%.
- Quyết định: Release
- Lý do, người nhận và phạm vi sử dụng: Đã xử lý 968 ca bất đồng qua `calibration_decisions.csv`, cập nhật quy tắc `R-01` đến `R-11` vào `guideline_v1.1_calibrated.md`; bàn giao cho đội ngũ huấn luyện mô hình 2D Object Detection trên tập dữ liệu giao thông đô thị.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: Truy vết đầy đủ từ `QC_REPORT.md` -> `submission/qc/reviews/round-01/qc_review.csv` -> `submission/qc/calibration_decisions.csv` -> `guideline_v1.1_calibrated.md` (`R-01`..`R-11`) -> `submission/final/labels.json`.
