# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Phan Ngọc Hòa<br>
**MSSV:** 2A202602329<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: drive_008, drive_022, drive_033, drive_038
- Số vật thể thực tế: 84
- Mã SHA-256 của gói YOLO của bạn: 3fe06933cee1e768bf130dfc2b50046265f70c5f9285f9d62128b44af546fb78
- Mã SHA-256 của gói CVAT gốc của bạn: 1e978818f6c26cb47063acdf8610de4f59015a728e5fc67e891ef73b06552583
- Nguồn đối chiếu: bộ tham chiếu do người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu: c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: 11h10p ngày 14/09/2026

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:
Vì bài được làm hoàn toàn cá nhân, tự đánh giá và dán nhãn dựa trên quan điểm cá nhân cũng như bám sát các tiêu chuẩn, hướng dẫn cơ bản của công việc mà chưa hề tham khảo hay trao đổi kết quả với bất kỳ bộ nhãn tham chiếu hay cá nhân nào khác.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_008 | car | Cấu trúc 4 bánh xe thân xe nhỏ gọn, thân có hình khối rõ ràng, kín  | Gán car cho xe sedan, hatchback, SUV |
| drive_008 | bus | Xe buýt dài nhiều chỗ ngồi, nhiều cửa sổ, có khớp nối ở giữa xe hoặc không | Gán bus cho xe thân dài, nhiều cửa sổ |
| drive_008 | truck | Xe tải màu đỏ, có thùng ben nhô cao chứa đầy đất | Gán truck khi có thùng, ben rõ ràng |
| drive_008 | van | Cấu trúc 4 bánh xe thân xe, thân có hình khối có hộp kín phía sau xe dùng để chở đồ | Gán van cho xe thân hộp nhỏ, kín |
| --- | --- | --- | --- |
| drive_022 | car | Cấu trúc 4 bánh xe thân xe nhỏ gọn, thân có hình khối rõ ràng, kín | Gán car cho xe sedan, SUV |
| drive_022 | bus | Xe buýt lớn chạy qua đầu ngả tư, sơn màu vàng và xanh dương | Gán bus cho xe thân dài, nhiều cửa sổ |
| drive_022 | truck | Chiếc xe tải chở hàng màu trắng, phần thùng hộp tách biệt phía sau | Gán truck khi có thùng, ben rõ ràng |
| drive_022 | van | Không có | Không có |
| --- | --- | --- | --- |
| drive_033 | car | Các xe sedan màu đỏ, đen và taxi gầm thấp trên các làn | Gán car cho xe sedan, hatchback, SUV |
| drive_033 | bus | Một phần thân xe khách cỡ lớn màu đỏ hoặc xe buýt chạy tuyến | Gán bus cho xe thân dài, nhiều cửa sổ |
| drive_033 | truck | Xe có thùng hộp kín màu trắng cỡ vừa dùng để chở hàng | Gán truck khi có thùng, ben rõ ràng |
| drive_033 | van | Không có | Không có |
| --- | --- | --- | --- |
| drive_038 | car | Ô tô con 4 chỗ màu trắng, đen thuôn gọn ở phần dưới ảnh | Gán car cho xe sedan, hatchback, SUV |
| drive_038 | bus | Chiếc xe khách lớn ở góc trên bên phải và bến trái | Gán bus cho xe thân dài, nhiều cửa sổ |
| drive_038 | truck | Xe cẩu cứu hộ màu trắng và chiếc xe tải chở hàng màu xanh | Gán truck khi có thùng, bệ hoặc thiết bị công vụ |
| drive_038 | van | Chiếc xe thân hộp màu đỏ, trắng đặc trưng | Gán van cho xe thân hộp nhỏ, kín |
| --- | --- | --- | --- |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

Lớp dùng để phân loại loại phương tiện, trả lời đó là loại xe gì. Thuộc tính dùng để cung cấp thông tin về trạng thái hoặc tính chất của vật thể trong khung hình (ví dụ: chiếc xe buýt đó có thể bị che khuất một phần nên thuộc tính visibility là `occluded` hoặc `unclear`), thể hiện vật thể đó xuất hiện như thế nào.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Xe buýt và xe tải | phạm vi | Xe buýt và xe tải có độ rộng của box lớn hơn so với file đối chiều | Giảm độ rộng nhỏ hơn khi bỏ các chi tiết như kính chiều hậu, ụ đất. |
| Boxing dư các car ở xa | phạm vi | Phóng to ảnh, thấy xe quá nhỏ và mờ, không đủ đặc điểm nhận dạng lớp | Xóa các hộp đó. Quy tắc: không gán vật thể quá nhỏ/mờ không có căn cứ phân lớp |


- Số hộp `needs_review` trước và sau khi kiểm:
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ:
  Một xe ở góc ảnh drive_008 có thân hộp nhỏ nhưng bị che khuất phần lớn, không phân biệt được rõ là van hay car. Mình đánh dấu `needs_review`, ghi chú vào nhật ký quyết định và hỏi Lab Coach để được hướng dẫn áp dụng đúng quy tắc.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: 2, 0.381437, 0.723539, 0.435875, 0.346766
- Tên lớp và tọa độ điểm ảnh `xyxy`: lớp 2 (bus) tâm=(0.3814, 0.7235) | kích thước=(0.4359, 0.3468)
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

Định dạng YOLO chỉ kiểm tra cú pháp, không kiểm tra ngữ nghĩa nên vẫn cần kiểm tra lại độ chính xác khi gán nhãn:
- Sai lớp: Số class ID đúng kiểu nhưng người gán nhầm, ví dụ gán xe van cho một chiếc xe buýt.
- Sai phạm vi: Hộp bao một xe quá nhỏ/mờ không đủ căn cứ phân lớp, nhưng dòng YOLO vẫn hợp lệ về định dạng.
- Sai hình học: Tọa độ hợp lệ nhưng hộp bao chứa nhiều nền xung quanh, cắt mất phần xe, hoặc gộp hai xe vào một hộp.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: drive_022, drive_033, drive_038
- Mã ảnh thẩm định: drive_008
- Mô tả một dự đoán trong `detect_result.jpg`:
  `detect_result.jpg` chính là ảnh `drive_008` gốc không có bất kỳ hộp dự đoán nào được vẽ lên — mô hình không phát hiện được vật thể nào trong ngưỡng tin cậy mặc định.
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
  Các thứ cần kiểm lại là số lượng ảnh huấn luyện quá ít (chỉ 3 ảnh, 1 epoch) dẫn đến mô hình chưa hội tụ và có thể ngưỡng confidence mặc định quá cao so với khả năng mô hình;
- Minh chứng nào có thể bác bỏ nhận định của bạn?
  Nếu hạ ngưỡng confidence xuống thấp mà mô hình vẫn không cho ra dự đoán nào, thì nguyên nhân chính là mô hình chưa học được đặc trưng, không phải do ngưỡng. Ngược lại, nếu ở ngưỡng thấp có dự đoán xuất hiện thì ngưỡng mới là vấn đề.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
  Vì tập dữ liệu quá nhỏ (3 ảnh train, 1 ảnh val), chỉ huấn luyện 1 epoch, không đủ để mô hình hội tụ và không đại diện cho phân phối dữ liệu thực tế. Kết quả mAP/IoU trên bốn ảnh này chỉ là tín hiệu chẩn đoán nhanh, không có ý nghĩa thống kê để đánh giá hiệu năng thực sự của mô hình trong triển khai.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 46
- IoU trung bình và trung vị:
- Mức đồng thuận lớp: 0.86504 và 0.889714
- Số hộp phía bạn không ghép được: 38
- Số hộp phía đối chiếu không ghép được: 4
- Một điểm khác biệt cụ thể: Một số vật thể nhìn thấy rõ hình dáng và có thể xác định được đặc trưng lớp, nhưng trong bộ nhãn tham chiếu lại không có hộp bao tương ứng, cho thấy hai bên áp dụng ngưỡng đủ bằng chứng phân lớp khác nhau.
- Quy tắc hoặc hành động sửa phát sinh: Cần làm rõ và thống nhất ngưỡng tối thiểu để quyết định gán nhãn một vật thể; nếu bộ đối chiếu thiếu quá nhiều vật thể rõ ràng thì cần xin bộ tham chiếu đầy đủ hơn từ Lab Coach.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
  Vì hai người có thể đồng thuận dựa trên cùng một cách hiểu sai về định nghĩa lớp hoặc cùng bỏ sót một nhóm vật thể theo cùng một quy tắc thiếu chính xác. Đồng thuận chỉ đo khả năng tái lập quy tắc giữa hai người gán nhãn, không đảm bảo rằng quy tắc đó là đúng.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

