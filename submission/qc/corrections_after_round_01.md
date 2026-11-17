# Điều chỉnh sau QC vòng 1

- Ảnh: `img_025.jpg`.
- Quy tắc: R-07 của guideline v1.1; đối tượng cao dưới 20 px không giữ box `person` riêng.
- Bản trước: `reviews/round-01/labels_snapshot.json`, SHA-256 `be44ce4567cf86ff2b578cb0f669a45aaeeab43db2d9f01735e09295bb486836`.
- Nguồn calibration mới: `labels_calibrated_8ccc426efec52f47.json`.
- Bản nhãn cuối mới: `labels_calibrated.json`, SHA-256 `f67d25de319bf1e36e8528783a6dbad93bf737f532399fe94602a4dca0d7f354`.

Phiếu `reviews/round-01/qc_review.csv` đã ghi `khong` cho `img_025.jpg` dưới mã reviewer 102. Đối chiếu sau vòng 1 mới phát hiện 5 box dưới ngưỡng R-07; vì vậy kết luận Release vòng 1 đã bỏ sót lỗi này. Giữ nguyên phiếu và snapshot vòng 1 để bảo toàn lịch sử, không dùng kết luận đó cho bản nhãn mới.

| Box `person` trước | Chiều cao trước (px) | Xử lý trong nguồn calibration mới |
|---|---:|---|
| #386 | 19.89 | Đổi thành vùng `ignore-region` `[348.00, 371.62, 12.75, 19.89]`. |
| #388 | 17.35 | Gộp với #389–391 thành vùng `ignore-region` `[567.12, 352.18, 19.50, 17.35]`. |
| #389 | 15.74 | Gộp vào vùng #388. |
| #390 | 14.32 | Gộp vào vùng #388. |
| #391 | 15.89 | Gộp vào vùng #388. |

Hai vùng mới lấy biên hợp từ các box cũ. Reviewer 101 xác nhận đã kiểm đủ 40 ảnh của vòng 2 và tất cả đều sạch; phiếu, bản chụp và báo cáo tự động nằm trong `reviews/round-02/`.
