# B?o c?o Nghi?m thu Ch?t l??ng D? li?u (QC Report) ? Nh?m TeamDauLop

- Mục đích và giới hạn sử dụng: B? d? li?u 40 ?nh giao th?ng ?? th? (372 annotations, 10 l?p) ph?c v? hu?n luy?n v? ??nh gi? m? h?nh Ph?t hi?n V?t th? 2D (ADAS / gi?m s?t giao th?ng); kh?ng d?ng cho nh?n di?n danh t?nh c? nh?n.
- Manifest và checksum nhãn cuối: `submission/qc/labels_calibrated.json` (SHA-256: `279cb687f7ef7cb2bd3a0e2c73bc718542c5e6a09d5affcb2941114b654659c1`), ??i chi?u `submission/qc/batch_manifest.json` (SHA-256: `a2e50a289b94c66ed4aa1a2065e1d4f6980e5e2f2dcccce6435173ad9e1e552a`).
- Guideline và taxonomy khi gán / khi bàn giao: G?n ch?o theo `taxonomy.json` v1.0 (10 l?p) v? `guideline_v1.0.md`; b?n giao theo `guideline_v1.1_calibrated.md` (b? sung sau Calibration, `kind: editorial`).
- Lượt hoàn thành / được giao: 120/120 l??t g?n ch?o ho?n th?nh (5 th?nh vi?n `101`?`105`, m?i th?nh vi?n 24/40 ?nh; ??t 100% ph?n c?ng).
- Vòng QC: 1
- Người review và ngày: 101 - Tr?n Th? L?m (TeamDauLop), ng?y 2026-11-17.
- Đã duyệt / cả lô: Duy?t to?n b? 40/40 ?nh (`full`), t? l? ?nh l?i 0/40 (0.0% <= ng??ng 2.0%), IoU trung b?nh tr??c calibration 0.848 (518 c?p gh?p) v? ??ng thu?n l?p 88.8%.
- Quyết định: Release
- Lý do, người nhận và phạm vi sử dụng: ?? x? l? 968 ca b?t ??ng qua `calibration_decisions.csv`, c?p nh?t quy t?c `R-01` ??n `R-11` v?o `guideline_v1.1_calibrated.md`; b?n giao cho ??i ng? hu?n luy?n m? h?nh 2D Object Detection tr?n t?p d? li?u giao th?ng ?? th?.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: Truy v?t ??y ?? t? `QC_REPORT.md` -> `submission/qc/reviews/round-01/qc_review.csv` -> `submission/qc/calibration_decisions.csv` -> `guideline_v1.1_calibrated.md` (`R-01`..`R-11`) -> `submission/final/labels.json`.
