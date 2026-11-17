# Báo cáo QC — Vin-AI-8888

## Trạng thái bản nhãn hiện tại

- Mục đích và giới hạn sử dụng: Thực hành gán nhãn người và phương tiện 2D trên 40 ảnh đường phố; chưa đánh giá độ đại diện cho dữ liệu vận hành.
- Nguồn và phép sử dụng dữ liệu: Xem `data/ATTRIBUTION.csv`.
- Manifest và checksum nhãn cuối: `batch_manifest.json`; SHA-256 nhãn cuối `f67d25de319bf1e36e8528783a6dbad93bf737f532399fe94602a4dca0d7f354`.
- Guideline và taxonomy khi gán / khi bàn giao: Gán độc lập theo v1.0; bản bàn giao dùng guideline v1.1 đã đồng bộ tên lớp `ignore-region` và thuộc tính checkbox `true`/`false` với `taxonomy.json`.
- Lượt hoàn thành / được giao: 80/80 lượt gán độc lập.
- IAA trước calibration: IoU trung bình 0.827 và đồng thuận lớp 93.8% trên 276 cặp box ghép; 34/40 ảnh có ca cần calibration.

## Sửa sau vòng QC 1

Vòng 1 đã duyệt 40/40 ảnh và ghi Release cho bản nhãn SHA-256 `be44ce4567cf86ff2b578cb0f669a45aaeeab43db2d9f01735e09295bb486836`. Kết quả đó chỉ áp dụng cho bản nhãn được lưu tại `reviews/round-01/`.

Đối chiếu R-07 với nhãn cuối phát hiện 5 box `person` cao dưới 20 px ở `img_025.jpg`. Bản calibration mới chuyển một box riêng và cụm bốn box thành hai vùng `ignore-region`; chi tiết và tọa độ ở `corrections_after_round_01.md`. Nhãn hiện tại có 566 annotation, gồm 2 vùng ignore mới. Vòng 2 đã được tạo cho 40/40 ảnh nhưng phiếu `reviews/round-02/qc_review.csv` chưa được reviewer điền.

- Vòng QC: 2
- Người review và ngày: Chưa có xác nhận của reviewer cho vòng 2; ngày duyệt chưa ghi.
- Đã duyệt / cả lô: 0/40 ở vòng 2; phiếu đã chuẩn bị cho 40/40 ảnh.
- Ảnh lỗi / ảnh đã duyệt: Chưa tính cho vòng 2 vì chưa có ảnh được xác nhận.
- Annotation đối tượng bị lỗi / annotation đã duyệt: Chưa tính cho vòng 2 vì chưa có annotation được xác nhận.
- Quyết định: Hold
- Lý do, người nhận và phạm vi sử dụng: Chờ người review kiểm bản nhãn mới và điền phiếu vòng 2; chưa bàn giao bản này dưới kết luận Release.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: `img_025.jpg`, R-07, bản trước có `person` #386 và #388–391 dưới 20 px; bản hiện tại có hai vùng `ignore-region` #386 và #388. Xem bản chụp vòng 1 và bản nhãn vòng 2 để đối chiếu.

Script kiểm cấu trúc COCO, checksum và ngưỡng kích thước; người review cần kiểm box ôm đúng vùng, lớp đúng thực tế và vật có bị bỏ sót không.
