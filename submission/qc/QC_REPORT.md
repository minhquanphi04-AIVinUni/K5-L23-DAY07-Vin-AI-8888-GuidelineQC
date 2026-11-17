# Báo cáo QC — Vin-AI-8888

## Trạng thái bản nhãn hiện tại

- Mục đích và giới hạn sử dụng: Thực hành gán nhãn người và phương tiện 2D trên 40 ảnh đường phố; chưa đánh giá độ đại diện cho dữ liệu vận hành.
- Nguồn và phép sử dụng dữ liệu: Xem `data/ATTRIBUTION.csv`.
- Manifest và checksum nhãn cuối: `batch_manifest.json`; SHA-256 nhãn cuối `f67d25de319bf1e36e8528783a6dbad93bf737f532399fe94602a4dca0d7f354`.
- Guideline và taxonomy khi gán / khi bàn giao: Gán độc lập theo v1.0; bản bàn giao dùng guideline v1.1 đã đồng bộ tên lớp `ignore-region` và thuộc tính checkbox `true`/`false` với `taxonomy.json`.
- Lượt hoàn thành / được giao: 80/80 lượt gán độc lập.
- IAA trước calibration: IoU trung bình 0.827 và đồng thuận lớp 93.8% trên 276 cặp box ghép; 34/40 ảnh có ca cần calibration.

## Sửa sau vòng QC 1

Vòng 1 đã ghi Release cho bản nhãn SHA-256 `be44ce4567cf86ff2b578cb0f669a45aaeeab43db2d9f01735e09295bb486836`, nhưng phiếu đã đánh dấu sạch `img_025.jpg` dù còn 5 box dưới ngưỡng R-07. Đây là lỗi bị bỏ sót trong kết luận lịch sử; giữ nguyên hồ sơ vòng 1 để đối chiếu và không áp dụng Release đó cho bản nhãn mới.

Đối chiếu R-07 với nhãn cuối phát hiện 5 box `person` cao dưới 20 px ở `img_025.jpg`. Bản calibration mới chuyển một box riêng và cụm bốn box thành hai vùng `ignore-region`; chi tiết và tọa độ ở `corrections_after_round_01.md`. Nhãn hiện tại có 566 annotation, gồm 564 đối tượng và 2 vùng ignore mới. Reviewer 101 xác nhận đã kiểm bằng mắt đủ 40 ảnh vòng 2 và tất cả đều sạch; kết quả được ghi trong `reviews/round-02/qc_review.csv`.

- Vòng QC: 2
- Người review và ngày: 101; xác nhận kết quả duyệt ngày 2026-11-17.
- Đã duyệt / cả lô: 40/40 = 100.0% ở vòng 2.
- Ảnh lỗi / ảnh đã duyệt: 0/40 = 0.0%.
- Annotation đối tượng bị lỗi / annotation đã duyệt: 0/564 = 0.0%; 2 vùng `ignore-region` không tính trong mẫu số đối tượng.
- Vật bỏ sót, ảnh và vùng: 0 theo phiếu reviewer vòng 2.
- Ca khó / phần còn lại: 0/34 ảnh lỗi trong nhóm ca khó; 0/6 ảnh lỗi trong phần còn lại.
- Quyết định: Release
- Lý do, người nhận và phạm vi sử dụng: Reviewer 101 xác nhận đủ 40/40 ảnh của bản nhãn hiện tại, không ghi lỗi; tỉ lệ ảnh lỗi 0/40 dưới ngưỡng 2%. Bàn giao cho thực hành huấn luyện mô hình phát hiện người và phương tiện 2D trong giới hạn của tập ảnh tuyển chọn.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: `img_025.jpg`, R-07, bản trước có `person` #386 và #388–391 dưới 20 px; bản hiện tại có hai vùng `ignore-region` #386 và #388. Xem bản chụp vòng 1 và bản nhãn vòng 2 để đối chiếu.

Script kiểm cấu trúc COCO, checksum và ngưỡng kích thước; reviewer xác nhận việc đối chiếu box, lớp và vật có thể bị bỏ sót trên ảnh gốc/overlay.
