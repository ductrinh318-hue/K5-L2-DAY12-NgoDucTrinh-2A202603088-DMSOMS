# Danh sách lớp v0 — Ngọ Đức Trinh · G07

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_state | open / closed / unknown | Phát hiện tài xế ngủ gật (mắt nhắm liên tục > 1.5 - 2s hoặc tỉ lệ PERCLOS cao) để phát cảnh báo âm thanh/rung vô lăng | Có, trừ khi bị đốm phản xạ LED NIR che khuất |
| 2 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung (nhìn điện thoại ở đùi 'lap', nhìn màn hình 'center' quá lâu khi xe đang chạy) | Có (dựa vào hướng đầu và con ngươi mắt) |
| 3 | hand_on_wheel | yes / no / unknown | Giám sát tuân thủ an toàn lái xe (Hands-on detection) khi bật hệ thống hỗ trợ lái tự động ADAS / Lane Keeping Assist | Có, nhận diện rõ biên dạng bàn tay chạm vành vô lăng |
| 4 | phone_use | held / mounted / lap / none | Phát hiện hành vi sử dụng điện thoại khi lái xe trái quy định để đưa ra cảnh báo vi phạm an toàn giao thông | Có, nhận diện qua hình dạng cạnh thẳng, góc bo và phản xạ màn hình |
| 5 | glasses_type | none / clear / sun / unknown | Điều chỉnh thuật toán theo dõi mắt phù hợp (kính râm cần bù sáng/chuyển sang theo dõi hướng đầu thay vì con ngươi) | Có, nhưng kính râm dưới NIR có thể bị nhìn xuyên tròng nên cần nhận diện gọng |
| 6 | seatbelt_driver | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo chưa thắt dây an toàn hoặc thắt sai quy cách (luồn dưới nách/sau lưng) trước khi xe lăn bánh | Có, dựa vào dải chéo phản xạ ánh sáng qua ngực |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | seat_occupancy | empty / adult / child_seat / infant_seat / object | Quản lý logic kích hoạt túi khí thông minh (Smart Airbag Deployment) theo vị trí ghế có người ngồi; nhắc thắt dây an toàn | Có, phân biệt rõ qua hình dáng hình học và độ nổi khối |
| 2 | infant_seat_facing | rear / front | Cực kỳ quan trọng: Tắt túi khí khẩn cấp nếu có ghế trẻ em quay ngược về sau (rear-facing) để tránh túi khí nổ gây chấn thương cổ trẻ | Có, nhận diện qua quai xách và chiều xoay của nôi |
| 3 | child_presence_detection | yes / no / unknown | Hệ thống cảnh báo bỏ quên trẻ em trên xe (Child Presence Detection - CPD theo chuẩn Euro NCAP) sau khi tắt máy khóa cửa | Có, phân biệt được chuyển động hoặc hình dáng trẻ |
| 4 | seatbelt_rear | worn_correct / not_worn / unknown | Hiển thị bảng trạng thái cài dây an toàn hàng ghế sau trên cụm đồng hồ táp-lô cho tài xế nắm bắt | Có, nếu dây tương phản tốt với áo của hành khách |
| 5 | forgotten_object | bag / box / cup / other | Cảnh báo quên hành lý, túi xách, máy tính xách tay hoặc thú cưng ở hàng ghế sau trước khi tài xế rời khỏi xe | Có, phân biệt qua hình khối và kích thước đồ vật |
| 6 | seat_location | rear_left / rear_middle / rear_right | Định vị chính xác vị trí ghế phát sinh sự cố để hiển thị trực quan trên màn hình trung tâm xe | Có, dựa vào vị trí hình học của camera cabin |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. **Câu hỏi:** Khi tài xế đeo kính râm mà dưới camera NIR bước sóng 940nm tròng kính lại nhìn xuyên thấu được mắt như kính trắng, thì thuộc tính `glasses` nên ưu tiên gán theo bản chất quang học mắt thường (`sun`) hay theo trạng thái quan sát được trên ảnh IR (`clear`)?
   - **Khung ví dụ:** `D02.png`
   - **Bạn tạm gán thế nào và vì sao:** Tôi tạm gán `glasses = sun` dựa vào kiểu dáng gọng to dày của kính râm, vì nếu gán `clear` thì model có thể bị học sai đặc trưng phân loại kính râm thực tế. Cần guideline thống nhất giữa đội Annotation và đội huấn luyện mô hình thị giác.

2. **Câu hỏi:** Trong trường hợp đồ vật (ví dụ túi xách nhỏ hoặc gối tựa) nằm đè lên một phần của người ngồi hoặc nằm kẹp giữa hai vị trí ghế (giữa `rear_left` và `rear_middle`), việc phân bổ thuộc tính `seat` cho `object` nên dựa vào tâm trọng tâm của vật (box center) hay ghế chịu tải trọng chính?
   - **Khung ví dụ:** `S03_rgb.png` (đồ vật ở vùng giáp ranh các ghế)
   - **Bạn tạm gán thế nào và vì sao:** Tôi tạm gán thuộc tính `seat` dựa vào tọa độ tâm hình hộp (bounding box center) chiếu xuống ghế tương ứng để đảm bảo tính nhất quán và dễ kiểm tra tự động bằng code.
