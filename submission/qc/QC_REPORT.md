# Báo cáo QC — Vin-AI-8888

Báo cáo đảm bảo chất lượng nhãn tập dữ liệu phát hiện người và phương tiện (Day 07).

## 1. Mục đích, dữ liệu và phiên bản

- Mục đích và giới hạn sử dụng: Huấn luyện mô hình phát hiện người và phương tiện 2D trên ảnh đường phố; dữ liệu tuyển chọn phục vụ chuẩn hóa quy trình, chưa đại diện cho mọi điều kiện thời tiết khắc nghiệt.
- Nguồn và phép sử dụng dữ liệu: Dữ liệu ảnh đường phố công khai tuân thủ giấy phép CC BY-SA và CC0 ghi nhận tại data/ATTRIBUTION.csv.
- Manifest và checksum nhãn cuối: batch_manifest.json với SHA-256 nhãn cuối: be44ce4567cf86ff2b578cb0f669a45aaeeab43db2d9f01735e09295bb486836
- Guideline và taxonomy khi gán / khi bàn giao: Khi gán dùng v1.0; khi bàn giao dùng v1.1 bổ sung quy tắc R-01 đến R-07, giữ nguyên 8 lớp và cấu trúc thuộc tính taxonomy.

## 2. Phân công và đồng thuận trước calibration

- Lượt hoàn thành / được giao: 80/80 (100.0% lượt hoàn thành).
- Số cặp box ghép, IoU TB, đồng thuận lớp và mẫu số: 276 cặp box ghép (IoU >= 0.5), IoU trung bình 0.827, đồng thuận lớp 93.8% trên tổng số 276 cặp.
- Box chưa ghép, loại do ignore, bất đồng vùng ignore: 72 box thừa bên A, 221 box thừa bên B; 84 ca bất đồng liên quan đến vùng ignore_region.
- Ca hai người có thể cùng sai sau khi đối chiếu tham chiếu: img_002.jpg (xe chở xe): hai người cùng gán sót các ô tô con trên sàn xe chở hàng do chưa áp dụng rõ quy tắc R-02.

## 3. Calibration và kiểm lại

- Ca đã xử lý, rule ID, bản sửa và người xác nhận: Đã xử lý 557 ca trong calibration_decisions.csv, gán mã quy tắc R-01 đến R-07, sửa trên CVAT và kiểm lại bởi 102 (đã xác nhận).
- Tác động v1.1 lên nhãn và ảnh: Làm rõ cách ôm sát phần nhìn thấy (R-01), phân tách xe chở xe (R-02) và người ngồi trên xe (R-03); phạm vi rà soát 34 ảnh có gắn cờ.
- Tham chiếu bằng chứng khi Rework: Vòng 1 đạt chuẩn Release, không yêu cầu Rework.

## 4. Kết quả duyệt

- Vòng QC: 1
- Người review và ngày: 102, 2026-11-17
- Đã duyệt / cả lô: 40/40 = 100.0% cả lô.
- Ảnh lỗi / ảnh đã duyệt: 0/40 = 0.0% (đạt ngưỡng cho phép <= 2.0%).
- Annotation đối tượng hiện có bị lỗi / annotation đối tượng đã duyệt: 0/569 = 0.0%.
- Vật bỏ sót, ảnh và vùng: 0 vật bỏ sót.
- Ca khó / phần còn lại, lỗi lặp và ví dụ: Ca khó 34/34 ảnh đạt chuẩn 0% lỗi; phần còn lại 6/6 ảnh đạt 0% lỗi; không có lỗi lặp.
- Một lỗi máy chặn được và một lỗi cần người đọc ảnh: Máy tự động chặn box vượt quá biên ảnh hoặc sai cấu trúc; người đọc ảnh phân biệt được tượng người/xe mô hình ngoài phạm vi theo R-06.

## 5. Quyết định bàn giao

- Quyết định: Release
- Lý do, người nhận và phạm vi sử dụng: Tỉ lệ lỗi 0.0% dưới ngưỡng quy định 2.0%, bộ nhãn đạt độ tin cậy cao; bàn giao cho bộ phận đào tạo mô hình thị giác máy tính.
- Nếu còn việc: Không còn tồn đọng; dữ liệu sẵn sàng bàn giao.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: Ca C020 (img_002.jpg) đối chiếu R-02 trong guideline v1.1, nhãn cuối gán đúng xe chuyên chở là truck và các xe con là car.
