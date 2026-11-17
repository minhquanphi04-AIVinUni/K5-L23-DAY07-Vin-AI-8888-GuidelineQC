# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 4
- Lượt gán đủ **80/80**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 276 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 40 | 276 | 0.827 | 93.8% | 60.9% | 92.4% | 72/221 | 0 | CHƯA ĐẠT |
| 101|102 | 6 | 27 | 0.849 | 96.3% | 77.8% | 100.0% | 8/2 | 0 | CHƯA ĐẠT |
| 101|103 | 9 | 91 | 0.825 | 96.7% | 62.6% | 90.1% | 46/22 | 0 | CHƯA ĐẠT |
| 101|104 | 5 | 52 | 0.807 | 96.2% | 46.2% | 86.5% | 11/27 | 0 | CHƯA ĐẠT |
| 102|103 | 5 | 25 | 0.819 | 76.0% | 68.0% | 96.0% | 3/26 | 0 | CHƯA ĐẠT |
| 102|104 | 9 | 41 | 0.833 | 90.2% | 63.4% | 95.1% | 2/84 | 0 | CHƯA ĐẠT |
| 103|104 | 6 | 40 | 0.839 | 97.5% | 57.5% | 95.0% | 2/60 | 0 | CHƯA ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 557 ({'bo_sot_thua': 217, 'iou_thap': 119, 'sai_lop': 17, 'sai_thuoc_tinh': 118, 'bat_dong_ignore': 84, 'co_can_review': 2}), trên 34 ảnh cần calibration.
