# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Vũ Tiến Thăng   Nhóm: chưa cung cấp   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 347 / 113 / 33 |
| Thời gian trung bình mỗi ảnh | Chưa có log thời gian |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 66% (`v=1`)
2. `right_ear`: 45% (`v=1`)
3. `left_eye`: 34% (`v=1`)

Chúng khá phù hợp với các khớp khó gán vì tai và mắt thường bị tóc, mũ hoặc góc mặt che; đây là khó xác định vị trí giải phẫu chứ không chỉ là có ít bề mặt nhìn thấy. Cổ tay trái và hông trái đều có 31% `v=1`, nhưng thấp hơn ba khớp trên. Các điểm `v=0` lại tập trung ở gối và cổ chân, phản ánh khớp ra ngoài mép ảnh hơn là bị che.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | Chưa có bản trước rework | 0.9319 |
| OKS@0.50 | Chưa có bản trước rework | 1.0000 |
| OKS@0.75 | Chưa có bản trước rework | 1.0000 |
| Lỗi `dao_trai_phai` | Chưa có bản trước rework | 0 |
| Lỗi `nham_nguoi` | Chưa có bản trước rework | 2 |
| Lỗi `xoa_khop_bi_che` | Chưa có bản trước rework | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- Artifact hiện có chỉ lưu kết quả sau cùng, không có log trước rework nên không thể xác nhận thao tác sửa cụ thể.
- `train_01.jpg`, người #2, `right_wrist`: kết quả sau cùng vẫn ghi `nham_nguoi`; cần kiểm tra lại theo chuỗi tay của đúng người.
- `train_04.jpg`, người #1, `left_wrist`: kết quả sau cùng vẫn ghi `nham_nguoi`; cần kiểm tra lại theo vai-khuỷu-cổ tay của đúng người.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Không có lỗi `dao_trai_phai` trong kết quả sau cùng của toàn bộ 20 ảnh. Vẫn còn 2 lỗi `nham_nguoi`, là lỗi định danh keypoint khác với đảo trái/phải.

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: chưa cung cấp

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| Chưa xác định | Chưa có | Chưa có | Chưa tính được | Thiếu bảng visibility của bạn cùng nhóm |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Khi khớp bị che bởi vật thể hoặc người khác nhưng vị trí vẫn nằm trong ảnh, đặt điểm theo chuỗi giải phẫu đúng người và dùng `v=1`; chỉ dùng `v=0` khi khớp nằm ngoài mép ảnh.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` tăng 0.0055, từ 0.6853 lên 0.6908; vì không giảm nên không có phần “làm hỏng” cần giải thích theo giả định đó. Mức tăng nhỏ phù hợp với 20 ảnh, trong đó có 113/493 điểm bị che (`v=1`) và vẫn còn lỗi nhầm người.

2. Chênh `box_mAP50-95 - pose_mAP50-95` là 0.1266 ở baseline và 0.1133 sau fine-tune. Model tìm người dễ hơn tìm khớp: box chỉ đo vùng người, còn pose cần vị trí chính xác của 17 khớp; các ca chồng lấn như train_01 và train_04 làm bài toán khớp khó hơn.

3. Notebook không lưu bảng dự đoán theo từng ảnh test hoặc ảnh lỗi, nên không thể gọi tên một lỗi model trên test mà không suy diễn. Bằng chứng gần nhất trong kết quả nhãn là `train_01.jpg`, người #2, `right_wrist`: lỗi `nham_nguoi`, OKS 0.8300.

4. Output hiện có không chứa OKS giữa nhãn và model theo từng ảnh; cell notebook chỉ có code in bảng nên không xác định được ảnh thấp nhất của cặp này. Với nhãn so gold, ảnh thấp nhất là `train_01.jpg`, người #2, OKS 0.8300; gold chỉ ra `right_wrist` bị nhầm người.

5. Không thể kiểm tra đồng thời vì thiếu bảng OKS model-vs-nhãn. Theo gold, `train_01.jpg` là ca tệ nhất và có lỗi nhầm người; nếu bảng model cũng cho ảnh này thấp, điều đó sẽ cho thấy cảnh hai người chồng lấn làm khó cả người gán lẫn model.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Ở `train_15.jpg`, người #2, `right_elbow` bị thân xe che một phần nhưng cánh tay trên và cẳng tay vẫn còn trong khung, nên vị trí khuỷu có thể suy ra từ hai đoạn chi liền kề. Tôi chọn đặt điểm tại khớp giải phẫu ước lượng và dùng `v=1`, không dùng `v=0` chỉ vì không nhìn thấy toàn bộ bề mặt. Kết quả gold cũng phân loại đây là `lech_nhe` 35 px, không phải lỗi xoá khớp bị che.
