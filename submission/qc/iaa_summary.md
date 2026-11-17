# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 5
- Lượt gán đủ **120/120**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 518 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 120 | 518 | 0.848 | 88.8% | 57.1% | 89.2% | 230/215 | 19 | CHƯA ĐẠT |
| 101|102 | 13 | 54 | 0.826 | 90.7% | 48.1% | 83.3% | 19/24 | 0 | CHƯA ĐẠT |
| 101|103 | 12 | 47 | 0.798 | 76.6% | 38.3% | 83.0% | 7/41 | 0 | CHƯA ĐẠT |
| 101|104 | 12 | 67 | 0.818 | 94.0% | 29.9% | 83.6% | 4/46 | 12 | CHƯA ĐẠT |
| 101|105 | 11 | 29 | 0.810 | 96.6% | 58.6% | 82.8% | 27/6 | 0 | CHƯA ĐẠT |
| 102|103 | 13 | 87 | 0.850 | 90.8% | 65.5% | 95.4% | 16/12 | 0 | CHƯA ĐẠT |
| 102|104 | 11 | 63 | 0.866 | 84.1% | 60.3% | 93.7% | 3/44 | 3 | CHƯA ĐẠT |
| 102|105 | 11 | 32 | 0.835 | 87.5% | 71.9% | 90.6% | 25/2 | 0 | CHƯA ĐẠT |
| 103|104 | 11 | 56 | 0.891 | 85.7% | 71.4% | 94.6% | 12/36 | 1 | CHƯA ĐẠT |
| 103|105 | 12 | 37 | 0.871 | 91.9% | 59.5% | 94.6% | 27/3 | 1 | CHƯA ĐẠT |
| 104|105 | 14 | 46 | 0.899 | 91.3% | 76.1% | 84.8% | 90/1 | 2 | CHƯA ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 968 ({'iou_thap': 189, 'sai_lop': 58, 'sai_thuoc_tinh': 248, 'bo_sot_thua': 312, 'bat_dong_ignore': 161}), trên 35 ảnh cần calibration.
