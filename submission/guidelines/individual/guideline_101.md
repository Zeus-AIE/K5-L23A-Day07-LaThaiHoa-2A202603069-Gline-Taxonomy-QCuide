# Guideline gán nhãn — v1.0 — Nhóm [Tên/Mã nhóm]

- Phiên bản: v1.0
- Ngày: 16/11/2026
- Người viết / duyệt: [Mã thành viên nhóm trưởng, VD: 101] & Cả nhóm

## 1. Mục tiêu
Bộ dữ liệu phục vụ huấn luyện mô hình **phát hiện vật thể 2D (2D Object Detection)** cho hệ thống hỗ trợ lái xe nâng cao (ADAS) và giám sát giao thông đường phố.
- **Sai sót nguy hiểm nhất (False Negative):** Bỏ sót `person` hoặc `motorcycle` đang tham gia giao thông dẫn đến nguy cơ va chạm.
- **Sai sót gây nhiễu (False Positive):** Gán nhầm tượng, ma-nơ-canh, ảnh phản chiếu hoặc xe trưng bày thành vật thể thật khiến xe phanh gấp ảo (phantom braking).

## 2. Lớp và thuộc tính

### Lớp
| Lớp | Định nghĩa (một câu) | Ví dụ | Phản ví dụ / dễ nhầm với |
|---|---|---|---|
| `car` | Ô tô con chở khách dưới 9 chỗ có thiết kế thân liền khối hoặc sedan/SUV/hatchback. | Sedan, SUV, hatchback, taxi, xe thể thao, xe giả tưởng (concept/cosplay) *đang lăn bánh trên đường*. | Xe bán tải (`truck`), xe minivan/MPV cỡ lớn (`van`), mô hình xe trưng bày tĩnh. |
| `truck` | Phương tiện vận tải hàng hóa có thùng hàng hoặc khoang chở tách biệt/chuyên dụng. | Xe tải thùng, xe đầu kéo, xe chở xe (car carrier), xe bán tải (pickup), xe ben, xe bồn. | Xe van chở khách (`van`), tượng xe pickup đặt trên bệ quảng cáo (loại trừ). |
| `van` | Xe thùng kín có khoang hành khách hoặc khoang hàng liền khối với cabin, kích thước lớn hơn xe con. | Ford Transit, Mercedes Sprinter, xe cứu thương, xe minivan 7-9 chỗ thân hộp (MPV cỡ lớn). | Xe SUV (`car`), xe tải thùng rời (`truck`). |
| `motorcycle` | Phương tiện cơ giới 2 hoặc 3 bánh có tay lái ghi-đông, không có khoang cabin kín hoàn toàn. | Xe máy, xe tay ga, mô tô phân khối lớn, xe đạp điện, xe tuk-tuk / xe ba gác máy. | Xe đạp thường không động cơ (loại trừ), người điều khiển ngồi trên xe (`person`). |
| `person` | Con người thật đang đứng, đi bộ, ngồi hoặc điều khiển phương tiện trên đường/vỉa hè. | Người đi bộ, người lái xe máy, người đi xe đạp, trẻ em, người ngồi xe lăn. | Ma-nơ-canh, tượng người, hình nộm cảnh sát, ảnh người trên biển quảng cáo. |
| `ignore_region` | Vùng chứa nhóm đối tượng quá dày đặc hoặc ca biên không thể tách rời từng box riêng lẻ. | Đám đông người đi bộ dính chùm, bãi đỗ xe máy san sát ở xa không tách được từng chiếc. | Các vật thể đơn lẻ có thể vẽ box bình thường nhưng bị che khuất một phần. |

**Xử lý vật không thuộc lớp nào (`R-00`):**
- Các vật thể ngoài danh sách trên (xe đạp thường, xe buýt cỡ lớn nếu không nằm trong `truck`, cột đèn, thùng rác, động vật) → **Không gán nhãn** (bỏ qua hoàn toàn).
- Tuyệt đối không tự ý tạo thêm class mới ngoài 6 class trong `taxonomy.json`.

### Thuộc tính
Áp dụng cho 5 lớp vật thể chính (`car`, `truck`, `van`, `motorcycle`, `person`). Lớp `ignore_region` không có thuộc tính.

1. **`occluded_level` (Mức độ che khuất bởi vật thể khác trong ảnh):**
   - `"0"`: Không bị che khuất (nhìn thấy 100% phần thân hướng về camera).
   - `"1"`: Che khuất nhẹ từ **1% – 30%** diện tích vật thể.
   - `"2"`: Che khuất trung bình từ **31% – 70%** diện tích vật thể.
   - `"3"`: Che khuất nặng từ **71% – 80%** diện tích nhưng vẫn nhận diện chắc chắn được loại vật thể. *(Nếu bị che lớn hơn 80% và không đoán được hình dáng → bỏ qua không vẽ)*.
2. **`truncated` (Mức độ cắt mép khung hình):**
   - `"no"`: Toàn bộ vật thể nằm trọn bên trong biên ảnh.
   - `"yes"`: Vật thể chạm sát cạnh trên/dưới/trái/phải của ảnh và bị cắt mất một phần cơ thể/thân xe.
3. **`can_review` (Cờ đánh dấu cần thảo luận):**
   - `"false"` (mặc định): Đã áp dụng đúng quy tắc trong guideline.
   - `"true"`: Ca biên nhập nhằng chưa có quy tắc xử lý rõ ràng, cần đưa ra họp Calibration.

## 3. Quy tắc vẽ box

- **`R-01` (Box ôm sát - Tight Box):** Vẽ hình chữ nhật ôm sát biên ngoài cùng của **phần nhìn thấy được (visible area)** của vật thể, bao gồm cả gương chiếu hậu, bánh xe, ba-lô/mũ bảo hiểm (với người). Không bao gồm bóng đổ trên mặt đường và không đoán mò phần bị vật khác che mất.
- **`R-02` (Người đi xe máy / xe đạp điện):** Tách thành **2 box độc lập**:
  - 1 box `person` ôm trọn cơ thể người lái (từ đầu/mũ bảo hiểm xuống hết bàn chân).
  - 1 box `motorcycle` ôm trọn chiếc xe (từ đầu xe/gương tới đuôi xe và bánh xe). Hai box được phép giao nhau ở phần chân người đè lên thân xe.
- **`R-03` (Xe chở xe - Car Carrier):** Vẽ **1 box `truck`** bao trọn cả xe đầu kéo và phần rơ-moóc chở hàng. Đồng thời vẽ **box `car` riêng** cho từng chiếc ô tô nằm trên thùng nếu nhìn thấy từ **50% thân xe trở lên** và xe đó nguyên vẹn.
- **`R-04` (Tượng, ma-nơ-canh, tượng pickup, xe giả tưởng):**
  - Ma-nơ-canh trước cửa hàng, tượng người bằng đá/đồng, hình nộm → **Không gán `person`**.
  - Tượng xe pickup, xe mô hình gắn trên bệ cao/biển quảng cáo 3D → **Không gán `truck`/`car`**.
  - Xe giả tưởng (xe cosplay, xe độ hình thù lạ) → Chỉ gán (`car` hoặc `truck` theo kích thước) nếu **có bánh xe và đang lưu thông/đỗ trên mặt đường**; nếu đặt tĩnh trong khu trưng bày thì bỏ qua.
- **`R-05` (Tuk-tuk và xe ba gác):** Xếp toàn bộ xe tuk-tuk, xe lam, xe ba gác chở hàng có động cơ vào lớp **`motorcycle`**.
- **`R-06` (Phản chiếu và hình in):** Tuyệt đối **không gán** người hoặc xe xuất hiện qua gương chiếu hậu, mặt kính tòa nhà, vũng nước, màn hình LED hoặc poster/biển quảng cáo ngoài trời.
- **`R-07` (Xe chủ - Ego Vehicle):** Phần nắp ca-pô, cần gạt nước hoặc gương của chính chiếc xe gắn camera thu thập dữ liệu → **Không gán**.
- **`R-08` (Vật quá nhỏ - Small Objects):** Bỏ qua không vẽ nếu vật thể ở quá xa có chiều dài cạnh lớn nhất **nhỏ hơn 15 pixel** (hoặc không thể phân biệt rõ là `car` hay `van`/`truck`).
- **`R-09` (Đám đông / Cụm xe dày đặc):** Khi có từ **5 đối tượng cùng loại trở lên** đứng/đỗ san sát chồng lấn nhau ở hậu cảnh khiến không thể xác định ranh giới từng box đơn lẻ → Vẽ **1 box `ignore_region`** bao trùm cả cụm đó.

## 4. Ca khó
*(Điền danh sách các ảnh cụ thể nhóm bạn tìm thấy trong thư mục `data/images/` sau khi soi 40 ảnh)*

| Ảnh | Tình huống | Rule ID | Quyết định | Lý do |
|---|---|---|---|---|
| `img_005.jpg` | Nhóm 5–6 người đi xe máy san sát tiền cảnh bị cắt mép dưới và che khuất chồng chéo; phía sau có xe van 16 chỗ và xe con lộ một phần | `R-01`, `R-02` | Vẽ riêng từng `person` thấy rõ đầu/vai (`truncated="yes"`, `occluded_level` từ `"1"` đến `"3"`), bỏ qua phần tay/chân lẻ ở sát mép trái; vẽ `motorcycle` cho phần đầu xe nhìn thấy được; gán `van` cho xe MB100 bạc | Tuy nhiều người nhưng vẫn tách được từng cá thể riêng biệt nên không dùng `ignore_region`; chỉ vẽ đúng phần nhìn thấy được (`visible box`), không đoán phần thân dưới bị cắt khỏi khung hình. |
| `img_010.jpg` | Xe đạp thồ bán hàng rong ở giữa ảnh; xe máy chở 2–3 người chồng lấn; poster hình người treo trên tường cửa hàng bên phải | `R-00`, `R-02`, `R-06` | Bỏ qua hoàn toàn chiếc xe đạp bán hàng; tách riêng `motorcycle` và từng `person` thấy rõ trên xe máy (bỏ qua phần áo trẻ nhỏ phía trước bị che >80%); không gán `person` cho ảnh người mẫu trên biển quảng cáo | Xe đạp không động cơ nằm ngoài taxonomy (`R-00`); chỉ gán `person` cho người thật và vật thể có đủ hình dáng nhận diện được, tránh nhiễu từ hình in 2D (`R-06`). |
| `img_011.jpg` | Tượng quái vật Predator khổng lồ bên phải; mô hình xe độ nghệ thuật đặt tĩnh trên thảm xanh; đám đông khán giả đứng dày đặc phía sau hàng rào | `R-04`, `R-08`, `R-09` | Không gán `person` cho tượng Predator; không gán `motorcycle` cho các xe mô hình trưng bày tĩnh trên thảm; gán 2 `person` cho cặp đôi đứng gần; vẽ `ignore_region` cho dải đám đông ở hậu cảnh; gán `van` cho xe trắng sọc xanh phía sau | Tượng và xe mô hình tĩnh thuộc khu trưng bày nghệ thuật, không có chức năng giao thông (`R-04`); đám đông ở xa chồng lấn >5 người không thể tách box đơn lẻ (`R-09`). |
| `img_014.jpg` | Hai người đi bộ sắc nét ở tiền cảnh (một người cầm túi xách); phía sau có 3 xe máy + người lái vụt qua bị nhòe chuyển động (motion blur) kéo vệt dài | `R-01`, `R-10` | Vẽ 2 box `person` sắc nét ở tiền cảnh (box người nữ ôm trọn cả túi xách cầm tay); bỏ qua hoàn toàn các bóng xe máy và người bị nhòe chuyển động phía sau | Các vệt nhòe chuyển động không có ranh giới vật lý rõ ràng, nếu gán sẽ gây lệch IoU nghiêm trọng và làm nhiễu đặc trưng biên của mô hình (`R-10`). |
| `img_022.jpg` | Ảnh đen trắng chụp xe mô tô gắn thùng bên (sidecar) đỗ trên đường đất, có 1 người ngồi trong thùng; phía cuối đường rất xa có các đốm mờ nhỏ | `R-01`, `R-02`, `R-05`, `R-08` | Vẽ 1 box `motorcycle` ôm trọn cả xe máy và thùng sidecar gắn liền; vẽ riêng 1 box `person` (`occluded_level="2"`) cho phần thân trên của người ngồi trong thùng; bỏ qua các vật thể mờ ở cuối đường | Thùng sidecar là bộ phận cơ khí gắn liền tạo thành phương tiện 3 bánh không cabin kín (`R-05`); người ngồi bên trong vẫn tách box độc lập theo `R-02`; các đốm ở xa <15px và không rõ hình dáng nên loại theo `R-08`. |
| `img_029.jpg` | Hai người tiền cảnh (một đi xe tay ga, một đi xe đạp) cùng vác thanh kim loại dài gần hết khung hình; hậu cảnh có người đi xe đạp và đi bộ | `R-00`, `R-02`, `R-08`, `R-11` | Vẽ box `person` ôm sát cơ thể từng người, không kéo box theo thanh kim loại dài; gán `motorcycle` cho xe tay ga bên trái nhưng bỏ qua chiếc xe đạp bên phải và các xe đạp ở hậu cảnh | Thanh kim loại nhô ra quá xa khỏi biên cơ thể, nếu bao gồm sẽ làm sai lệch kích thước thực của đối tượng (`R-11`); xe đạp không động cơ nằm ngoài danh sách lớp (`R-00`). |

## 5. QC và phiên bản
- **Ngưỡng QC áp dụng:** Tuân thủ cây quyết định trong `docs/QC_CHEATSHEET.md` (kiểm tra đủ số lượng ảnh trong file COCO, khớp 100% schema `taxonomy.json`, đạt ngưỡng đồng thuận IoU và tỷ lệ lỗi lô ≤ 2% để **Release**).
- **Người quyết định ca mơ hồ:** Nhóm trưởng (`[Mã nhóm trưởng]`) tổng hợp ý kiến cả nhóm trong buổi họp Calibration và chốt phương án cuối cùng.

| Phiên bản | Thay đổi | Lý do (ca nào) |
|---|---|---|
| `v0` | Bản khởi đầu của đề bài, thiếu quy tắc xử lý ca biên và ngưỡng thuộc tính. | Khởi tạo ban đầu. |
| `v1.0` | Bổ sung định nghĩa 6 lớp, ngưỡng % cho `occluded_level`, và bộ quy tắc `R-00` đến `R-09`. | Chốt sau khi cả nhóm rà soát toàn bộ 40 ảnh ở Pha 1–2. |