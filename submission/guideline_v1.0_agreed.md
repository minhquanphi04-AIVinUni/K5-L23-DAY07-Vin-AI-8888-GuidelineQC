# Guideline gán nhãn — v1.0
- Phiên bản: v1.0
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
| `car` | Ô tô con, xe bán tải, 2 đến 9 chỗ, thường chở người và khoang xe thông nhau, khong bị ngăn cách. |
| `truck` | Xe tải, chở hàng, có khu vực lái và khu vực chở đồ đươc tách riêng, trọng tải có khối lượng chuyên chở từ 1500kg trở lên. |
| `van` | Xe thùng kín cỡ nhỏ, khoang xe liền, kín, buồng lái và buồng chở hàng/người được tách riêng nhưng kín và chung một khối trong khoang xe. |
| `motorcycle` | Xe hai bánh có động cơ. |
| `bus` | Xe buýt. |
| `tuk_tuk` | Xe máy ba bánh chở người hoặc hàng. |
| `bicycle` | Xe đạp, xe hai bánh không có động cơ. |
| `person` | Người. |
| `ignore-region` | Những vật không ảnh hưởng đến chủ thể và không cần bận tâm. |

### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | tick / không (`true` / `false`) | Tick khi vật bị che; không tick khi thấy rõ. |
| `truncated` | tick / không (`true` / `false`) | Tick khi vật bị cắt ở mép ảnh. |
| `can_review` | tick / không (`true` / `false`) | Tick khi bạn không chắc, để người khác xem lại. |

## 3. Quy tắc vẽ box

1. Gán box cho mọi người và phương tiện trong ảnh.
2. Box ôm sát vật được định nghĩa (Không gồm hành lý).
3. Chọn đúng lớp và thuộc tính.
4. Không gán cho tượng, hình ảnh, mô hình,..

## 4. Ca khó

(chưa có)

## 5. QC và phiên bản

- Mọi thay đổi làm đổi nhãn đều tăng phiên bản guideline.
- Ca mơ hồ: tick `can_review`.
