# Danh sách lớp v0 — Nguyễn Anh Thư · G06

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_left`, `eye_right` (từng mắt) | open / closed / unknown | Phát hiện buồn ngủ, ngủ gật: đếm thời gian mắt nhắm liên tục qua nhiều khung | Có, trừ khi đốm phản xạ LED trên tròng kính hoặc kính râm che mắt → `unknown` |
| 2 | `gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: tài xế rời mắt khỏi đường quá lâu (nhìn đùi, màn hình giữa) | Có, dựa vào hướng đầu và mắt; khó khi mặt quay hẳn hoặc mắt bị che |
| 3 | `face13` (13 điểm mặt) + cờ occluded/outside | toạ độ 13 điểm | Ước lượng hướng đầu (yaw, pitch), độ mở mắt và miệng (ngáp) cho cảnh báo buồn ngủ và mất tập trung | Có với mặt ≥ 60 px; điểm mắt hay bị che bởi phản xạ trên kính |
| 4 | `glasses` | none / clear / sun / unknown | Biết khi nào trạng thái mắt kém tin cậy (kính phản xạ, kính râm) để hạ độ tin cậy cảnh báo, tránh cảnh báo giả | Một phần: trên NIR kính râm có thể trông trong suốt; không chắc thì `unknown` |
| 5 | `hand` + `on_wheel` | yes / no / unknown; `side` left / right | Kiểm tra tay trên vô lăng (hands-on detection), cần cho chuyển quyền lái giữa xe và người | Có (hình dạng tay); vô lăng và tay có thể bị cắt khỏi khung hình |
| 6 | `phone` + `state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái (cầm áp tai, nhắn tin trên đùi) | Có khi thấy cạnh, góc hoặc màn hình; vật tối trong tay không đủ để kết luận |
| 7 | `seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây, phát hiện thắt sai (dây dưới nách, sau lưng) | Có khi dây tương phản với áo; áo tối hoặc vùng tối → `unknown` |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` + `seat` | driver / front_passenger / rear_left / rear_middle / rear_right | Đếm người theo từng ghế (gọi theo xe): nhắc thắt dây đúng ghế, điều chỉnh túi khí, điều hoà theo vùng | Có (hình dạng người); ghế phải gọi theo xe, không theo ảnh |
| 2 | `seatbelt` (hành khách) | not_worn / unknown (trên SVIRO) | Nhắc thắt dây cho từng ghế có người | Khó: trên IR-sim dây và áo tối dễ lẫn → `unknown` |
| 3 | `child_seat` + `occupied` | no / unknown | Biết ghế trẻ em đang lắp ở ghế nào; nhắc quên trẻ trong xe (child presence detection) | Có (hình dạng ghế); không thấy rõ bên trong thì `unknown` |
| 4 | `infant_seat` + `facing` | rear / front | Tắt túi khí ghế trước khi có ghế em bé quay ngược; nhắc quên trẻ | Có (hình dạng nôi xách) |
| 5 | `object` + `type` | bag / box / cup / other | Phân biệt ghế có vật với ghế có người (tránh nhắc thắt dây sai); nhắc quên đồ khi rời xe | Có theo hình dạng; không đoán bằng màu |
| 6 | `face` (OMS, chỉ box) | box | Xác nhận có người thật ở ghế (không phải vật), làm đầu vào cho đếm người và theo dõi hành khách | Có khi mặt hướng về camera; mặt nhỏ hoặc quay đi thì không vẽ |
| 7 | `frame_meta.camera_blocked` | yes / no | Phát hiện camera bị che để báo hệ thống OMS/DMS không còn tin cậy | Có |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi có một người ngồi trong ghế trẻ em (`child_seat`), `occupied` chỉ cho `no` hoặc `unknown`, còn guideline nói bộ ảnh không có trẻ em ngồi. Vậy nên gán `person` + `child_seat` (`occupied = unknown`), hay chỉ gán một trong hai, hay báo coach và bỏ qua?
   Khung ví dụ: G06 S05_rgb, S05_ir (ghế `rear_right`); tương tự ở S03 (`rear_middle`), S06_ir, S07_ir.
   Bạn tạm gán thế nào và vì sao: gán cả `person` và `child_seat` cùng ghế, `occupied = unknown`, vì `yes` không có trong guideline 4.8 và ghi `no` thì mâu thuẫn với box `person`.
2. Câu hỏi: Khi tài xế nhìn xuống phía trước giữa xe, ranh giới giữa `gaze_zone = lap` và `center` là gì (theo góc cúi đầu, hay theo vật tài xế đang nhìn)?
   Khung ví dụ: G06 D02 (mình ghi `lap`, bạn Hồ Xuân Hùng ghi `center`).
   Bạn tạm gán thế nào và vì sao: giữ `lap` theo hướng đầu và mắt; guideline 4.5 chưa có ngưỡng phân biệt hai vùng này nên cần người viết guideline quyết.
