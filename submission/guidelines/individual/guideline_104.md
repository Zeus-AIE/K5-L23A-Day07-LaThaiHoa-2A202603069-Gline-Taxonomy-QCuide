# Guideline gán nhãn — v0.1-draft — Đề xuất cá nhân Thành viên 104

- Phiên bản: `v0.1-draft` (Bản cá nhân độc lập tại mốc CP1 - Pha 1)
- Ngày thực hiện: 2026-11-16
- Người viết: Thành viên 104
- Mục đích: Khảo sát độc lập toàn bộ 40 ảnh đường phố (`img_001.jpg` – `img_040.jpg`), chỉ ra các điểm mơ hồ của `guideline_v0_starter.md` và xây dựng bản đề xuất cá nhân chuẩn bị cho buổi họp thống nhất của nhóm tại Pha 2.

---

## 1. Mục tiêu (Đề xuất của Thành viên 104)

- **Mục tiêu sử dụng nhãn:** Dữ liệu bounding box 2D được dùng để huấn luyện mô hình phát hiện đối tượng (Object Detection) phục vụ bài toán giám sát giao thông đô thị thông minh và hệ thống hỗ trợ lái xe tự hành (ADAS/Autonomous Driving) trong điều kiện đường phố hỗn hợp.
- **Rủi ro sai lệch nguy hiểm cần tránh:**
  - *Bỏ sót người (`person`):* Là lỗi nguy hiểm nhất (safety-critical), đặc biệt là người đi bộ sang đường (`img_014`), người đi bộ khuất sau xe (`img_001`) hoặc người điều khiển xe máy (`img_005`, `img_026`, `img_030`). Nếu mô hình bỏ sót, xe tự lái có thể không kích hoạt phanh tự động khẩn cấp, dẫn đến tai nạn chết người.
  - *Nhận diện nhầm vật thể giả thành người/xe thật:* Nếu mô hình nhầm tượng đài điêu khắc (`img_011`, `img_017`, `img_027`), ma-nơ-canh trong tủ kính (`img_033`), mô hình đồ chơi (`img_038`) hoặc hình ảnh chân dung in trên pano quảng cáo (`img_018`) thành đối tượng giao thông thực, hệ thống xe tự lái sẽ bị phanh gấp sai (phantom braking) hoặc báo động giả liên tục, gây nguy hiểm cho các xe chạy phía sau.

---

## 2. Lớp và thuộc tính

### 2.1. Đề xuất danh mục Lớp (Classes)

Khảo sát 40 ảnh cho thấy bản `v0` chỉ có 5 lớp sơ sài (`car`, `truck`, `van`, `motorcycle`, `person`) và 1 vùng `ignore_region`, hoàn toàn không xử lý được các phương tiện đặc trưng như xe 3 bánh (`img_004`, `img_016`, `img_035`) hay xe buýt lớn (`img_009`, `img_031`, `img_032`, `img_040`). Do đó, tôi đề xuất với nhóm danh mục lớp rõ ràng như sau:

| Lớp | Định nghĩa đề xuất (một câu) | Ví dụ cụ thể trong 40 ảnh | Phản ví dụ / Dễ nhầm lẫn |
|---|---|---|---|
| `car` | Ô tô con chở người từ 4–9 chỗ, thân xe liền khối (sedan, hatchback, SUV, MPV gia đình). | Ô tô con lưu thông hoặc đỗ trên đường (`img_002`, `img_015`, `img_021`, `img_040`). | Xe bán tải có thùng hàng hở, xe van chở hàng, ô tô đồ chơi trong tủ kính (`img_038`). |
| `truck` | Xe tải chở hàng, xe đầu kéo/rơ-moóc chuyên dụng, hoặc xe bán tải (pickup) có thùng chở hàng tách rời cabin. | Xe chở ô tô 2 tầng (`img_002`), xe tải giao hàng kho bãi (`img_007`), xe bán tải (`img_001`, `img_012`, `img_036`, `img_037`). | Xe van thân liền khối không tách thùng, tượng xe bán tải bằng đá (`img_027`). |
| `van` | Xe thùng kín cỡ nhỏ chở hàng hoặc chở khách thương mại có thân dáng hộp liền khối (minivan, panel van). | Xe van giao hàng, xe thùng kín thân liền (`img_008`, `img_031`). | Xe buýt lớn, xe bán tải cabin rời gắn thêm nắp thùng cao. |
| `bus` *(Đề xuất bổ sung)* | Xe buýt công cộng nội đô hoặc xe khách vận chuyển hành khách cỡ lớn. | Xe buýt đón trả khách, xe khách du lịch (`img_009`, `img_031`, `img_032`, `img_040`). | Xe van chở khách 16 chỗ dáng nhỏ. |
| `tuk_tuk` *(Đề xuất bổ sung)* | Xe cơ giới 3 bánh chở người hoặc hàng hóa (auto-rickshaw, tuk-tuk). | Xe tuk-tuk chở khách tại Thái Lan, Ấn Độ (`img_004`, `img_016`, `img_035`). | Xích lô đạp chân không có động cơ, xe máy 2 bánh. |
| `motorcycle` | Xe hai bánh có gắn động cơ cơ giới (xe mô tô, xe gắn máy, xe tay ga; kể cả mô tô gắn thùng phụ sidecar). | Dòng xe máy lưu thông trên đường phố (`img_005`, `img_008`, `img_022`, `img_026`, `img_030`). | Xe đạp không động cơ (`img_003`), xe tuk-tuk 3 bánh. |
| `person` | Con người thật bằng xương bằng thịt ở mọi tư thế (đang đi bộ, đứng, ngồi, đang điều khiển hoặc ngồi sau phương tiện). | Người đi bộ, người lái xe máy, người ngồi trên xích lô (`img_001`, `img_005`, `img_006`, `img_026`). | Tượng đồng/đá (`img_011`, `img_017`), ma-nơ-canh (`img_033`), búp bê (`img_038`), chân dung in trên biển quảng cáo (`img_018`). |
| `ignore_region` | Vùng bỏ qua: khoanh các đám đông người/phương tiện quá xa, dính thành cụm dày đặc không thể tách rời hoặc có chiều cao `< 20 px`. | Cụm người và xe ở xa cuối phố đêm hoặc bãi biển (`img_008`, `img_013`, `img_025`, `img_040`). | Không dùng để che lấp các vật thể lớn nhìn rõ ở tiền cảnh. |

> **Xử lý vật ngoài taxonomy (Quan điểm cá nhân Thành viên 104):**
> - Đối với phương tiện thô sơ không động cơ (xe đạp `img_003`, xích lô `img_006`, `img_009`, `img_023`, `img_034`, `img_039`): Tôi đề xuất **không gán phần xe đạp/xích lô**, nhưng **bắt buộc gán `person`** cho người đạp xe, người lái xích lô và hành khách ngồi trên xe.

---

### 2.2. Đề xuất Ngưỡng định lượng cho Thuộc tính (Attributes)

Bản `v0` dùng các từ định tính như "che ít", "che nhiều" khiến người gán không thể thống nhất. Tôi đề xuất ranh giới định lượng cụ thể:

| Thuộc tính | Kiểu | Các mức giá trị | Ngưỡng đo lường định lượng cụ thể |
|---|---|---|---|
| `occluded_level` | `select` | `0`, `1`, `2`, `3` | **`0`**: Hoàn toàn không bị che khuất (`0%` diện tích vật thể bị che).<br>**`1`**: Bị che khuất nhẹ (Từ `1%` đến `30%` diện tích tự nhiên của vật bị vật khác che).<br>**`2`**: Bị che khuất trung bình đến nhiều (Từ `31%` đến `70%` diện tích bị che).<br>**`3`**: Bị che khuất rất nặng (`> 70%` diện tích bị che nhưng các chi tiết đặc trưng còn lại vẫn đủ nhận dạng chắc chắn). |
| `truncated` | `select` | `no`, `yes` | **`no`**: Toàn bộ vật thể nằm trọn vẹn trong khung ảnh.<br>**`yes`**: Ít nhất một phần cấu trúc cơ thể/xe bị cắt đứt bởi cạnh viền bức ảnh (`x=0`, `y=0`, `x=width`, hoặc `y=height`). |
| `can_review` | `checkbox` | `true`, `false` | **`true`**: Đánh dấu tình huống biên người gán còn phân vân chưa chắc chắn, cần đưa ra nhóm xem xét ở buổi họp Pha 2 / Pha 5.<br>**`false`**: Đã tự tin gán theo quy tắc (mặc định). |

---

## 3. Quy tắc vẽ Bounding Box (Đề xuất giải quyết ca biên)

1. **Nguyên tắc ôm sát phần nhìn thấy (Visible Bounds):** 
   - Bounding box phải ôm sát các pixel thực tế nhìn thấy của vật thể. Tuyệt đối không tự suy diễn kéo rộng box bao phần bị vật cản che khuất hoặc phần ngoài mép ảnh.
2. **Quy tắc người và xe máy (`person` & `motorcycle`):**
   - Với người điều khiển hoặc ngồi sau xe máy (`img_005`, `img_026`, `img_030`), **phải vẽ 2 box độc lập**:
     - Một box `person` ôm toàn bộ phần cơ thể nhìn thấy của người.
     - Một box `motorcycle` ôm toàn bộ khung xe nhìn thấy (bánh xe, yếm xe, pô, gương, tay lái).
     - Hai box này sẽ có phần diện tích đè lên nhau. Chấm `occluded_level` phù hợp cho từng box dựa trên mức độ bị che khuất.
3. **Quy tắc xe chở xe & vật thể lồng nhau:**
   - Trường hợp xe chuyên dụng chở ô tô (`img_002.jpg`): Gán 1 box `truck` bao toàn bộ xe tải đầu kéo và sàn chở, đồng thời gán riêng từng box `car` cho mỗi ô tô con nhìn thấy rõ trên sàn tầng 1 và tầng 2.
4. **Quy tắc loại trừ các vật thể không có thật trên đường:**
   - **Tượng điêu khắc:** Tuyệt đối không gán `person` hay phương tiện cho tượng đài (`img_011`, `img_017`, `img_027`, tượng ngựa gỗ trên `img_028`).
   - **Ma-nơ-canh & Búp bê đồ chơi:** Không gán `person` cho ma-nơ-canh trong tủ kính (`img_033`) và không gán `car` cho ô tô đồ chơi (`img_038`).
   - **Hình ảnh in 2D:** Không gán chân dung người in trên biển quảng cáo tấm lớn (`img_018`).
   - **Ảnh phản chiếu qua gương:** Không gán cảnh vật/người phản chiếu qua gương cầu (`img_020`) và không gán phần gương của chính xe người chụp (ego vehicle).
   - *(Nếu ảnh chỉ có các đối tượng này và không có người/xe thật nào khác, ảnh đó sẽ có 0 box — là ảnh trống hợp lệ trong COCO).*
5. **Quy tắc hàng hóa cồng kềnh trên phương tiện:**
   - Box phương tiện (`tuk_tuk`, `motorcycle`, `truck`) ôm trọn kết cấu khung xe và thùng xe cố định. Với khối hàng hóa, bao tải chất cao vượt quá nóc hoặc hông xe (`img_035`, `img_029`, `img_012`), box xe không được kéo giãn bao trùm cả khối hàng mà chỉ ôm khung xe; nếu hàng hóa che khuất xe thì chấm `occluded_level` cao.
6. **Quy tắc vật quá nhỏ và đám đông:**
   - Vật đơn lẻ có chiều cao `< 20 px` không gán box cá thể. Nếu đứng thành cụm đông ở xa không thể tách bạch từng người/xe, vẽ một box `ignore_region` bao quanh cả cụm (`img_008`, `img_013`, `img_025`, `img_040`).

---

## 4. Ca khó và Tình huống biên khảo sát thực tế từ 40 ảnh

Dưới đây là 5 ca khó điển hình tôi đã khảo sát trực tiếp từ 40 ảnh mà bản `v0` không giải quyết được:

| Tên ảnh | Tình huống biên phát hiện | Lỗ hổng của v0 | Quyết định đề xuất của Thành viên 104 | Lập luận lý do |
|---|---|---|---|---|
| `img_002.jpg` | Xe tải chuyên dụng chở 8 ô tô con trên 2 tầng sàn xe. | v0 không nói rõ xe chở nhiều ô tô con thì gán 1 box to hay gán từng ô tô con. | Gán 1 box `truck` cho toàn bộ xe tải chở hàng, đồng thời gán 8 box `car` cho từng ô tô con trên sàn. | Mô hình camera giao thông cần nhận diện cả xe vận tải đang lưu thông lẫn các phương tiện ô tô đang được vận chuyển trên đường. |
| `img_017.jpg` | Tượng đồng Công tước Wellington cưỡi ngựa trên bệ đá cao. | v0 chỉ ghi `person: Người`, không hướng dẫn cách xử lý tượng điêu khắc nghệ thuật. | **Không gán box** (0 annotation cho ảnh này). | Đây là tác phẩm điêu khắc tĩnh, không phải người thật tham gia giao thông; không gán để tránh mô hình học sai gây phantom braking. |
| `img_035.jpg` | Xe tuk-tuk 3 bánh chở khối bao tải hàng hóa khổng lồ chất cao trên nóc. | v0 hoàn toàn thiếu lớp xe cơ giới 3 bánh và không hướng dẫn ranh giới box với hàng hóa cồng kềnh. | Đề xuất gán lớp `tuk_tuk`, box chỉ ôm sát khung và thân xe, tạm thời tick `can_review = true` để đưa ra thảo luận cùng nhóm. | Xe tuk-tuk có kích thước và động học riêng biệt cần lớp riêng; box chỉ ôm cấu trúc xe cơ giới cố định, không kéo trùm hàng hóa. |
| `img_037.jpg` | Xe bán tải pickup GO Transit gắn nắp thùng cao kín phía sau (`Master 75`). | v0 phân định giữa `truck` và `van` rất mập mờ, dễ nhầm xe này thành `van`. | Đề xuất gán lớp `truck`, không gán `van`. | Xe có kết cấu khung gầm và khoang cabin tách biệt hoàn toàn với thùng chở hàng; nắp thùng cao chỉ là phụ kiện lắp thêm. |
| `img_020.jpg` | Cầu vồng chụp phản chiếu qua gương chiếu hậu ô tô. | v0 không có quy tắc cho ảnh phản chiếu qua gương và bộ phận của xe chủ. | **Không gán box** (0 annotation cho ảnh này). | Hình ảnh phản chiếu và phần gương chiếu hậu thuộc xe chủ (ego vehicle), không phải chướng ngại vật thực trên mặt đường. |

---

## 5. QC và Lịch sử Phiên bản (Đề xuất cá nhân)

- **Ngưỡng kiểm soát chất lượng (QC) đề xuất áp dụng:**
  - Ngưỡng IoU box lớn (chiều cao ≥ 50 px): `≥ 0.85`.
  - Ngưỡng IoU box nhỏ (chiều cao < 50 px): `≥ 0.70`.
  - Độ đồng thuận giữa các người gán (IAA): IoU trung bình `≥ 0.85`, đồng thuận cùng lớp `≥ 95%`.
  - Tỉ lệ ảnh có lỗi tối đa toàn lô: `≤ 2.0%` (với 40 ảnh, chỉ cần 1 ảnh lỗi = 2.5% là phải Rework).
- **Cơ chế xử lý ca mơ hồ khi gán độc lập ở Pha 3:**
  - Nếu gặp ca khó chưa chắc chắn: Tick cờ `can_review = true` trên box hoặc ghi chép vào danh sách để đưa ra họp Calibration ở Pha 5.

### Lịch sử thay đổi

| Phiên bản | Tác giả | Thay đổi chính | Cơ sở / Lý do |
|---|---|---|---|
| `v0` | Giảng viên | Bản khởi đầu nhanh, sơ sài. | Chỉ có 5 lớp sơ sài, định nghĩa định tính "che ít/nhiều", thiếu quy tắc tuk-tuk, xe chở xe, tượng, phản chiếu, hàng hóa quá khổ. |
| `v0.1-draft` | Thành viên 104 | Bản đề xuất cá nhân độc lập tại mốc CP1 (Pha 1). | Đề xuất bổ sung lớp `bus`, `tuk_tuk`; định lượng hóa `occluded_level` (0/1/2/3); quy tắc tách người/xe máy, xe chở xe, loại trừ tượng/phản chiếu; khảo sát 5 ca khó điển hình trong 40 ảnh. |
