# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Phương tiện ô tô chở người thông thường. |Sedan, hatchback, SUV|
| `truck` |Phương tiện được thiết kế chủ yếu để vận chuyển hàng hóa. |Xe tải thùng, xe tải chở hàng.|
| `bus` |Phương tiện chuyên chở nhiều hành khách. |Xe buýt có thân dài, nhiều cửa sổ và cửa lên xuống. |
| `van` |Phương tiện có khoang cabin và khoang chở hàng hoặc hành khách liền nhau. |Xe van giao hàng, xe van chở khách. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là:  Bao quanh phương tiện theo đường viền ngoài có thể quan sát được, hạn chế lấy thừa phần nền.
- Vật thể bị che khuất: Gán nhãn nếu vẫn có thể nhận diện chắc chắn phương tiện. Box theo phạm vi vật thể quan sát được, không tự suy đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: Gán nhãn nếu phần còn lại đủ để nhận diện lớp; box không vượt ra ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ:Chỉ gán nhãn khi có đủ đặc điểm nhận diện. Không tự đoán lớp khi không đủ thông tin.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_005.jpg` |Nhiều phương tiện ở xa, kích thước nhỏ và nằm gần nhau |Gán nhãn riêng từng xe khi vẫn nhận diện được; không gộp nhiều xe vào một box. |Mỗi phương tiện là một object độc lập, cần có class và bounding box riêng. |
| `drive_007.jpg` | Một số phương tiện bị che khuất bởi các cây và các vật thể khác.|Chỉ gán nhãn khi có thể xác định phương tiện; box theo phần nhìn thấy theo quy định của bài lab. |Không suy đoán phần bị che khuất và không gán lớp khi thiếu bằng chứng nhận diện. |
| `drive_009.jpg` | Xe buýt, xe tải và ô tô xuất hiện sát nhau, các bounding box dễ chồng lấn.|Tạo box riêng cho từng phương tiện và phân loại theo đặc điểm nhận diện của từng xe. |Không gộp các object khác nhau; class phải phản ánh đúng loại phương tiện |
