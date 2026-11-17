# Thông tin nhóm

- Tên nhóm: Vin_AI_8888
- Mã thành viên (ba chữ số): 101, 102, 103, 104
- Người điều phối: Chưa xác nhận trong hồ sơ nhóm.
- Người review bản cuối: 101 (vòng QC 2); 102 đã duyệt vòng 1 lịch sử.
- Commit chốt bài: Xem HEAD của `main` sau khi đồng bộ.

## Kết quả tóm tắt (điền cuối bài)

- Lượt gán hoàn tất / được giao: 80/80.
- IAA trước calibration (kèm mẫu số): IoU trung bình 0.827; đồng thuận lớp 93.8% trên 276 cặp box ghép; chưa đạt ngưỡng IAA.
- Số ảnh phải calibration: 34/40.
- Vòng QC và số ảnh đã duyệt / cả lô: vòng 2, 40/40; vòng 1 lịch sử cũng đã duyệt 40/40 nhưng bỏ sót lỗi R-07 ở `img_025.jpg`.
- Tỉ lệ lỗi vòng 2: 0.0% theo ảnh (0/40), 0.0% theo đối tượng (0/564); 2 vùng ignore không tính trong mẫu số đối tượng.
- **Quyết định cho bản nhãn hiện tại:** Release theo xác nhận của reviewer 101 ở vòng 2; kết luận vòng 1 chỉ là bằng chứng lịch sử.
