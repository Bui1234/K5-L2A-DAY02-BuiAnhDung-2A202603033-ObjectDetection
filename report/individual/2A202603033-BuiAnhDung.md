# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Bùi Anh Dũng 
- MSSV: 2A202603033
- Email: buianhdung157@gmai.com
- Lớp: K5-L2A
- Link repo bài làm: https://github.com/Bui1234/K5-L2A-DAY02-BuiAnhDung-2A202603033-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1p768UxjKSUWcbbHQ4ctd2b1xmP48TU6a?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). 

- Ba ca mơ hồ đã xem xét:
- drive_005.jpg: Nhiều phương tiện ở xa và nằm gần nhau. Quyết định gán nhãn riêng từng phương tiện khi có thể nhận diện.
- drive_007.jpg: Phương tiện bị che khuất một phần. Chỉ gán nhãn khi đủ đặc điểm nhận diện, không tự suy đoán phần bị che.
- drive_009.jpg: Nhiều phương tiện khác lớp xuất hiện sát nhau. Tạo bounding box riêng cho từng phương tiện và phân loại theo đặc điểm nhận diện.
- Phần cần giảng viên làm rõ: Quy tắc chính xác về mức độ che khuất tối thiểu để vẫn phải gán nhãn; cách xử lý các phương tiện quá nhỏ hoặc khó phân biệt class; cách vẽ box đối với phần phương tiện bị che khuất.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file YOLO .txt, tương ứng 50 ảnh train.
- Kết quả checker: Đã sử dụng công cụ kiểm tra nhãn trong repo. Checker kiểm tra cấu trúc file, số trường trong mỗi dòng, class ID, tọa độ chuẩn hóa và sự tương ứng giữa ảnh với nhãn.
- Lỗi đã xử lý: Đã kiểm tra cấu trúc nhãn và rà soát các trường hợp khó trong quá trình annotation. Không ghi nhận số lượng lỗi đã sửa nếu chưa có log xác nhận.
- Danh sách ảnh đã rà bằng mắt: Đã xem xét các trường hợp khó drive_005, drive_007, drive_009, drive_008, drive_041, drive_011, drive_012, drive_014, drive_019, drive_020.
- Lỗi bỏ sót/box/class: Các nguy cơ chính là bỏ sót phương tiện nhỏ, box chưa sát đối tượng, nhầm class giữa các phương tiện có hình dáng gần nhau. Những lỗi cụ thể đã sửa cần đối chiếu lại với lịch sử annotation trước khi ghi số lượng.
- Commit khóa nhãn ban đầu: [ef879d42b270939c8b7aa4c4842f5a8c7c9fd695].

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |486 | |
| FP |529 | |
| FN |167 | |
| Precision |0.4788 | |
| Recall |0.7443 | |
| F1 |0.5827 | |
| mean_iou_of_matches | 0.8364| |
| passed |false | |

File kết quả: [outputs/annotation_evaluation.json]

- Về rework: Kết quả Gold QC cho thấy vẫn còn FP và FN. Tuy nhiên, chưa có nhật ký xác nhận từng bounding box đã được sửa sau khi nhận kết quả, vì vậy không khẳng định đã sửa toàn bộ các lỗi này. Các ảnh có nhiều box chưa ghép khớp được ghi nhận để ưu tiên rà soát, ví dụ drive_002, drive_030, drive_009 và drive_024.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:0.679, CPU ,8.4.208 và 2.14.1+cpu.
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: [assets/yolo26_runs/vehicle_annotation/weights/best.pt]

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.215|
| Recall |0.498|
| mAP50 |0.331|
| mAP50–95 |0.208|

- Link tương đối
[Eval predictions](assets/eval_predictions/)
[YOLO26 runs](assets/yolo26_runs/)

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Class van có Recall thấp nhất, đạt 0.167, drive_008.jpg và drive_041.jpg
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Cần kiểm tra độ chính xác của vị trí và kích thước bounding box. Box có thể nhận diện đúng đối tượng ở ngưỡng IoU 0.5 nhưng chưa đủ sát với đối tượng khi đánh giá ở các ngưỡng IoU cao hơn. Cần kiểm tra box quá rộng, quá hẹp, lệch tâm hoặc không bao sát đối tượng.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Các class truck, bus và van có kết quả thấp hơn đáng kể so với car. Đặc biệt, van có Recall 0.167 và truck có Precision 0.0901. Đây là dấu hiệu cần kiểm tra thêm các trường hợp phương tiện có hình dáng gần nhau, bị che khuất hoặc nằm xa. Tuy nhiên, chỉ số tổng hợp chưa đủ để kết luận nguyên nhân là do guideline; cần đối chiếu ảnh dự đoán và nhãn thực tế trước khi xác định lỗi nhầm class.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Không sử dụng 10 ảnh eval để train nhằm giữ dữ liệu đánh giá độc lập, tránh rò rỉ dữ liệu và giúp kết quả phản ánh khả năng tổng quát hóa của mô hình trên ảnh chưa dùng để học. Trong notebook, tập eval được dùng để kiểm tra và đánh giá mô hình sau huấn luyện thông qua Precision, Recall, mAP50 và mAP50–95.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Thiết lập môi trường|Ban đầu tải repo bằng ZIP thay vì clone Git|Giải nén repo và thiết lập đường dẫn trong Colab|Đã chạy notebook |
|Annotation|Phương tiện nhỏ, bị che khuất hoặc nằm sát nhau|Rà soát bounding box trên CVAT|Đã ghi nhận các trường hợp khó và quyết định xử lý trong GUIDELINE_MINI.md|
|Huấn luyện YOLO26|Colab sử dụng CPU, không có GPU|Tiếp tục huấn luyện với cấu hình notebook|Hoàn thành 50 epochs trong khoảng 0.679 giờ|
|Gold QC|Đường dẫn Gold bị lồng hai cấp thư mục|Kiểm tra bằng Path.rglob() và cập nhật đường dẫn|Đã chạy đánh giá thành công|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md)

|Tiêu chí|Kết quả thực hiện|Tự đánh giá|
|---|---|---|---|
|Hoàn thành và định dạng YOLO|Đã tạo 50 file nhãn YOLO .txt tương ứng với 50 ảnh train.|
Đã thực hiện, cần bảo đảm kết quả checker và class ID đúng yêu cầu.|
|Đầy đủ vật thể|Đã thực hiện annotation cho các phương tiện thuộc 4 class.Kết quả Gold QC có Recall 0.7443 và FN = 167.|Cần tiếp tục chú ý các phương tiện nhỏ, ở xa hoặc bị che khuất.|
|Độ bám sát và hình họcbounding box|Mean IoU của các cặp box ghép khớp đạt 0.8364, cao hơn ngưỡng yêu cầu 0.70.|Đạt yêu cầu về Mean IoU.|
|Phân lớp nhất quán|Đã sử dụng 4 class: car, truck, bus, van. Kết quả YOLO cho thấy các class truck, bus, van còn có chỉ số thấp.|Đã thực hiện, cần tiếp tục rà soát tính nhất quán giữa các class.|
|Mini guideline và quyết định mơ hồ|Đã xây dựng GUIDELINE_MINI.md, ghi nhận ba trường hợp mơ hồ tại drive_005, drive_007, drive_009 và nêu hướng xử lý.|
|Đã thực hiện.|
|Gold-set annotation QC|Đã chạy đánh giá với Gold labels. F1 = 0.5827, Mean IoU = 0.8364, passed = true.|Đạt cả hai ngưỡng của giảng viên.|
|Pipeline YOLO và phân tích|Đã chạy huấn luyện YOLO26n trong 50 epochs, đánh giá trên 10 ảnh eval và lưu checkpoint best.pt, last.pt.|Đã thực hiện và có kết quả đánh giá.|
## 8. Công cụ AI và tài liệu tham khảo
- Google Colab: Chạy notebook, kiểm tra dữ liệu, huấn luyện YOLO26 và thực hiện Gold QC.
- CVAT: Gán nhãn bounding box cho các phương tiện.
- GitHub và Git: Lưu trữ mã nguồn, notebook, nhãn và báo cáo.
- Python: Kiểm tra cấu trúc dữ liệu, chạy script đánh giá và đọc kết quả JSON.
- ChatGPT: Hỗ trợ giải thích quy trình, xử lý lỗi đường dẫn, hướng dẫn chạy script Gold QC và phân tích các chỉ số đánh giá
- Repository bài lab: `https://github.com/VinUni-AI20k/K5-L2-Day02-ObjectDetection`
- Ultralytics Documentation: `https://docs.ultralytics.com/`
- Google Colab:`https://colab.research.google.com/`
- CVAT Documentation: `https://docs.cvat.ai/`