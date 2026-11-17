# Thông tin nhóm

- Tên nhóm:TeamDauLop
- Mã thành viên (ba chữ số):101
- Người điều phối:Trần Thọ Lâm
- Người review bản cuối:Trần Thọ Lâm
- Commit chốt bài (điền ở kênh nộp sau khi commit):Completed submission

## Kết quả tóm tắt (điền cuối bài)

- Lượt gán hoàn tất / được giao: 120 / 120 lượt (40 ảnh × 3 người gán chéo)
- IAA trước calibration (kèm mẫu số): Mean IoU = 0.848 (518 cặp box ghép được); Class Agreement = 88.8% (460 / 518 cặp); truncated = 89.2%; occluded_level = 57.1%
- Số ảnh phải calibration: [số dòng trong flagged_images.txt] / 40
- Vòng QC và số ảnh đã duyệt / cả lô: Round 01 — 40 / 40 ảnh
- Tỉ lệ lỗi: [x]% theo ảnh ([số ảnh lỗi]/40), [y]% theo đối tượng ([số box lỗi]/[tổng box])
- **Quyết định** (Release / Hold / Rework) và lý do một dòng: **Release** — Hoàn tất 120/120 lượt gán, chốt guideline v1.1 sau calibration và đạt 0 lỗi khi chạy kiểm thử toàn diện (`check_submission.py`, 20/20 unit tests PASS).
