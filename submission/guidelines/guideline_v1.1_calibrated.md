# Guideline gán nhãn — v0 (bản khởi đầu)

> **Đây là bản do giảng viên viết nhanh. Nó CHƯA đủ dùng.**
> Đừng sửa file này. Hãy copy sang `submission/guidelines/individual/` rồi sửa bản của bạn
> (xem `GUIDE.md`, Pha 1).

- Phiên bản: v0
- Loại nhãn: bounding box (hộp chữ nhật) trên ảnh đường phố
- Công cụ: CVAT, xuất COCO 1.0

---

## 1. Mục tiêu

Nhãn dùng để huấn luyện mô hình phát hiện **người và phương tiện** trên ảnh đường phố
(ví dụ cho camera giao thông hoặc xe tự lái).

## 2. Lớp và thuộc tính

### Lớp (mỗi vật thuộc đúng một lớp)

| Lớp | Định nghĩa |
|---|---|
| `car` | Ô tô con. |
| `truck` | Xe tải. |
| `van` | Xe thùng kín cỡ nhỏ. |
| `motorcycle` | Xe máy, mô tô. |
| `person` | Người. |
| `ignore_region` | Vùng không cần gán từng vật. |

### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | `0`, `1`, `2`, `3` | Mức bị che: 0 là thấy rõ, 1 là bị che ít, 2 là bị che nhiều, 3 là không rõ. |
| `truncated` | `no`, `yes` | Vật bị cắt ở mép ảnh. |
| `can_review` | tick / không | Tick khi bạn không chắc, để người khác xem lại. |

## 3. Quy tắc vẽ box

1. Gán box cho mọi người và phương tiện trong ảnh.
2. Box ôm sát vật.
3. Chọn đúng lớp và thuộc tính.

## 4. Ca khó

(chưa có)

## 5. QC và phiên bản

- Mọi thay đổi làm đổi nhãn đều tăng phiên bản guideline.
- Ca mơ hồ: tick `can_review`.


---

## 6. B? sung Hi?u ch?nh sau H?p Calibration (`v1.1` - 2026-11-17)

1. **Chu?n h?a `occluded_level` cho ng??i ?i?u khi?n & h?nh kh?ch (`R-09` m? r?ng):**
   - Ng??i l?i xe m?y/xe ??p kh?ng b? ph??ng ti?n kh?c che ch?n th?n tr?n: gi? m?c `"0"` (ph?n ch?n b? che nh? b?i y?m xe d??i 10% v?n t?nh l? `"0"`).
   - Ng??i ng?i sau b? l?ng ng??i l?i che t? 35%?65% di?n t?ch c? th?: g?n `"2"`.
2. **Ng??ng t?ch box ??n l? v? v?ng `ignore_region` (`R-03` m? r?ng):**
   - ??i t??ng c? chi?u cao `< 20px` ? h?u c?nh xa: n?u ??ng th?nh c?m t? 5 ??i t??ng tr? l?n th? khoanh 1 box `ignore_region` chung; n?u ??ng ??n l? d??i 20px th? b? qua, kh?ng v? box l?.
   - ?? khoanh `ignore_region` th? tuy?t ??i kh?ng v? ?? box con b?n trong.
3. **Th?ng nh?t c?c ca bi?n ph??ng ti?n ??c th? (`R-05`, `R-07`, `R-08`):**
   - Xe b?n t?i (pickup) th?ng nh?t 100% g?n `truck`; xe minivan/MPV vu?ng v?n g?n `van`; xe 3 b?nh kh?ng mui g?n `three_wheel`, c? mui v?m li?n g?n `tuk_tuk`.
