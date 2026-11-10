# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên
- Họ tên: Nguyễn Thị Hương Giang
- MSSV: 2A202603233
- Email: 26ai.giangnth2@vinuni.edu.vn
- Lớp: E403
- Link repo bài làm: https://github.com/Giang0511/K5-L2B-Day02-NguyenThiHuongGiang-2A202603233-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1DhQMwaScUewTDkezrHaxAjsBSdDh8jjr?usp=sharing

## 2. Schema và mini guideline
[GUIDELINE_MINI.md](https://github.com/Giang0511/K5-L2B-Day02-NguyenThiHuongGiang-2A202603233-ObjectDetection/blob/main/GUIDELINE_MINI.md)

Ba ca mơ hồ đã ghi trong guideline:
- `drive_026`: hàng xe buýt nối đuôi nhau. Gán `bus` cho các xe còn thấy rõ thân xe, mỗi xe một box; các xe ở xa chỉ còn một vệt nhỏ thì không gán.
- `drive_005`: ô tô ở góc phải dưới bị cắt khung, chỉ thấy đuôi và một bánh. Lúc gán nhãn tôi không gán.
- `drive_009`: xe buýt phía sau bị che khoảng 50%. Lúc gán nhãn tôi không gán.

Theo GUIDE, hai ca sau (`drive_005`, `drive_009`) có thể là bỏ sót vì xe bị che hoặc cắt khung vẫn nên gán quanh phần nhìn thấy. Phần cần giảng viên làm rõ: ngưỡng phần nhìn thấy tối thiểu để gán, và ranh giới giữa `van` với `car`/`bus`.

## 3. Annotation và self-QC
- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: `py tools/check_yolo_labels.py` báo "ĐẠT cấu trúc: 50/50 file nhãn hợp lệ". Checker chỉ kiểm tra cấu trúc, không chứng minh nhãn đủ xe, đúng lớp hay box sát.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_003, drive_005, drive_007, drive_009, drive_011, drive_013, drive_015, drive_017, drive_019, drive_021
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: khi rà lại, `drive_005` (ô tô bị cắt khung ở góc phải dưới) và `drive_009` (xe buýt bị che khoảng 50%) có thể đã bỏ sót xe. Chưa sửa vì nhãn đã khóa và đã chấm với gold; hai ca này được ghi trong GUIDELINE_MINI.md.
- Commit khóa nhãn ban đầu: 4b64ced02c8343c531728059f3d395e94a3728c7

## 4. Gold QC và rework
| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 399 | không sửa |
| FP | 290 | không sửa |
| FN | 254 | không sửa |
| Precision | 0.5791 | không sửa |
| Recall | 0.611 | không sửa |
| F1 | 0.5946 | không sửa |
| mean_iou_of_matches | 0.8647 | không sửa |
| passed | false | không sửa |

Kết quả đầy đủ: `outputs/annotation_evaluation.json` trong repo. Tôi không rework nhãn. F1 = 0.5946 thấp hơn ngưỡng 0.80 trong GUIDE; giảng viên đã giải thích gold có lỗi chỉ số và chấp nhận ngưỡng F1 > 0.5 (<ghi thời điểm và kênh thông báo>). mean_iou_of_matches = 0.8647 đạt ngưỡng 0.70. Chưa kết luận được nguyên nhân của FP/FN chỉ từ các số này.

## 5. Kết quả YOLO
- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, CPU (Intel Xeon @ 2.20GHz), Ultralytics 8.4.208, Python 3.13.18, Torch 2.14.1+cpu
- Các tham số thay đổi so với notebook nếu có và lý do: không thay đổi tham số; chỉ điền REPO_URL.
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.2429 |
| Recall | 0.3931 |
| mAP50 | 0.3052 |
| mAP50–95 | 0.2161 |

Kết quả theo lớp trên 10 ảnh eval:

| Lớp | Ảnh | Instances | P | R | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|---:|
| car | 10 | 94 | 0.607 | 0.872 | 0.822 | 0.557 |
| truck | 5 | 5 | 0.279 | 0.2 | 0.274 | 0.23 |
| bus | 2 | 2 | 0.086 | 0.5 | 0.0672 | 0.0471 |
| van | 6 | 12 | 0 | 0 | 0.0579 | 0.0307 |

Ảnh dự đoán và log: [eval_predictions/visuals](assets/eval_predictions/visuals/), [yolo26_runs](assets/yolo26_runs/). Ví dụ ảnh cụ thể: [drive_056](assets/eval_predictions/visuals/drive_056.jpg), trong đó xe van bị mô hình nhận thành `car` (xem confusion matrix trong `assets/yolo26_runs/vehicle_eval/`).

Trả lời bốn câu hỏi cuối notebook:

1. **Lớp nào recall thấp nhất? Ảnh nào minh họa?**
Lớp `van` có recall = 0 (12 instance trong 10 ảnh eval nhưng mô hình không bắt đúng cái nào). Ví dụ `drive_056`, `drive_057`, `drive_059`: xe van bị nhận thành `car`.

2. **Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**
mAP50 = 0.305 và mAP50–95 = 0.216, chênh vừa phải. Với `car` là 0.822 so với 0.557: mô hình tìm đúng xe nhưng box chưa khớp chặt ở IoU cao. Trên box cần kiểm tra lề box (lỏng hoặc hụt), cách xử lý xe bị che, bị cắt khung và xe nhỏ ở xa. Mean IoU của nhãn tôi so với gold là 0.8647, tức box nhãn khá sát, nên tôi cho rằng phần chênh này nhiều khả năng đến từ mô hình ít dữ liệu hơn là từ nhãn, nhưng chưa đủ để kết luận chắc chắn.

3. **Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**
Lớp `van` là lớp dễ liên quan tới guideline nhất. Trong GUIDE, van/minivan mơ hồ với `bus` hoặc `car` thì không được tự chọn mà phải ghi case ID rồi hỏi lại, nghĩa là ranh giới giữa `van` và hai lớp kia vốn không rõ ràng. Kết quả trên 10 ảnh eval cho thấy `van` có 12 instance nhưng precision và recall đều bằng 0; ngược lại `car` có 94 instance và nhận khá tốt (recall 0.872, mAP50 0.822). Trong confusion matrix, xe van bị nhầm thành `car` 4 lần. Tuy vậy chưa thể kết luận lỗi nằm ở annotation: số mẫu giữa các lớp chênh rất nhiều (`car` 94, `van` 12, `truck` 5, `bus` 2) và tập train chỉ có 50 ảnh.

4. **Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**
Eval là phần dùng để đo; nếu đưa vào train, mô hình đã thấy đáp án nên metrics bị thổi phồng. Trong notebook, 10 ảnh eval còn được dùng làm validation để chọn `best.pt`, nên chúng không phải test set độc lập; vì vậy phải giữ nguyên split.

## 6. Khó khăn và cách xử lý
| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chạy checker/evaluator | `python` báo "Python was not found" (Windows chuyển sang Microsoft Store) | Kiểm tra `py --version`, dùng `py` thay cho `python` | Chạy được |
| Chạy checker | `[Errno 2] No such file or directory` | Phát hiện thư mục dự án lồng 2 tầng, `cd` vào thư mục con | Checker đạt 50/50 |
| Đưa nhãn lên GitHub | `fatal: not a git repository` vì tải repo bằng ZIP | Upload file qua giao diện web GitHub | Nhãn đã có trên GitHub |
| Chấm nhãn với gold | F1 = 0.5946 < 0.80 | Hỏi giảng viên | Giảng viên chấp nhận F1 > 0.5, không rework |
| Chạy Colab | Báo "Không gán được phần phụ trợ" / Colab lỗi cấp runtime | Thử kết nối lại, đổi loại runtime | Chạy được, nhưng phiên nộp chạy bằng CPU (torch +cpu) |

## 7. Tự đánh giá theo rubric
| Tiêu chí | Tình trạng | Bằng chứng / ghi chú |
|---|---|---|
| Hoàn thành và định dạng YOLO (10) | Đã có kết quả | Checker báo "ĐẠT cấu trúc: 50/50 file nhãn hợp lệ" |
| Đầy đủ vật thể (20) | Chưa kiểm chứng đầy đủ | Recall so với gold là 0.611 (FN = 254). Chưa kết luận được nguyên nhân từ số liệu; `drive_005`, `drive_009` có thể bỏ sót |
| Độ bám sát và hình học box (20) | Có số liệu hỗ trợ | mean IoU = 0.8647 (đạt ngưỡng 0.70) |
| Mini guideline (10) | Đã có | GUIDELINE_MINI.md có định nghĩa 4 lớp, quy tắc chung và 3 ca mơ hồ (drive_026, drive_005, drive_009) |
| Gold-set annotation QC (15) | Chưa đạt ngưỡng của rubric | F1 = 0.5946 < 0.80, mean IoU đạt. |
| Pipeline YOLO và phân tích (10) | Đã chạy | Colab chạy trên CPU, không dùng eval để train, có metrics, ảnh eval|

Phần còn thiếu / chưa kiểm được: nguyên nhân của FP/FN chưa xác định; hai ca có thể bỏ sót (`drive_005`, `drive_009`) chưa sửa vì nhãn đã khóa.

## 8. Công cụ AI và tài liệu tham khảo
- Công cụ AI đã dùng: Claude (Anthropic), ChatGPT.
- Các bước dùng AI hỗ trợ:
  + Hướng dẫn thao tác kỹ thuật: chạy `check_yolo_labels.py` và `evaluate_annotations.py` trên Windows, lỗi `No such file or directory` do thư mục dự án lồng hai tầng, lỗi `not a git repository` do tải repo bằng ZIP.
  + Gợi ý cấu trúc và cách diễn đạt khi điền báo cáo (mục 3, 5, 6, 7) và khi viết GUIDELINE_MINI.md.

- Tài liệu tham khảo: GUIDE.md, CHECKPOINTS.md, SUBMISSION.md, RUBRIC.md, RULES.md của đề bài.
