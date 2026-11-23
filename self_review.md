# Tự đánh giá — Ngọ Đức Trinh · G07

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S06_rgb.png | child_seat | seat_flip | Lúc đầu gán nhầm ghế bên phải ảnh thành rear_right thay vì rear_left | Đã sửa |
| S01_rgb.png | frame_meta | ok | Đã gán đủ frame_meta modality=rgb, camera_blocked=no | Đã kiểm tra ok |
| D01.png | face13 | ok | 13 điểm khung xương mặt đặt chuẩn viền mắt và môi, cờ occluded đầy đủ | Đã kiểm tra ok |
| D02.png | hand | lr_flip | Cần chú ý xác định tay trái/phải theo cơ thể tài xế, không theo màn hình | Đã kiểm tra ok |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Nhầm lẫn vị trí ghế OMS theo góc nhìn của ảnh thay vì theo chiều xe chạy (`seat_flip`), cụ thể là ở ảnh S06 đã gán ghế bên phải ảnh thành `rear_right`.

Vì sao mình mắc: Do thói quen tự nhiên khi nhìn vào màn hình máy tính thì bên phải màn hình nghĩ là bên phải xe, trong khi camera OMS lại quay ống kính ngược từ trước ra sau nên bên phải màn hình thực chất là ghế bên trái xe (`rear_left`).

Lần sau tránh bằng cách: Luôn ghi nhớ câu thần chú "Ngược với màn hình": Mắt nhìn bên TRÁI ảnh $\rightarrow$ gán `rear_right`; Mắt nhìn bên PHẢI ảnh $\rightarrow$ gán `rear_left`. Đồng thời luôn chạy script `cabin_check.py` để phát hiện cảnh báo trùng ghế sớm.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó:
Ở ảnh S04_rgb.png đối với ghế trẻ em (`child_seat`), việc xác định thuộc tính `occupied` cần quan sát kỹ lòng ghế: nếu thấy rõ ràng không có gì bên trong thì dứt khoát gán `no`, không nên ngần ngại để `unknown`. Tuy nhiên ở những ảnh bị khuất tầm nhìn hoặc bóng tối thì nguyên tắc vàng "không chắc thì chọn unknown" vẫn là quy tắc bất di bất dịch của Guideline để tránh dạy mô hình đoán mò.
