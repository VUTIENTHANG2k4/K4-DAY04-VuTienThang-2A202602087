# Mini guideline - nhóm: chưa cung cấp  |  người gán: Vũ Tiến Thăng  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao / ảnh mẫu |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt tại tâm khớp giải phẫu, ở chỗ thân người chuyển sang đùi; nếu bị che nhưng còn trong khung thì dùng `v=1`. | Suy ra từ eo, thân và hướng đùi. Ảnh mẫu: [train_15](outputs/vis_train/train_15.jpg) |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đặt tại vị trí tai ước lượng và dùng `v=1`; chỉ dùng `v=0` khi tai ngoài khung. | `left_ear` có 66% và `right_ear` có 45% `v=1`. Ảnh mẫu: [train_04](outputs/vis_train/train_04.jpg) |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Vẫn tạo đủ 17 điểm. Khớp ngoài ảnh dùng `v=0`; khớp bị che nhưng còn trong ảnh dùng `v=1`. | Không loại cả người; 33 điểm `v=0` tập trung ở gối/cổ chân. Ảnh mẫu: [train_15](outputs/vis_train/train_15.jpg) |
| Cổ tay nằm sau tay lái / sau thân mình | Theo liên tục của cẳng tay và hướng bàn tay; còn trong khung thì đặt ước lượng với `v=1`, không chuyển sang người bên cạnh. | Ảnh mẫu: [train_01](outputs/vis_train/train_01.jpg) |
| Hai người chồng lên nhau | Gán theo hộp thân, vai và chuỗi chi của từng người; bị che thì vẫn giữ keypoint trên đúng chuỗi với `v=1`. | Có lỗi `nham_nguoi` ở train_01 #2/right_wrist và train_04 #1/left_wrist. Ảnh mẫu: [train_01](outputs/vis_train/train_01.jpg) |
| Người nhỏ đến mức nào thì không gán nữa | Không đặt ngưỡng kích thước để bỏ người. Người nào trong ảnh cũng nhận đủ skeleton; visibility quyết định `v=0/1/2`. | 29 skeleton trong 20 ảnh, không thiếu hoặc thừa người khi so gold. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01.jpg`, người thứ `2`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: Hai người đứng sát nhau, cổ tay bị che và điểm dễ rơi sang cánh tay người còn lại.
- Bạn quyết thế nào: Theo chuỗi vai -> khuỷu -> cổ tay của người đang gán; còn trong ảnh thì dùng `v=1`.
- Vì sao: Gold báo lỗi `nham_nguoi` ở `right_wrist`, OKS 0.8300.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Gán cổ tay sang người bên cạnh, làm sai định danh người và hình học cánh tay.

### Ca 2 - ảnh `train_04.jpg`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Hai người đi xe máy chồng lên nhau; tay trái bị xe che và gần tay người kia.
- Bạn quyết thế nào: Giữ khớp trên cánh tay nối từ vai/khuỷu của đúng người, dùng `v=1` nếu còn trong khung.
- Vì sao: Gold phân loại `nham_nguoi` tại `left_wrist`, OKS 0.8582.
- Nếu người khác quyết ngược lại thì model học một cánh tay chuyển người khi có xe hoặc người chồng lên nhau.

### Ca 3 - ảnh `train_15.jpg`, người thứ `2`, khớp `right_elbow`

- Mơ hồ ở chỗ nào: Người ngồi trên xe máy bị thân xe che một phần, nên khuỷu không có bề mặt rõ.
- Bạn quyết thế nào: Suy ra điểm tại chỗ nối cánh tay trên và cẳng tay; giữ `v=1` vì khớp còn trong khung.
- Vì sao: Gold ghi lệch nhẹ 35 px, 1.1 lần bán kính dung sai; đây là lỗi vị trí chứ không phải lý do xoá khớp.
- Nếu người khác quyết ngược lại thì model học bỏ khuỷu ở tư thế ngồi sau xe và giảm khả năng dự đoán pose bị che.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: chưa xác định (chưa có bảng visibility của bạn cùng nhóm trong workspace).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: chưa thể kết luận khi thiếu bảng đối chiếu; bảng của tôi cho thấy tai là vùng hay bị che nhất.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: khớp bị che nhưng còn trong ảnh vẫn đặt chấm theo chuỗi giải phẫu đúng người và dùng `v=1`; chỉ dùng `v=0` khi ngoài mép ảnh.
