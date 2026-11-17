# Guideline Gán Nhãn Hiệu Chuẩn — v1.1 — Nhóm TeamDauLop

- **Phiên bản**: `v1.1` (Bản hiệu chuẩn sau buổi họp Calibration Pha 5)
- **Ngày hiệu chuẩn**: 2026-11-17
- **Đội ngũ biên soạn & đồng thuận**: Toàn bộ thành viên TeamDauLop (`101` - Trưởng nhóm, `102`, `103`, `104`, `105`)
- **Tập tin Taxonomy chuẩn**: `submission/taxonomy.json` (Phiên bản `v1.0`, 10 lớp đối tượng)
- **Tập tin khai báo thay đổi**: `submission/guideline_change.json` (`kind: editorial`)

---

## 1. Mục tiêu Dự án & Bối cảnh Huấn luyện

Bộ dữ liệu phục vụ huấn luyện mô hình **Phát hiện Vật thể 2D (2D Object Detection)** ứng dụng trong hệ thống hỗ trợ lái xe nâng cao (ADAS) và camera giám sát giao thông đô thị thông minh.

Mô hình học trực tiếp từ bounding box hình học và nhãn phân loại, do đó sự sai lệch hoặc thiếu nhất quán sẽ dẫn đến các hậu quả nghiêm trọng:
- **Sai sót nguy hiểm nhất (False Negative - Bỏ sót vật thể thật)**: 
  - Bỏ sót người đi bộ (`person`), người điều khiển xe máy (`motorcycle`) hoặc xe đạp (`bicycle`) đang tham gia giao thông. Điều này khiến xe tự hành không kích hoạt phanh tự động khẩn cấp (AEB), dẫn đến tai nạn trực tiếp.
- **Sai sót gây phiền toái & mất an toàn (False Positive - Nhận nhầm vật thể giả)**:
  - Gán nhầm ma-nơ-canh trong tủ kính, tượng điêu khắc nghệ thuật (như tượng Predator), hình người in trên pano quảng cáo 2D thành `person`.
  - Hậu quả: Xe tự hành nhận diện sai chướng ngại vật ảo trên mặt đường, gây hiện tượng phanh gấp ảo (phantom braking) làm các phương tiện phía sau đâm dồn toa.
- **Sai lệch biên hình học (IoU < 0.70)**: Box vẽ quá rộng (ôm cả bóng đổ, thanh sắt vươn dài) hoặc quá chật (cắt mất đầu, bánh xe) làm mô hình hồi quy biên (box regression) bị trôi và dự đoán khoảng cách thực tế bị sai.

---

## 2. Lớp Đối tượng và Thuộc tính (Taxonomy v1.0)

### 2.1. Danh mục 10 Lớp Đối tượng (Classes)

Toàn bộ nhóm thống nhất tuân thủ nghiêm ngặt 10 lớp đã được khóa trong `submission/taxonomy.json` (v1.0). Tên lớp viết theo định dạng `snake_case` (không có dấu cách hay dấu gạch nối):

| STT | Tên Lớp (`name`) | Định nghĩa chuẩn xác (1 câu) | Ví dụ bao gồm | Phản ví dụ / Dễ nhầm lẫn |
| :---: | :--- | :--- | :--- | :--- |
| 1 | `car` | Ô tô con chở người dưới 9 chỗ ngồi, thân liền khối hoặc kiểu dáng sedan, hatchback, SUV. | Sedan, SUV, CUV, hatchback, taxi con, xe coupe thể thao, xe tạo hình đặc biệt (concept/cosplay) đang lăn bánh. | Xe bán tải cabin kép (`truck`), xe minivan/MPV cỡ lớn (`van`), mô hình vỏ xe treo trang trí. |
| 2 | `truck` | Phương tiện cơ giới chuyên chở hàng hóa có thùng hàng riêng biệt, xe đầu kéo hoặc xe bán tải. | Xe tải thùng kín, xe tải ben, xe đầu kéo container, xe bồn, xe chở ô tô chuyên dụng (car carrier), xe bán tải (pickup truck). | Xe van thùng kín chở người (`van`), kiện hàng rời đặt dưới mặt đường. |
| 3 | `van` | Xe ô tô chở người hoặc hàng hóa có khoang sau liền khối cabin, dáng hình hộp vuông vắn. | Ford Transit, Mercedes Sprinter, xe cứu thương, xe chở thư báo chuyên dụng, xe minivan MPV 7–9 chỗ cỡ lớn. | Xe SUV 7 chỗ gầm cao (`car`), xe tải cabin rời (`truck`). |
| 4 | `bus` | Xe ô tô chở khách cỡ lớn chuyên dụng có sức chứa từ 16 chỗ ngồi trở lên. | Xe buýt nội đô, xe khách đường dài liên tỉnh, xe buýt du lịch 2 tầng/mui trần. | Xe khách nhỏ 12–16 chỗ thân hẹp (`van`), trạm chờ xe buýt. |
| 5 | `motorcycle` | Phương tiện cơ giới 2 hoặc 3 bánh điều khiển bằng tay lái ghi-đông, không có cabin kín hoàn toàn. | Xe máy số, xe tay ga, mô tô phân khối lớn, xe máy điện, tổ hợp xe mô tô gắn thùng bên (sidecar). | Xe đạp không động cơ (`bicycle`), người lái ngồi trên xe (`person`). |
| 6 | `bicycle` | Phương tiện 2 bánh di chuyển hoàn toàn bằng sức đạp của con người, không gắn động cơ. | Xe đạp thể thao, xe đạp mini đường phố, xe đạp địa hình, xe đạp thồ hàng rong. | Xe đạp điện / xe máy điện (`motorcycle`), xe xích lô đạp (`three_wheel`). |
| 7 | `three_wheel` | Phương tiện 3 bánh (cơ giới hoặc thô sơ) dùng chở hàng hoặc chở người, không thuộc kiểu dáng xe tuk-tuk. | Xe ba gác máy chở hàng, xe lôi tự chế, xe xích lô đạp truyền thống, xe máy 3 bánh chuyển đổi. | Xe tuk-tuk chở khách có mái vòm liền (`tuk_tuk`), xe máy 2 bánh (`motorcycle`). |
| 8 | `tuk_tuk` | Phương tiện 3 bánh chở khách đặc trưng có mái che liền khối từ đầu xe đến khoang hành khách và tay lái ghi-đông. | Xe tuk-tuk Thái Lan, auto-rickshaw Ấn Độ, xe lam chở khách truyền thống có mui vòm. | Xe ba gác chở hàng thùng hở không mui (`three_wheel`), xe taxi con 4 bánh (`car`). |
| 9 | `person` | Con người sinh học thật (còn sống) ở bất kỳ tư thế nào (đi, đứng, ngồi, lái xe, bế ẵm). | Người đi bộ qua đường, người điều khiển xe máy, hành khách ngồi sau xe, người ngồi ăn vỉa hè. | Ma-nơ-canh cửa hàng, tượng đá/đồng/quái vật Predator, tranh ảnh người mẫu in 2D trên biển quảng cáo. |
| 10 | `ignore_region` | Vùng chữ nhật bao trùm nhóm đối tượng dày đặc, đám đông dính chùm hoặc vật thể quá xa không thể tách box đơn lẻ. | Đám đông khán đài lễ hội, đoàn người diễu hành xa xôi, bãi đỗ xe máy san sát ở hậu cảnh. | Vật thể đơn lẻ đứng riêng biệt có thể vẽ box độc lập; nền trời, mặt đường trống. |

> **Quy tắc ngoại lệ (`R-00 - Vật ngoài Taxonomy`)**:  
> Các đối tượng không thuộc 10 lớp trên (chó mèo, chim chóc, cột điện, thùng rác, cây cối, gờ giảm tốc) $\rightarrow$ **Tuyệt đối không gán nhãn** (bỏ qua hoàn toàn).

---

### 2.2. Hệ thống Thuộc tính (Attributes)

Áp dụng bắt buộc cho **9 lớp đối tượng vật lý** (`car`, `truck`, `van`, `bus`, `motorcycle`, `bicycle`, `three_wheel`, `tuk_tuk`, `person`). Riêng lớp `ignore_region` không mang thuộc tính.

#### 1. `occluded_level` (Mức độ che khuất bởi vật thể khác trong ảnh)
Được đo lường bằng tỉ lệ diện tích bề mặt nhìn thấy so với toàn bộ hình thể vật lý ước tính:
- `"0"` (**Không che khuất**): Thấy trọn vẹn từ $\ge 90\%$ hình thể vật thể hướng về ống kính camera.
- `"1"` (**Che khuất nhẹ**): Bị che khuất từ $10\%$ đến dưới $35\%$ diện tích (ví dụ: bánh xe bị che bởi lề đường, người bị che một phần cẳng chân).
- `"2"` (**Che khuất trung bình**): Bị che khuất từ $35\%$ đến $65\%$ diện tích (ví dụ: ô tô chỉ thấy nửa thân trên, người ngồi sau xe máy bị lưng người trước che khuất một nửa).
- `"3"` (**Che khuất nặng**): Bị che khuất trên $65\%$ diện tích, nhưng phần nhìn thấy vẫn còn đủ đặc trưng hình học nhận dạng chắc chắn ($\ge 15\%$ diện tích).

#### 2. `truncated` (Bị cắt cụt bởi mép viền ảnh)
- `"no"`: Toàn bộ vật thể nằm trọn vẹn bên trong 4 cạnh viền của khung ảnh.
- `"yes"`: Vật thể bị cắt ngang bởi mép viền ảnh (biên trên, dưới, trái hoặc phải). Box dừng lại chính xác tại pixel mép ảnh.

#### 3. `can_review` (Đánh dấu ca khó/nghi ngờ)
- `false` (Mặc định): Nhãn rõ ràng, tự tin, đúng theo guideline.
- `true`: Ca biên mơ hồ, ánh sáng quá yếu hoặc góc khuất phức tạp cần cả nhóm rà soát trong buổi họp Calibration.

---

## 3. Bộ 11 Quy tắc Gán Nhãn Thực Chiến (Rules R-01 đến R-11)

### `R-01`: Bounding Box Ôm Khít (Tight Bounding Box)
- Bounding box phải là hình chữ nhật nhỏ nhất bao trọn **toàn bộ phần nhìn thấy được (visible pixels)** của vật thể.
- **Bao gồm**: Toàn bộ thân xe, cản va trước/sau, gương chiếu hậu hai bên, lốp xe tiếp đất, biển số xe. Đối với người: bao gồm quần áo, giày dép, mũ bảo hiểm, ba-lô đeo trên lưng và túi xách cầm sát cơ thể.
- **Loại trừ**: Tuyệt đối **không vẽ lan ra bóng đổ (shadow)** trên mặt đường hoặc luồng khói xả.

### `R-02`: Giới hạn Ước lượng Phần Che khuất (No Hallucination)
- Chỉ vẽ box bao quanh phần thực tế nhìn thấy được trên ảnh.
- Trường hợp vật bị che khuất một phần bởi cột đèn hoặc phương tiện khác: vẽ box bao trùm các mảnh nhìn thấy được tạo thành một khối liên tục hợp lý.
- Tuyệt đối **không "vẽ mò / viễn tưởng"** kéo dài box xuống phần thân dưới bị che khuất $> 85\%$ (ví dụ: chỉ thấy mỗi chiếc đầu người sau bức tường cao thì box dừng ở mép tường, không vẽ box dài xuống chân).

### `R-03`: Vùng Bỏ Qua Đám Đông (`ignore_region`)
- Chỉ sử dụng khi xuất hiện đám đông $\ge 5$ người dính chùm, chồng lấn phức tạp không thể phân biệt ranh giới từng người, hoặc bãi đỗ xe máy ở hậu cảnh xa xôi.
- Khi đã vẽ 1 box `ignore_region` bao trùm vùng đó thì **không vẽ thêm các box đơn lẻ bên trong vùng ignore** để tránh xung đột đánh giá IoU.

### `R-04`: Thực thể Phi Sinh học & Tác phẩm Nghệ thuật (Non-Biological Entities)
- **Ma-nơ-canh (Mannequins)**: Người giả bằng nhựa/thạch cao đứng sau tủ kính hoặc trước cửa hàng thời trang $\rightarrow$ **TUYỆT ĐỐI KHÔNG GÁN `person`** (Điển hình: `img_033.jpg` để trống 0 box).
- **Tượng điêu khắc**: Tượng quái thú Predator (`img_011.jpg`), tượng danh nhân bằng đồng/đá, tượng động vật bằng gỗ (`img_028.jpg`) $\rightarrow$ **KHÔNG GÁN NHÃN**.
- **Mô hình tĩnh**: Khung vỏ ô tô treo trang trí trên nóc (`img_024.jpg`), mô hình xe trưng bày trên bệ cố định $\rightarrow$ **KHÔNG GÁN NHÃN**.

### `R-05`: Phân định Nhất quán giữa các Lớp Phương tiện
- Phân biệt rõ xe con (`car`) và xe khách nhỏ (`van`): Nếu là xe minivan dáng hộp vuông vắn (như Toyota Hiace, Ford Transit) $\rightarrow$ Gán `van`. Nếu là SUV/Crossover gầm cao $\rightarrow$ Gán `car`.
- Phân biệt `three_wheel` và `tuk_tuk`: Xe có khoang hành khách mái vòm liền khối từ đầu xe ra sau $\rightarrow$ Gán `tuk_tuk`. Xe ba gác chở hàng thùng hở, xe xích lô $\rightarrow$ Gán `three_wheel`.

### `R-06`: Tranh ảnh 2D, Áp phích và Biển Quảng cáo
- Hình ảnh người mẫu, ca sĩ, vận động viên in trên biển pano tấm lớn, poster dán tường, áp phích cột đèn $\rightarrow$ **TUYỆT ĐỐI KHÔNG GÁN `person`**.
- Hình ảnh xe cộ in 2D trên biển hiệu $\rightarrow$ **KHÔNG GÁN NHÃN**.
- Hình ảnh phản chiếu qua gương cầu lồi hoặc kính tòa nhà $\rightarrow$ **KHÔNG GÁN NHÃN**.

### `R-07`: Phân loại Xe Bán tải (Pickup Truck)
- Xe bán tải (Ford Ranger, Toyota Hilux, Isuzu D-Max) dù chở người hay có gắn thêm nắp thùng cuộn/canopy cao $\rightarrow$ **Thống nhất 100% gán nhãn `truck`** (do có chassis tải và thùng hàng riêng biệt).

### `R-08`: Ranh giới Xe Tải Thùng hở & Xe Chuyên dụng
- **Xe chở ô tô chuyên dụng (Car Carrier - `img_002.jpg`)**:
  - Vẽ **1 box lớn `truck`** bao trọn toàn bộ xe (từ cabin đầu kéo đến hết chiều dài rơ-moóc 2 tầng).
  - Vẽ **box `car` riêng biệt cho từng ô tô con** được chở trên giá đỡ (phải vẽ đủ cả các xe ở tầng dưới nếu nhìn thấy, tổng cộng 8 xe con trên ảnh `img_002.jpg`).
- **Xe tải đang mở cửa bốc dỡ (`img_007.jpg`)**: Box `truck` ôm khít thân xe và mép ngoài cùng của cánh cửa mở, không bao trùm các kiện hàng đã dỡ xuống nền kho.

### `R-09`: Tách riêng Người Điều khiển và Xe Máy / Xe Đạp
- Tuyệt đối **không bao giờ gộp chung** người và phương tiện 2 bánh vào 1 box.
- Phải vẽ tách rời:
  - 1 box `person` cho người lái (từ đỉnh mũ bảo hiểm đến hết bàn chân).
  - 1 box `motorcycle` hoặc `bicycle` cho chiếc xe (từ đầu xe, tay lái đến hết bánh xe tiếp đất).
  - Nếu có thêm người ngồi sau: vẽ thêm 1 box `person` độc lập cho người ngồi sau.
  - Vùng chân người và thân xe giao nhau là hoàn toàn hợp lệ.

### `R-10`: Xử lý Vật thể Nhòe do Chuyển động (Motion Blur)
- Những vật thể di chuyển với vận tốc cao tạo ra vệt bóng mờ quét ngang màn hình (`img_014.jpg`):
  - Nếu đường biên cơ thể hoặc phương tiện bị nhòe mất dạng, không thể xác định ranh giới hình học $\rightarrow$ **Bỏ qua không gán nhãn**.
  - Chỉ gán nhãn cho những đối tượng giữ được biên dạng rõ ràng.

### `R-11`: Phương tiện Chở Hàng Cồng kềnh Quá khổ
- Xe máy hoặc người vác thanh kim loại, ống nước, cành cây dài nhiều mét nhô ra phía trước hoặc phía sau (`img_029.jpg`):
  - Box `motorcycle` và box `person` **chỉ ôm sát cơ thể người và thân xe máy**.
  - **Tuyệt đối không kéo giãn box** bao trùm toàn bộ thanh kim loại dài lòng thòng ra ngoài biên vật thể.

---

## 4. Bảng Tra Cứu Ca Khó & Thư Viện Mẫu (40 Ảnh Đường Phố)

| Tên Ảnh | Tình huống Quan sát Thực tế | Mã Quy tắc | Quyết định Đồng thuận | Lý do Kỹ thuật |
| :--- | :--- | :---: | :--- | :--- |
| `img_001.jpg` | Khách ngồi ăn san sát trên vỉa hè; xe tải bán hàng đậu lề đường bên trái. | `R-01`, `R-09` | Vẽ từng `person` thấy rõ thân trên; gán 1 `truck` cho xe tải; vùng quá xa dùng `ignore_region`. | Tách từng cá thể riêng biệt phục vụ cảnh báo va chạm lề đường. |
| `img_002.jpg` | Xe tải chuyên dụng 2 tầng chở 8 ô tô con mới. | `R-01`, `R-08` | Vẽ **1 box lớn `truck`** bao trọn xe chuyên dụng + **8 box `car` riêng** cho 4 xe tầng trên và 4 xe tầng dưới. | Tránh bẫy bỏ sót 4 xe con tầng dưới như thành viên 105; đảm bảo phát hiện đa đối tượng lồng nhau. |
| `img_005.jpg` | Hai xe máy chở người chạy song song trên đường nhựa. | `R-01`, `R-09` | Vẽ đúng **4 boxes**: 2 box `person` riêng + 2 box `motorcycle` riêng. | Đồng thuận 100% giữa 101 và 104; phân tách người và xe cơ giới. |
| `img_007.jpg` | Hai xe tải giao hàng mở cửa sau dỡ hàng trong kho bãi. | `R-01`, `R-08` | Gán nhãn `truck` cho 2 xe tải; box ôm sát mép cửa xe mở, không ôm kiện hàng sàn kho. | Kiện hàng đã rời khỏi xe thuộc phạm vi hạ tầng kho bãi, không phải bộ phận xe. |
| `img_009.jpg` | Xe buýt du lịch mui trần chở khách; người đi bộ trên vỉa hè. | `R-01`, `R-05` | Gán chiếc xe buýt lớn là `bus`; người đi bộ bên trái gán `person`. | Xe buýt cỡ lớn thuộc lớp `bus` mới trong taxonomy v1.0. |
| `img_010.jpg` | Người đi bộ băng qua đường; xe máy chở người; poster in 2D trên tường. | `R-06`, `R-09` | Tách riêng `person` và `motorcycle`; bỏ qua hoàn toàn poster quảng cáo in 2D. | Triệt tiêu nguy cơ False Positive từ tranh ảnh quảng cáo vỉa hè (`R-06`). |
| `img_011.jpg` | Tượng quái vật Predator khổng lồ; khách tham quan triển lãm; đám đông phía sau. | `R-03`, `R-04` | **Tuyệt đối KHÔNG gán nhãn cho tượng Predator**; gán `person` cho khách thật; dùng `ignore_region` cho đám đông xa. | Tượng điêu khắc là vật thể phi sinh học (`R-04`); chống phanh gấp ảo. |
| `img_012.jpg` | Xe bán tải (pickup) cabin kép và ô tô con chạy cạnh nhau. | `R-01`, `R-07` | Xe bán tải gán nhãn `truck`; ô tô con bên cạnh gán `car`. | Thống nhất xe bán tải xếp vào lớp `truck` (`R-07`). |
| `img_014.jpg` | Hai vũ công nhảy múa tiền cảnh sắc nét; bóng xe máy vụt qua bị nhòe chuyển động phía sau. | `R-01`, `R-10` | Vẽ đúng 2 box `person` cho 2 vũ công tiền cảnh; bỏ qua toàn bộ bóng nhòe phía sau. | Vệt nhòe chuyển động không có biên vật lý xác định, gán sẽ phá vỡ hồi quy biên (`R-10`). |
| `img_016.jpg` | Giao thông Nam Á gồm xe tuk-tuk, xe tải nhỏ và người đi bộ. | `R-01`, `R-05` | Xe tuk-tuk gán đúng nhãn `tuk_tuk`; xe tải nhỏ gán `truck`. | Sử dụng đúng mã nhãn snake_case `tuk_tuk` trong taxonomy v1.0. |
| `img_018.jpg` | Xe chạy trên cao tốc; tấm biển quảng cáo ngoài trời in hình chân dung người cỡ lớn. | `R-04`, `R-06` | Gán các xe trên đường là `car`; **tuyệt đối không gán `person` cho khuôn mặt trên biển quảng cáo**. | Tránh lỗi nhận nhầm ảnh in 2D thành người đi bộ trên cao tốc (`R-06`). |
| `img_022.jpg` | Xe mô tô gắn thùng bên (sidecar) chở người trên đường đất. | `R-01`, `R-05` | Gán 1 box `motorcycle` ôm trọn cả xe máy và thùng phụ; gán 1 box `person` riêng cho người ngồi trong thùng. | Thùng phụ gắn cơ khí liền khối với xe máy; người ngồi trong vẫn gán `person` độc lập. |
| `img_024.jpg` | Chiếc xe cổ AMC Pacer; phía trên nóc có khung vỏ xe rời treo trang trí. | `R-01`, `R-04` | Gán chiếc xe thật phía dưới là `car`; bỏ qua khung vỏ xe rời phía trên nóc. | Phụ tùng rời không phải phương tiện hoàn chỉnh có khả năng vận hành (`R-04`). |
| `img_026.jpg` | Xe máy chở 2 người trong đường hầm (Ảnh dọc 784x1024). | `R-01`, `R-09` | Vẽ đúng **3 boxes**: 1 box `person` (người lái) + 1 box `person` (người ngồi sau) + 1 box `motorcycle`. | Đồng thuận tuyệt đối giữa 101, 103 và 104. |
| `img_028.jpg` | Xe tải chở một bức tượng con ngựa bằng gỗ lớn trên thùng sau. | `R-04`, `R-08` | Vẽ 1 box `truck` bao trọn thân xe; không gán nhãn cho bức tượng ngựa gỗ. | Tượng gỗ thuộc nhóm hàng hóa phi sinh học, không phải động vật sống. |
| `img_029.jpg` | Xe máy chở bó thanh kim loại dài vượt khổ nhiều mét. | `R-09`, `R-11` | Vẽ box `person` và `motorcycle` ôm khít thân người và xe; **không kéo box bao trùm thanh kim loại**. | Ngăn chặn việc làm phình to kích thước vật thể một cách phi thực tế (`R-11`). |
| `img_033.jpg` | Tủ kính trưng bày thời trang với các tượng Ma-nơ-canh mặc quần áo. | `R-04` | **ĐỂ TRỐNG TOÀN BỘ 0 BOX (Empty Annotation)**. | Cả 5 thành viên đều đồng thuận 100%: Ma-nơ-canh không gán `person` (`R-04`). |
| `img_036.jpg` | Xe tải nhỏ CMC Veryca màu xanh chụp cận cảnh. | `R-01`, `R-08` | Gán đúng 1 box `truck` ôm khít gương chiếu hậu, cản trước và bánh xe. | Đồng thuận tuyệt đối 100% giữa toàn bộ các thành viên gán nhãn. |

---

## 5. Quy trình Đảm bảo Chất lượng (QC)

- **Độ chính xác phân loại lớp (Class Agreement)**: $\ge 95\%$ giữa các thành viên gán chéo.
- **Độ trùng khớp biên hình học (Mean IoU)**: $\ge 0.85$ cho vật thể lớn, $\ge 0.70$ cho vật thể nhỏ.
- **Chiều cao tối thiểu cho phép gán box đơn lẻ**: $\ge 20\text{ px}$.
- **Tỉ lệ lỗi tối đa cho phép trên lô bàn giao**: $\le 2.0\%$ theo số ảnh.

---

## 6. Bổ sung Hiệu chỉnh sau Họp Calibration (`v1.1` - 2026-11-17)

Sau buổi họp Calibration phân tích 968 ca candidate bất đồng từ vòng IAA, nhóm thống nhất bổ sung 3 hướng dẫn chi tiết nhằm giải quyết triệt để sự không đồng nhất:

1. **Chuẩn hóa `occluded_level` cho người điều khiển & hành khách (`R-09` mở rộng):**
   - Người lái xe máy/xe đạp không bị phương tiện khác che chắn thân trên: giữ mức `"0"` (phần chân bị che nhẹ bởi yếm xe dưới 10% vẫn tính là `"0"`).
   - Người ngồi sau bị lưng người lái che từ 35%–65% diện tích cơ thể: gán `"2"`.
2. **Ngưỡng tách box đơn lẻ và vùng `ignore_region` (`R-03` mở rộng):**
   - Đối tượng có chiều cao `< 20px` ở hậu cảnh xa: nếu đứng thành cụm từ 5 đối tượng trở lên thì khoanh 1 box `ignore_region` chung; nếu đứng đơn lẻ dưới 20px thì bỏ qua, không vẽ box lẻ.
   - Đã khoanh `ignore_region` thì tuyệt đối không vẽ đè box con bên trong.
3. **Thống nhất các ca biên phương tiện đặc thù (`R-05`, `R-07`, `R-08`):**
   - Xe bán tải (pickup) thống nhất 100% gán `truck`; xe minivan/MPV vuông vắn gán `van`; xe 3 bánh không mui gán `three_wheel`, có mui vòm liền gán `tuk_tuk`.
   - Xe chở nhiều ô tô con trên sàn (`img_002.jpg`): Gán 1 box `truck` cho xe tải đầu kéo và sàn chở, đồng thời gán riêng từng box `car` cho mỗi ô tô con nhìn thấy trên cả 2 tầng sàn.
   - Tượng đài nghệ thuật (`img_017.jpg`): Không gán box (để trống 0 box) vì là công trình điêu khắc tĩnh phi sinh học (`R-04`).
   - Xe tuk-tuk chở hàng hóa cao trên nóc (`img_035.jpg`): Box chỉ ôm sát khung và thân xe, không bao trùm khối hàng hóa chất cao trên nóc xe.

---

## 7. Nhật ký Phiên bản (Changelog)

| Phiên bản | Ngày | Tóm tắt Thay đổi Chính | Lý do & Ca ảnh Tham chiếu |
| :---: | :---: | :--- | :--- |
| `v0` | 2026-11-16 | Khởi tạo ban đầu từ đề bài, chỉ có 6 lớp cơ bản, thiếu ngưỡng đo lường cho các thuộc tính. | Khởi tạo dự án sơ bộ. |
| `v1.0` | 2026-11-17 | - Bổ sung thêm 4 lớp mới: `bus`, `bicycle`, `three_wheel`, `tuk_tuk` (chuẩn hóa tên `snake_case`).<br/>- Thiết lập định lượng % rõ ràng cho 4 mức `occluded_level` (0, 1, 2, 3).<br/>- Ban hành bộ 11 quy tắc chuẩn `R-01` đến `R-11`.<br/>- Bổ sung bảng tra cứu thực tế 18 ca biên trọng điểm. | Thống nhất sau khi 5 thành viên gán thử nghiệm và phát hiện các bẫy giao thông tại `img_002`, `img_011`, `img_026`, `img_033`. |
| `v1.1` | 2026-11-17 | - Bổ sung Mục 6 làm rõ quy ước gán `occluded_level` khi người ngồi trên xe máy/xe đạp.<br/>- Khóa cứng ngưỡng $\ge 20\text{ px}$ và quy định dứt khoát không vẽ box lồng trong `ignore_region`.<br/>- Giải quyết đồng thuận 100% cho 968 ca candidate trong `calibration_decisions.csv`. | Hiệu chuẩn sau buổi họp nhóm phân tích kết quả IAA Pha 4 và chuẩn bị bàn giao Pha 6. |
