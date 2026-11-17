# Thông tin nhóm

- Tên nhóm: TeamDauLop

- Mã thành viên (ba chữ số): 101, 102, 103, 104, 105

- Người điều phối: 104 (Lã Thái Hòa — Điều phối kỹ thuật, chuyên môn, quy tắc và kiểm thử toàn nhóm); 101 (Hỗ trợ thao tác)

- Người review bản cuối: 104 (Chủ trì rà soát chất lượng & giải quyết bất đồng) & 101

- Commit chốt bài: Completed submission


## Kết quả tóm tắt

- Lượt gán hoàn tất / được giao: 120 / 120 lượt (40 ảnh × 3 người gán chéo)

- IAA trước calibration (kèm mẫu số): Mean IoU = 0.848 (518 cặp box ghép được); Class Agreement = 88.8% (460 / 518 cặp); truncated = 89.2%; occluded_level = 57.1%

- Số ảnh phải calibration: 35 / 40

- Vòng QC và số ảnh đã duyệt / cả lô: Round 01 — 40 / 40 ảnh

- Tỉ lệ lỗi: 0.0% theo ảnh (0/40), 0.0% theo đối tượng (0/370)

- **Quyết định** (Release / Hold / Rework) và lý do một dòng: **Release** — Hoàn tất 120/120 lượt gán, chốt guideline v1.1 sau calibration và đạt 0 lỗi khi chạy kiểm thử toàn diện (`check_submission.py`).


## Đánh giá mức độ hoàn thành & Đóng góp qua các Pha (Theo hướng dẫn của Labcoach Glenn-Quagmire)

Bảng tổng hợp phản ánh trung thực mức độ tham gia và trách nhiệm thực tế của từng thành viên qua 6 pha thực hành Lab Day 07:

| Mã thành viên | Pha 1 (Soi ảnh & Guideline cá nhân) | Pha 2 (Họp thống nhất v1.0 & Taxonomy) | Pha 3 (Gán nhãn độc lập 24 ảnh CVAT) | Pha 4 (Đo IAA & Soi bất đồng) | Pha 5 (Họp Calibration & Chốt v1.1) | Pha 6 (Duyệt QC & Đóng gói bàn giao) | Mức độ hoàn thành | Đánh giá & Kết luận |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **104** (Lã Thái Hòa) | ✔ Hoàn thành | ✔ Tác giả Taxonomy v1.0 & 11 quy tắc R-01..R-11 | ✔ 24 ảnh (241 box chi tiết nhất nhóm) | ✔ Phân tích 968 ca bất đồng | ✔ **DUY NHẤT 104 THỰC HIỆN**: Tự gom nhãn, đo đạc, chốt 968 ca calibration, soạn Guideline v1.1, sửa lỗi font/JSON cả nhóm | ✔ Tự xây dựng Tool QA/QC & Audit đối chiếu, đạt chuẩn Release | **100% (+ Gánh vác Pha 4–6)** | **XUẤT SẮC (Điều phối chính & Trụ cột kỹ thuật)** — Người điều phối thực tế của nhóm: chủ động thúc đẩy các thành viên nhưng cả 4 bạn đều không tham gia Pha 5; 104 tự tay gánh vác toàn bộ việc chuẩn hóa, xây dựng công cụ kiểm thử và cứu toàn bộ đồ án đạt 0 lỗi. |
| **101** | ✔ Hoàn thành | ✔ Tham gia trên lớp | ✔ 24 ảnh (128 box) | ⚠️ Chạy lệnh theo chỉ dẫn của 104 | ❌ Không tham gia hiệu chuẩn; chỉ dựa vào label thô tự dựng báo cáo mà không thông qua Pha 5 | ⚠️ Chỉ hỗ trợ nộp sau khi 104 hoàn tất toàn bộ | **60%** | **ĐẠT MỘT PHẦN (Thiếu chủ động điều phối)** — Chỉ hoàn thành phần việc cơ bản trên lớp; không chủ động điều phối, không tham gia đối chiếu xử lý bất đồng Pha 5 mà tự dựa vào nhãn thô dựng báo cáo; phụ thuộc hoàn toàn vào 104 để hoàn thiện bài. |
| **103** | ✔ Hoàn thành | ✔ Tham gia trên lớp | ✔ 24 ảnh (161 box) | ⚠️ Ít tương tác | ❌ Không tham gia hiệu chuẩn; chỉ dựa vào label thô tự dựng báo cáo mà không thông qua Pha 5 | ❌ Không tham gia duyệt QC | **50%** | **ĐẠT MỘT PHẦN TRÊN LỚP** — Hoàn thành gán nhãn Pha 1–3 trên lớp; sau khi rời lớp không tham gia hiệu chuẩn Pha 5 dù đã được 104 thúc đẩy, chỉ dựa vào nhãn thô tự dựng báo cáo cá nhân mà không giải quyết bất đồng theo quy định. |
| **102** | ✔ Hoàn thành | ✔ Tham gia trên lớp | ✔ 24 ảnh (153 box) | ⚠️ Ít tương tác | ❌ Vắng mặt hiệu chuẩn; chỉ dựa vào label thô tự dựng báo cáo mà không thông qua Pha 5 | ❌ Không tham gia duyệt QC | **50%** | **ĐẠT MỘT PHẦN TRÊN LỚP** — Hoàn thành gán nhãn Pha 1–3 trên lớp; sau khi rời lớp không tham gia hiệu chuẩn Pha 5 dù đã được 104 thúc đẩy, chỉ dựa vào nhãn thô tự dựng báo cáo cá nhân mà không giải quyết bất đồng theo quy định. |
| **105** | ✔ Hoàn thành trên lớp | ✔ Tham gia trên lớp | ✔ 24 ảnh (82 box sơ sài) | ❌ Không tương tác | ❌ Vắng mặt hoàn toàn | ❌ Không commit Git | **30% (0% sau lớp)** | **KHÔNG ĐẠT (Không đủ điều kiện Review)** — Sau khi rời lớp mất liên lạc hoàn toàn; nhóm đã tạo nhóm, gọi điện, tag tên nhiều lần nhưng không phản hồi, không commit Git, làm chậm tiến độ nhóm. Ghi nhận theo đúng hướng dẫn của Labcoach Glenn-Quagmire. |

> **Biên bản ghi nhận thực tế từ Điều phối viên chính (104 - Lã Thái Hòa):**
> 1. Trong suốt quá trình làm bài sau giờ học, thành viên 104 đã liên tục nhắn tin, tag tên và gọi điện thúc đẩy cả 4 thành viên (101, 102, 103, 105) tham gia buổi họp Calibration Pha 5 để giải quyết các ca bất đồng theo đúng quy trình Lab. Tuy nhiên, cả 4 thành viên đều không tích cực tương tác, kéo dài thời gian và né tránh thực hiện Pha 5.
> 2. Cả 3 thành viên (101, 102, 103) hoàn toàn không trải qua quá trình hiệu chuẩn Pha 5, mà chỉ dựa vào bộ nhãn thô ban đầu (khi còn đầy bất đồng, lệch class và box) rồi tự dựng báo cáo cá nhân của mình để hoàn thành bài nộp một cách hình thức mà không thông qua quá trình đối chiếu giải quyết bất đồng thực chất của Pha 5.
> 3. Thành viên 104 là người duy nhất đứng ra tự mình gánh vác toàn bộ: gom nhãn của cả nhóm, đo lường chéo IAA, phân tích xử lý triệt để 968 ca bất đồng, chốt bảng quyết định calibration (`calibration_decisions.csv`), soạn thảo bộ 11 quy tắc chuẩn hóa `R-01` đến `R-11`, nâng cấp tài liệu `guideline_v1.1_calibrated.md`, sửa lỗi mã hóa UTF-8 cho các file của đồng đội, đồng thời xây dựng công cụ kiểm thử đối chiếu trực quan QA/QC Dashboard để đưa toàn bộ lô dữ liệu về chuẩn Release với 0 lỗi.
> 4. Biên bản này được lưu vết trên kho Git theo đúng hướng dẫn của Labcoach Glenn-Quagmire (phòng C201 ngày 17/11/2026) nhằm đảm bảo sự minh bạch và công bằng tuyệt đối trong việc chấm điểm cá nhân của từng thành viên.
