# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` (0) | Phương tiện giao thông thiết kế để chở người, thường có từ 4 đến 7 chỗ, có 4 bánh và từ 2 đến 4 cửa. | sedan, hatchback, SUV, crossover, coupe, taxi con. Điểm phân biệt: thấp hơn `van`, có khoang động cơ và khoang hành lý nhìn thấy rõ. |
| `truck` (1) | Phương tiện thiết kế để vận chuyển hàng hóa, vật liệu và hàng có tải trọng khác nhau. Thường có cabin cho người lái và khu vực chở hàng riêng biệt. | xe tải nhẹ, xe tải nặng, xe tải thùng kín, xe ben, xe bán tải. Điểm phân biệt: cabin và thùng hàng tách biệt. |
| `bus` (2) | Phương tiện kích thước lớn, thiết kế để chở nhiều hành khách. Thân xe dài, cao, có nhiều hàng cửa sổ và nhiều ghế. | xe buýt thành phố, xe khách đường dài, xe buýt nối đuôi nhau. Điểm phân biệt: dài và cao hơn hẳn `van`, nhiều cửa sổ dọc thân. |
| `van` (3) | Phương tiện có thân dạng hình hộp, chở hàng hóa hoặc hành khách. Kích thước thường nhỏ hơn xe buýt và xe tải. | van chở hàng, van giao hàng, van chở khách, minivan. Điểm phân biệt: thân hình hộp liền khối, cao hơn `car`, nhỏ hơn `bus`. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box ôm phần thân xe nhìn thấy được, không lấy thêm mặt đường, trời hay xe bên cạnh. Mỗi xe một box riêng, kể cả các xe cùng lớp đứng sát nhau.
- Vật thể bị che khuất: vẫn gán box quanh phần nhìn thấy được nếu nhận diện được lớp. Hai xe chồng nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh, không đoán phần nằm ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: nếu chỉ còn mảnh rất nhỏ hoặc không xác định chắc lớp thì không tự đoán; ghi vào bảng bên dưới và hỏi giảng viên.
- Xe đang đỗ vẫn gán theo loại xe.
- Không gán: người đi bộ, xe đạp, mô tô/xe máy, biển báo, đèn giao thông, hình xe trên bảng quảng cáo, phản chiếu trong kính/gương.
- `van`/minivan mơ hồ với `bus` hoặc `car`: không tự chọn, ghi case ID và hỏi.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_026` | Hàng xe buýt nối đuôi nhau; xe phía sau bị xe phía trước che một phần. | Gán `bus` cho các xe còn nhìn rõ thân xe, mỗi xe một box riêng trên phần nhìn thấy. Các xe buýt ở xa chỉ còn một vệt nhỏ thì không gán. | Hai xe chồng nhau thì vẽ hai box riêng; vật thể bị che vẫn gán nếu nhận diện được lớp; mảnh quá nhỏ không xác định chắc lớp thì không tự đoán. |
| `drive_005` | Ô tô ở góc phải dưới bị cắt khung, chỉ thấy phần đuôi và một bánh. | Không gán khi gán nhãn. | Lúc đó tôi coi phần nhìn thấy quá ít để chắc chắn về lớp. Khi rà lại, theo GUIDE xe bị cắt khung vẫn gán với box chạm rìa ảnh, nên đây có thể là một chỗ bỏ sót; chưa sửa vì nhãn đã khóa và đã chấm với gold. |
| `drive_009` | Một xe buýt phía sau bị xe buýt phía trước che khoảng 50%. | Không gán khi gán nhãn. | Lúc đó tôi coi xe nằm sau nhiều xe khác và bị che nhiều. Khi rà lại, nếu còn nhận ra thân xe thì theo GUIDE nên gán box quanh phần nhìn thấy; đây có thể là bỏ sót, chưa sửa vì nhãn đã khóa. |

## Điểm cần giảng viên làm rõ

- Ngưỡng cụ thể cho xe bị che hoặc bị cắt khung: phần nhìn thấy tối thiểu bao nhiêu thì nên gán.
- Ranh giới giữa `van` và `car`/`bus` với các xe thân hộp cỡ trung.
