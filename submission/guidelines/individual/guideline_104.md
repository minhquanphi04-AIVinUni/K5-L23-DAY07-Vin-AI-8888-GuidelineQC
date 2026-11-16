# Guideline gán nhãn — v0.1-draft — Thành viên 104

> Bản guideline cá nhân được soạn độc lập ở Pha 1 dựa trên việc khảo sát toàn bộ 40 ảnh trong `data/images/`. Bản này giải quyết triệt để các điểm mơ hồ của `guideline_v0_starter.md`: bổ sung đầy đủ các lớp phương tiện thực tế (`bus`, `tuk_tuk`, `bicycle`, `cyclo`) và lớp người điều khiển `rider`, chuẩn hóa thuộc tính `occluded` về dạng boolean (`true`/`false`) kèm ngưỡng định lượng đo được.

- Phiên bản: v0.1-draft (cá nhân)
- Ngày: 2026-11-16
- Người viết: Thành viên 104

---

## 1. Mục tiêu

Nhãn bounding box 2D được sử dụng để huấn luyện mô hình phát hiện vật thể (Object Detection) thời gian thực phục vụ hệ thống camera giám sát giao thông đô thị và hệ thống hỗ trợ lái xe an toàn (ADAS / xe tự lái).
- **Rủi ro sai lệch nguy hiểm:**
  - Bỏ sót (False Negative) người đi bộ, người đi xe máy, xe đạp, xích lô hoặc phương tiện chuyển động cắt ngang dẫn đến nguy cơ va chạm nghiêm trọng.
  - Nhận diện nhầm (False Positive) các vật thể tĩnh, vô tri như tượng đài, tranh ảnh biển quảng cáo, ma-nơ-canh hoặc hình phản chiếu qua gương chiếu hậu sẽ khiến xe tự lái phanh gấp vô căn cứ (phantom braking), gây nguy hiểm cho phương tiện phía sau.
  - Box vẽ quá rộng (bao trùm cả hàng hóa cồng kềnh ngoài quy chuẩn) hoặc quá lỏng lẻo làm sai lệch ước lượng khoảng cách không gian 3D của hệ thống điều khiển.

---

## 2. Lớp và thuộc tính

### 2.1. Phân loại lớp (Classes - 11 lớp)

Mỗi đối tượng hợp lệ chỉ thuộc đúng một lớp. Áp dụng nguyên tắc: *"Chỉ gán các thực thể sống hoặc phương tiện thực tế tham gia giao thông"*.

| Lớp | Định nghĩa (một câu) | Ví dụ thực tế trong dữ liệu | Phản ví dụ / Dễ nhầm lẫn |
|---|---|---|---|
| `car` | Xe ô tô con chở người từ 9 chỗ trở xuống (sedan, SUV, hatchback, coupe). | Ô tô đỗ ven đường (`img_001.jpg`), ô tô con chở trên sàn xe tải (`img_002.jpg`). | Tượng xe bán tải bằng đá (`img_027.jpg`), mô hình xe đồ chơi (`img_038.jpg`). |
| `truck` | Xe ô tô tải có kết cấu thùng chuyên dùng chở hàng hóa hoặc kéo rơ-moóc. | Xe tải chở ô tô 2 tầng Renault (`img_002.jpg`), xe tải nhỏ giao hàng (`img_007.jpg`, `img_036.jpg`). | Xe bán tải pickup tiêu chuẩn (xếp `car`), xích lô chở hàng (`cyclo`). |
| `van` | Xe ô tô thùng kín cỡ nhỏ đến trung bình liền khối, chở hàng nhẹ hoặc chở khách dưới 16 chỗ. | Xe van bưu chính / chở hàng nhẹ đô thị. | Xe buýt lớn (`img_032.jpg`), xe bán tải gắn nắp thùng cao (`img_037.jpg`). |
| `bus` | Xe ô tô chở khách công cộng cỡ lớn từ 16 chỗ trở lên (xe buýt đô thị, xe khách du lịch). | Xe buýt công cộng đón trả khách (`img_009.jpg`, `img_031.jpg`, `img_032.jpg`, `img_040.jpg`). | Xe van chở khách nhỏ 16 chỗ (`van`), xe tải thùng kín (`truck`). |
| `tuk_tuk` | Xe cơ giới 3 bánh chở khách hoặc chở hàng có mái che đặc thù đô thị/châu Á. | Hai xe tuk-tuk đỗ trên vỉa hè (`img_004.jpg`), auto rickshaw chở hàng (`img_035.jpg`). | Xích lô đạp chân (`cyclo`), xe mô tô 2 bánh (`motorcycle`). |
| `motorcycle` | Phương tiện giao thông cơ giới 2 bánh gắn động cơ (xe máy, xe mô tô, xe tay ga, xe gắn máy). | Xe máy di chuyển trong luồng giao thông (`img_005.jpg`, `img_008.jpg`, `img_026.jpg`). | Xe đạp không gắn động cơ (`bicycle`), xích lô (`cyclo`). |
| `bicycle` | Xe 2 bánh di chuyển hoàn toàn bằng sức người (đạp chân). | Người đi xe đạp trong dòng giao thông (`img_006.jpg`). | Xe máy / xe gắn máy chạy xăng/điện (`motorcycle`). |
| `cyclo` | Xe xích lô 3 bánh đạp chân bằng sức người, có khoang chở khách du lịch hoặc chở hàng phía trước. | Đoàn xích lô chở khách du lịch tại Huế (`img_009.jpg`, `img_023.jpg`, `img_034.jpg`, `img_039.jpg`). | Xe cơ giới 3 bánh gắn động cơ (`tuk_tuk`), xe đạp 2 bánh (`bicycle`). |
| `rider` | Người thật đang trực tiếp điều khiển phương tiện hai bánh khi tham gia giao thông, gồm người lái xe máy và người đạp xe đạp. | Người áo trắng lái xe máy ở giữa và người phụ nữ điều khiển xe đạp bên phải trong `img_006.jpg`. | Người ngồi sau, người đứng cạnh hoặc dắt xe dùng `person`; bản thân xe dùng `motorcycle` hoặc `bicycle`. |
| `person` | Người thật không thuộc lớp `rider`, gồm người đi bộ, đứng, ngồi, dắt xe, hành khách và người điều khiển phương tiện khác hai bánh. | Người áo đỏ đứng cạnh xe đạp ở tiền cảnh `img_003.jpg`; hành khách áo vàng chấm đen ngồi sau xe máy bên trái `img_006.jpg`. | Người trực tiếp điều khiển xe máy/xe đạp dùng `rider`; tượng, ảnh trên áp phích và ma-nơ-canh không gán. |
| `ignore_region` | Vùng tập trung đám đông hoặc cụm phương tiện ở khoảng cách quá xa, kích thước nhỏ hoặc bị che khuất không thể tách rời từng vật. | Đám đông khách dạo bờ biển ở cự ly xa (`img_025.jpg`), cụm xe mờ phía xa ban đêm (`img_013.jpg`). | Từng cá nhân hay từng phương tiện ở cự ly gần có thể tách rời rõ ràng. |

#### Xử lý vật thể ngoài phạm vi:
- Tượng đài nghệ thuật, tượng đá, ma-nơ-canh, poster in ấn, phản chiếu gương: **Tuyệt đối không gán**.

---

### 2.2. Thuộc tính (Attributes) và ngưỡng định lượng đo lường

| Thuộc tính | Kiểu nhập | Giá trị | Ngưỡng định lượng cụ thể & Ý nghĩa |
|---|---|---|---|
| `occluded` | `checkbox` | `false` (không tick) | **Không bị che khuất (0% diện tích):** Toàn bộ đường bao nhìn thấy của vật thể quan sát được trọn vẹn, không bị chướng ngại vật nào che chắn. Mặc định là `false`. |
| | | `true` (tick chọn) | **Bị che khuất ($\ge 1\%$ diện tích):** Bị che khuất một phần hoặc phần lớn diện tích nhìn thấy bởi người khác, xe khác, chướng ngại vật hoặc kết cấu công trình, nhưng người gán vẫn nhận diện chắc chắn được vật thể. |
| `truncated` | `select` | `no` | Vật thể nằm trọn vẹn bên trong ranh giới 4 cạnh của khung hình ảnh. Mặc định là `no`. |
| | | `yes` | Vật thể bị ít nhất một cạnh mép ảnh cắt ngang qua thân thể / thân xe (dù chỉ cắt 1 pixel). |
| `can_review` | `checkbox` | `false` (không tick) / `true` (tick chọn) | Bật (`true`) khi annotator cảm thấy không chắc chắn về lớp, ranh giới box hoặc rơi vào tình huống biên chưa có trong quy tắc, cần đưa ra nhóm hiệu chuẩn (Calibration). Mặc định là `false`. |

---

## 3. Quy tắc vẽ Bounding Box

Mỗi bounding box là một hình chữ nhật bao quanh vật thể theo các nguyên tắc sau:

1. **Ranh giới ôm sát phần nhìn thấy (Tight Visible Boundary):**
   - Box phải ôm sát ranh giới các pixel nhìn thấy được của vật thể (visible bounding box). Không vẽ ước lượng vùng bị che khuất ngầm phía sau chướng ngại vật.
2. **R-RIDER — Tách người khỏi phương tiện và phân biệt vai trò:**
   - **Xe máy / xe đạp:** Vẽ 1 box `motorcycle` hoặc `bicycle` ôm sát phần xe nhìn thấy, không mở rộng box để bao cả người. Vẽ thêm 1 box `rider` riêng ôm sát phần người điều khiển nhìn thấy, gồm đầu/mũ bảo hiểm, thân và các phần tay chân nhìn thấy. Không suy đoán phần bị che.
   - **Không gán trùng người:** Một người chỉ nhận `rider` hoặc `person`. `rider` là nhãn của người, không phải nhãn cho tổ hợp người + xe. Hai box người và xe có thể giao nhau do tư thế hoặc che khuất.
   - **Hành khách:** Người ngồi sau xe máy/xe đạp dùng `person`, không dùng `rider`. Xe máy chở hai người thường có 3 box: 1 `motorcycle`, 1 `rider` cho người lái và 1 `person` cho hành khách, nếu từng đối tượng đủ điều kiện gán.
   - **Đứng cạnh / dắt xe:** Người đi bộ dắt xe hoặc đứng cạnh xe dùng `person`; xe vẫn được gán riêng. Không suy ra `rider` chỉ từ việc người chạm tay vào ghi đông, đứng sát xe hoặc đội mũ bảo hiểm.
   - **Dừng chờ trong luồng giao thông:** Người vẫn ngồi ở vị trí lái và có tư thế điều khiển dùng `rider`, kể cả đang chống chân hoặc dừng chờ. Không cần suy đoán xe có chuyển động từ một ảnh tĩnh.
   - **Ca chưa rõ (`img_003.jpg`):** Người áo đỏ đứng cạnh xe đạp ở tiền cảnh dùng `person` và xe dùng `bicycle`. Người áo trắng quay lưng ngồi trên xe máy giữa ảnh chỉ dùng `rider` khi xác định được vị trí/tư thế điều khiển. Nếu chưa phân biệt được người lái, người ngồi sau hoặc người đang dắt xe, bật `can_review` và ghi lý do; không tự coi mọi người cạnh xe là `rider`.
   - **Ví dụ rõ (`img_006.jpg`):** Người áo trắng lái xe máy ở giữa và người phụ nữ điều khiển xe đạp bên phải dùng `rider`; hành khách áo vàng chấm đen bên trái dùng `person`. Mỗi phương tiện có box riêng chỉ bao phần xe.
   - **Xích lô (`cyclo`):** Vẽ box xe chỉ bao phần xe nhìn thấy; vẽ box `person` riêng cho người đạp xích lô và từng hành khách. Xích lô có ba bánh nên người đạp không thuộc định nghĩa `rider` của bản này.
   - **Tuk-tuk (`tuk_tuk`):** Vẽ box ôm sát thân xe. Tài xế và hành khách nhìn thấy rõ trên 50% thân người được gán `person`, không dùng `rider`.
3. **Xe chuyên dụng chở phương tiện khác (`img_002.jpg` - Car Carrier):**
   - Vẽ 1 box lớn nhãn `truck` bao toàn bộ đầu kéo và sàn rơ-moóc chở hàng.
   - Vẽ từng box `car` riêng biệt cho từng chiếc ô tô con được chở trên sàn xe (đặt `occluded = true` nếu bị giàn khung xe che chắn).
4. **Hàng hóa cồng kềnh trên phương tiện (`img_029.jpg`, `img_035.jpg`):**
   - Box phương tiện (`motorcycle`, `truck`, `tuk_tuk`, `cyclo`) chỉ ôm sát kết cấu khung thân cơ giới của phương tiện và giá chở hàng tiêu chuẩn.
   - **Không** kéo box mở rộng bao trọn các bao tải, kiện hàng chất cao ngất ngưởng vượt quá kích thước xe quá 50 cm.
5. **Vật thể không phải thực thể sống / thực thể giao thông:**
   - Tượng đá, tượng đồng nghệ thuật (`img_011.jpg`, `img_017.jpg`, `img_027.jpg`): **Tuyệt đối không gán**.
   - Ma-nơ-canh thời trang, búp bê, thú nhồi bông (`img_033.jpg`, `img_038.jpg`): **Tuyệt đối không gán**.
   - Ảnh người in trên áp phích, biển quảng cáo, bảng tranh cổ động (`img_018.jpg`): **Không gán**.
   - Hình ảnh phản chiếu của xe/người qua gương chiếu hậu (`img_020.jpg`) hoặc qua vũng nước mưa: **Không gán**.
6. **Vật thể quá nhỏ và Vùng bỏ qua (`ignore_region`):**
   - Bất kỳ đối tượng nào có chiều cao hiển thị nhỏ hơn **20 pixel** trên ảnh: Không vẽ box đơn lẻ.
   - Nếu có cụm người/xe tập trung dày đặc ở cự ly xa: Vẽ 1 box `ignore_region` bao quanh toàn bộ khu vực đó.
7. **R-MOTION-BLUR — Nhiễu thời tiết và nhòe chuyển động (`img_014.jpg`, `img_015.jpg`):**
   - Nhòe chuyển động không phải lý do tự động loại bỏ đối tượng. Chỉ gán box riêng khi vẫn xác định được lớp và ranh giới phần đối tượng nhìn thấy; không suy đoán hình dạng từ toàn bộ vệt nhòe.
   - Người trực tiếp điều khiển xe máy/xe đạp dùng `rider`; phương tiện dùng `motorcycle`/`bicycle` với box riêng theo R-RIDER. Người ngồi sau hoặc người đi bộ dùng `person`.
   - Box ôm sát phần còn nhận diện được của từng đối tượng, không mở rộng để bao toàn bộ vệt kéo dài do chuyển động. Nếu vẫn tách được đối tượng nhưng ranh giới box chưa chắc chắn, bật `can_review` và ghi lý do cần kiểm lại.
   - Vùng người/xe nhòe quá mức, không xác định được lớp hoặc không thể tách từng đối tượng: dùng `ignore_region` bao vùng không thể gán tin cậy; không đoán box riêng. Không để vùng ignore bao trùm các đối tượng rõ có thể gán riêng.
   - **Áp dụng `img_014.jpg`:** Hai người đứng rõ ở tiền cảnh dùng `person`. Với người lái và xe máy bị nhòe phía sau, xét từng đối tượng theo các tiêu chí trên: gán `rider` và `motorcycle` riêng khi còn nhận diện được; vùng không thể tách dùng `ignore_region`. Không kết luận tốc độ xe chỉ từ một ảnh tĩnh.
   - Vệt sáng đèn pha phản chiếu trên mặt đường ướt mưa không tính vào ranh giới xe và không gán thành đối tượng riêng.

---

## 4. Bảng phân tích các ca khó phát hiện trong 40 ảnh

| ID Ảnh | Tình huống cụ thể | Mã quy tắc | Quyết định cá nhân | Lý do & Lập luận kỹ thuật |
|---|---|---|---|---|
| `img_002.jpg` | Xe tải Renault 380 chở 8 ô tô con trên hai tầng sàn xe. | R-CARRIER | Gán 1 box `truck` cho xe tải; gán 8 box `car` riêng cho 8 ô tô trên sàn (`occluded=true`). | Xe tải chở hàng là 1 phương tiện, nhưng 8 ô tô con trên sàn vẫn là mục tiêu phát hiện chướng ngại vật vật lý độc lập. |
| `img_004.jpg` | Hai xe tuk-tuk 3 bánh đỗ trên vỉa hè ban đêm. | R-TUKTUK | Gán nhãn `tuk_tuk` cho từng xe. | Xe cơ giới 3 bánh đặc thù đã có lớp `tuk_tuk` riêng, không gán nhầm vào `car` hay `van`. |
| `img_003.jpg` | Người đứng cạnh xe đạp ở tiền cảnh; người ngồi trên xe máy giữa ảnh. | R-RIDER | Người áo đỏ đứng cạnh xe: `person` + `bicycle`. Người ngồi trên xe máy: xét vị trí/tư thế lái; chưa rõ vai trò thì bật `can_review`. | Chạm vào xe hoặc đứng gần xe không đủ để kết luận là người điều khiển. |
| `img_006.jpg` | Xe máy và xe đạp cùng tham gia giao thông, có cả người lái và người ngồi sau. | R-RIDER | Người điều khiển: `rider`; hành khách: `person`; từng xe: `motorcycle` hoặc `bicycle`. Box người và xe tách riêng. | Phân biệt vai trò người với loại phương tiện, không gán cả người và xe thành một box. |
| `img_009.jpg` | Xe buýt lớn chạy song song với đoàn xích lô du lịch. | R-BUS | Gán 1 box `bus` cho xe buýt, các box `cyclo` cho xích lô. | Phân định rõ giữa xe khách cỡ lớn (`bus`) và phương tiện 3 bánh đạp chân (`cyclo`). |
| `img_014.jpg` | Người và xe máy bị nhòe chuyển động phía sau hai người đứng rõ ở tiền cảnh. | R-MOTION-BLUR, R-RIDER | Hai người tiền cảnh: `person`. Người lái/xe còn nhận diện được: box `rider`/`motorcycle` riêng, bật `can_review` nếu ranh giới chưa chắc. Vùng không thể tách: `ignore_region`. | Không bỏ đối tượng chỉ vì nhòe; không đoán box theo toàn bộ vệt chuyển động hoặc để vùng ignore che đối tượng rõ. |
| `img_017.jpg` | Tượng công tước Wellington cưỡi ngựa bằng đồng có đội chóp nón giao thông. | R-STATUE | **Không gán box** `person`. | Tượng điêu khắc cố định, không phải con người thật. Tránh gây phanh ma (phantom braking). |
| `img_018.jpg` | Biển quảng cáo ngoài trời khổ lớn in hình chân dung chính trị gia. | R-BILLBOARD | **Không gán box** `person`. | Ảnh 2D in trên mặt phẳng, không phải người thật chiếm không gian vật lý 3D. |
| `img_020.jpg` | Cầu vồng và cảnh vật phản chiếu qua gương chiếu hậu ô tô. | R-REFLECTION | **Không gán box**. | Ảnh ảo phản chiếu qua gương phẳng, không phải vật thể nằm trước mặt camera. |
| `img_023.jpg` | Xích lô di chuyển phía trước góc nhìn đường phố Huế. | R-CYCLO | Gán box `cyclo` chỉ ôm phần xe; gán box `person` cho người đạp xích lô. | Đảm bảo tính nhất quán của quy tắc gán kép cho xích lô. |
| `img_027.jpg` | Bia mộ bằng đá granite tạc hình chiếc xe bán tải (Oak Hill Memorial Park). | R-MONUMENT | **Không gán box** `car` hay `truck`. | Tượng đài bằng đá tĩnh ở nghĩa trang, không có bánh xe cơ học hay khả năng lưu thông. |
| `img_032.jpg` | Trạm chờ xe buýt giờ cao điểm với xe buýt đô thị ETS và người đi bộ. | R-BUS | Gán box `bus` cho xe buýt, gán box `person` cho người chờ xe. | Nhận diện rõ xe vận tải hành khách khối lượng lớn. |
| `img_033.jpg` | Ma-nơ-canh mặc quần áo đứng trong tủ kính cửa hàng Buchanan Street. | R-MANNEQUIN | **Không gán box** `person`. | Ma-nơ-canh bằng nhựa/sáp trong nhà, không phải người đi bộ. |
| `img_035.jpg` | Xe tuk-tuk (auto rickshaw) chở bao tải hàng hóa phình to gấp 3 lần xe. | R-OVERLOAD | Gán box `tuk_tuk` chỉ ôm thân xe; không kéo box trùm cả khối bao tải trên nóc. | Box dự báo vị trí phương tiện để căn lề và tính toán quỹ đạo, kéo quá to sẽ làm sai lệch tâm vật thể (centroid). |
| `img_037.jpg` | Xe bán tải pickup GO Transit lắp nắp thùng cao 'Master 75' trông như xe van. | R-PICKUP-CAP | Gán nhãn `truck`, tick `can_review` để nhóm chốt. | Phụ kiện thùng cao khiến ranh giới giữa `van`, `truck` và `car` bị nhòe. |
| `img_038.jpg` | Tủ kính trưng bày bộ sưu tập đồ chơi, búp bê và mô hình xe hơi thu nhỏ. | R-TOY | **Không gán box** nào trong tủ kính. | Toàn bộ là mô hình đồ chơi thu nhỏ (scale models), không phải thực thể giao thông. |

---

## 5. QC và Quy trình quản lý phiên bản

### 5.1. Ngưỡng kiểm định chất lượng (QC Targets)
- **Tỉ lệ bao phủ:** 100% ảnh được giao phải được gán nhãn đầy đủ; ảnh không có vật thể vẫn phải xuất hiện trong file kết quả COCO (0 annotation).
- **Mục tiêu IAA khi so sánh chéo:**
  - IoU trung bình giữa các box khớp nhau: $\ge 0.85$.
  - Tỉ lệ thống nhất nhãn lớp: $\ge 95\%$.
- **Tỉ lệ lỗi tối đa cho phép:** Tỉ lệ ảnh có lỗi khi reviewer kiểm tra ngẫu nhiên phải $\le 2\%$.

### 5.2. Lịch sử thay đổi

| Phiên bản | Nội dung thay đổi | Cơ sở / Lý do |
|---|---|---|
| `v0` | Bản khởi đầu sơ sài của ban tổ chức. | Chỉ có 5 lớp thô, định nghĩa che khuất định tính, chưa có quy tắc cho các ca khó. |
| `v0.1-draft` | Bản cá nhân độc lập của Thành viên 104 (đã cập nhật). | - Bổ sung 4 lớp phương tiện thực tế: `bus`, `tuk_tuk`, `bicycle`, `cyclo` (cùng `rider`, tổng 11 lớp).<br>- Chuẩn hóa thuộc tính `occluded` về dạng boolean (`false` / `true`) kèm ngưỡng đo lường định lượng ($\ge 1\%$).<br>- Bổ sung R-RIDER: tách box người và xe; phân biệt người điều khiển hai bánh (`rider`), hành khách và người dắt xe (`person`), với ví dụ `img_003.jpg` và `img_006.jpg`.<br>- Giữ nguyên các quy tắc loại trừ tượng đài, ma-nơ-canh, đồ chơi, hình phản chiếu gương. |
