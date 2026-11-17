# Guideline gán nhãn — v1.1 (Calibrated)
- Phiên bản: v1.1
- Loại nhãn: bounding box (hộp chữ nhật) trên ảnh đường phố
- Công cụ: CVAT, xuất COCO 1.0

---

## 1. Mục tiêu

Nhãn dùng để huấn luyện mô hình phát hiện **người và phương tiện** trên ảnh đường phố phục vụ hệ thống camera giao thông và xe tự lái.

## 2. Lớp và thuộc tính

### Lớp đối tượng

| Lớp | Định nghĩa |
|---|---|
| `car` | Ô tô con, xe bán tải, SUV từ 2 đến 9 chỗ. |
| `truck` | Xe tải chở hàng, xe đầu kéo chuyên dụng có cabin và thùng riêng biệt. |
| `van` | Xe thùng kín cỡ nhỏ, buồng lái và khoang chở hàng chung một khối kín. |
| `motorcycle` | Xe hai bánh có gắn động cơ (xe máy, mô tô, xe gắn máy). |
| `bus` | Xe buýt chở khách công cộng cỡ trung và lớn. |
| `tuk_tuk` | Xe ba bánh có gắn động cơ chở người hoặc hàng. |
| `bicycle` | Xe đạp hai bánh không có động cơ. |
| `person` | Người đi bộ, người đang điều khiển phương tiện hoặc người ngồi trên xe. |
| `ignore_region` | Vùng đám đông dày đặc, cụm đối tượng quá xa hoặc vật nhỏ không xác định được biên. |

### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | `0` (thấy rõ), `1` (bị che) | Đánh dấu `1` khi vật bị che khuất một phần bởi vật thể khác. |
| `truncated` | `no` (không), `yes` (có) | Chọn `yes` khi vật bị cắt ngang bởi mép khung hình ảnh. |
| `can_review` | `true` / `false` | Đánh dấu `true` khi cần reviewer kiểm tra lại trong phiên calibration. |

---

## 3. Quy tắc trọng tâm (Rules Code)

- **`R-01` (Ôm sát phần nhìn thấy):** Bounding box phải ôm sát tối đa phần cơ thể người hoặc thân xe nhìn thấy được. Không kéo box bao trùm bóng đổ trên mặt đường, không bao quanh vệt nhòe do chuyển động tốc độ cao, và không bao quanh khối hàng hóa cồng kềnh chất rời ngoài giá đỡ chuẩn.
- **`R-02` (Xe chuyên chở phương tiện):** Đối với xe chuyên dụng chở ô tô/xe máy, bản thân xe vận chuyển được gán là `truck`. Toàn bộ các ô tô con/xe máy chở trên sàn xe nếu nhìn rõ đều phải được gán nhãn riêng biệt từng chiếc (`car`/`motorcycle`) và gắn cờ `occluded_level=1`.
- **`R-03` (Người điều khiển phương tiện):** Người ngồi trên xe máy, xe đạp hoặc tuk-tuk phải được gán một box riêng là `person`, tách biệt với box của phương tiện (`motorcycle`, `bicycle`, `tuk_tuk`).
- **`R-04` (Xác định che khuất):** Gán `occluded_level=1` khi có từ 10% đến 80% diện tích nhìn thấy của vật bị che bởi vật cản khác (cột đèn, cây cối, xe khác, người khác). Nếu thấy rõ toàn bộ thân vật thì gán `occluded_level=0`.
- **`R-05` (Xác định cắt mép):** Gán `truncated=yes` khi bất kỳ cạnh nào của đối tượng bị cắt cụt do chạm mép ảnh (biên trên, dưới, trái, phải).
- **`R-06` (Đối tượng ngoài phạm vi):** Tuyệt đối không gán nhãn cho tượng điêu khắc nghệ thuật, hình ảnh quảng cáo 2D, tranh vẽ, ma-nơ-canh trong tủ kính, hình ảnh phản chiếu qua gương/cửa kính, hoặc mô hình xe đồ chơi thu nhỏ.
- **`R-07` (Ngưỡng kích thước tối thiểu và ignore_region):** Các đối tượng có chiều cao nhỏ hơn 20 pixel hoặc nằm trong cụm đám đông quá xa không tách rời được từng người/xe thì không gán riêng rẽ mà gom chung vào một vùng `ignore_region`.

---

## 4. Lịch sử thay đổi (Changelog)

| Phiên bản | Nội dung thay đổi | Phân loại |
|---|---|---|
| `v0` | Khởi tạo ban đầu với các định nghĩa cơ bản. | Khởi tạo |
| `v1.0` | Thống nhất 8 lớp chính, bổ sung `bus`, `tuk_tuk`, `bicycle` và chuẩn hóa thuộc tính. | Thống nhất |
| `v1.1` | Chuẩn hóa bộ mã quy tắc `R-01` đến `R-07`, làm rõ quy tắc xe chở xe (`R-02`), phân tách người ngồi trên xe (`R-03`), định nghĩa loại trừ tượng/mô hình (`R-06`) và ngưỡng kích thước nhỏ (`R-07`). | Hiệu chuẩn (Calibration) |
