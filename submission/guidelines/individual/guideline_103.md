# Guideline gán nhãn — v0 (bản khởi đầu)

- Phiên bản: v0
- Ngày viết: 16/11/2026
- Người viết: 103
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
| `truck` | Xe tải |
| `van` | Xe thùng kín cỡ nhỏ. |
| `bus` | xe bus/ xe khách |
| `bycicle` | xe đạp |
| `motorcycle` | Xe máy, mô tô. |
| `tuktuk` | xe 3 bánh chở hàng hoặc chở người |
| `person` | Người đứng hoặc đi bộ, không phải ma-nơ-canh |
| `rider` | người ngồi trên xe 2 bánh/ 3 bánh |


### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | `0`/ `1` | Vật không bị che/ có bị che|
| `truncated` | `no`/ `yes` | Vật không bị cắt/ bị cắt ở mép ảnh. |
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
