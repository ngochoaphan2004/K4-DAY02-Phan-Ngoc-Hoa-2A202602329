# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Phan Ngọc Hòa<br>
**MSSV:** 2A202602329<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: drive_008 / chiếc xe màu đỏ bên góc phải trên của hình
- Dấu hiệu nhìn thấy: chiếc xe nhỏ gọn giống xe van nhưng lại có nhiều cửa sổ và chổ ngồi giống xe buýt
- Quy tắc áp dụng: khớp với điều kiện sua cấu trúc 4 bánh xe thân xe, thân có hình khối có hộp kín phía sau xe dùng để chở đồ.
- Quyết định: van
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đánh dấu phần review_state là needs_review, và  hỏi Lab Coach để xác định rõ ràng hơn

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: drive_038 / xe cẩu cứu hộ màu trắng ở giữa ngã tư
- Dấu hiệu nhìn thấy: Thân xe nhỏ gọn giống van, nhưng phía sau cabin gắn cần cẩu và thiết bị kéo công vụ lộ thiên rõ ràng
- Quy tắc áp dụng: Gán truck khi thấy rõ thiết bị công vụ, sàn hàng hoặc bệ gắn thiết bị tách biệt phía sau - dù thân xe trông nhỏ như van
- Quyết định: truck
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đánh dấu needs_review, ghi chú mô tả chi tiết vào nhật ký quyết định và hỏi Lab Coach để được xác nhận cách phân lớp

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: drive_008 / chiếc xe màu đen bị che phía sau đuôi xe buýt, nằm phía bên phải ảnh
- Dấu hiệu nhìn thấy khi phóng 100%: xe bị che đi rất nhiều, chỉ còn lại một phần nhỏ của chiếc xe
- Giá trị `visibility`: occluded
- Giá trị `boundary`: inside
- Trạng thái `review_state`: needs_review
- Lý do: Vật thể nằm trọn vẹn trong khung hình và không chạm mép ảnh nên chọn inside. Tuy nhiên xe bị thân xe buýt che khuất phần lớn diện tích nên chọn occluded. Do phần lộ ra quá nhỏ, thiếu đặc điểm nhận dạng rõ ràng để chắc chắn phân lớp (car hay van) và khó ước lượng chuẩn biên hộp giới hạn nhìn thấy, nên cần đặt needs_review để kiểm tra lại và xin hỗ trợ từ Lab Coach.

## 6. Xác nhận tự kiểm tra

- [x] Đã rà đủ bốn ảnh.
- [x] Đã kiểm vật thể thiếu và trùng.
- [x] Đã kiểm lớp và hình học từng hộp.
- [x] Mỗi hộp có đủ ba thuộc tính.
- [x] Đã xử lý mọi hộp `needs_review`.
- [x] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [x] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [x] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [x] Số vật thể thực tế: 84
