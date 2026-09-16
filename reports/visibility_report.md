# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 15 ảnh, 21 skeleton, trung bình 15.43 khớp có v > 0 mỗi người
- Tổng: v=2 237 | v=1 87 | v=0 33

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 17 | 4 | 0 | 19% |
| 1 | left_eye | 16 | 5 | 0 | 24% |
| 2 | right_eye | 16 | 5 | 0 | 24% |
| 3 | left_ear | 8 | 13 | 0 | 62% |
| 4 | right_ear | 11 | 10 | 0 | 48% |
| 5 | left_shoulder | 20 | 1 | 0 | 5% |
| 6 | right_shoulder | 21 | 0 | 0 | 0% |
| 7 | left_elbow | 17 | 4 | 0 | 19% |
| 8 | right_elbow | 18 | 3 | 0 | 14% |
| 9 | left_wrist | 14 | 7 | 0 | 33% |
| 10 | right_wrist | 14 | 6 | 1 | 29% |
| 11 | left_hip | 12 | 8 | 1 | 38% |
| 12 | right_hip | 14 | 6 | 1 | 29% |
| 13 | left_knee | 11 | 4 | 6 | 19% |
| 14 | right_knee | 10 | 5 | 6 | 24% |
| 15 | left_ankle | 9 | 3 | 9 | 14% |
| 16 | right_ankle | 9 | 3 | 9 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
