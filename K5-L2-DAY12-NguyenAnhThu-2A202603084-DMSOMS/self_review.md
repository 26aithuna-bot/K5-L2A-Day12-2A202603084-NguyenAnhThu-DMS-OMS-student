# Tự đánh giá — Nguyễn Anh Thư · G06

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb | 2 `person`, `infant_seat` | `seat_flip` | Người và ghế em bé bên trái ảnh ghi `rear_left`, đúng phải là `rear_right` (camera nhìn về phía sau xe, GUIDE 4.4-3) | Có, trong `export_final.zip` |
| S07_rgb | `person`, `child_seat` ở `rear_middle` | `missing` | Ảnh chỉ có tag `frame_meta`, chưa có box; S07_ir cùng cảnh đã có 2 box này | Chưa (ghi nhận sau hạn, theo RULES §6) |
| D01 | `face13` (8 điểm mắt) | `lr_flip` | Điểm `left_eye_*` đặt ở mắt có y nhỏ hơn; trên ảnh D## xoay 90° mắt trái có y lớn hơn (GUIDE 4.6) | Có, trong `export_final.zip` |
| D03 | `face` (`eye_left`, `eye_right`) | `guess` | Ghi `open` trong khi không nhìn rõ khe mắt (cả 8 điểm mắt bật cờ che); đúng ra phải là `unknown` (GUIDE 4.4-1, 4.5) | Chưa (ghi nhận sau hạn) |

**Lỗi phát hiện thêm sau khi nộp** (theo RULES §6, không sửa lại `export_final.zip`, chỉ ghi ở đây):

| Ảnh | Đối tượng | Loại lỗi | Ghi chú |
|---|---|---|---|
| D01 | `face13` (4 điểm `left_eye_*`) | `flag` | Mắt trái không thấy rõ, đã ghi `eye_left = unknown` nhưng quên bật cờ `occluded` cho 4 điểm |
| D05 | `face` (`eye_left`, `eye_right`) | `guess` | Mặt nghiêng, mắt trái khuất, mắt phải không rõ nhưng ghi `open`; đúng ra `unknown` |
| D06 | `face` (`eye_left`, `eye_right`) | `guess` | Không thấy rõ hai mí khép nhưng ghi `closed`; đúng ra `unknown` |
| S07_rgb | `person`, `child_seat` | `missing` | Chưa gán box (xem bảng trên) |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: `seat_flip` — gọi ghế theo bên trái/phải của ảnh thay vì theo xe, lặp lại ở cả 8 cảnh OMS (S01–S08, cả ảnh RGB lẫn IR-sim), tổng cộng 30 box.

Vì sao mình mắc: …

Lần sau tránh bằng cách: …

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai: `S01_rgb, person, seat` — mình ghi `rear_right` cho người bên phải ảnh, bạn Hồ Xuân Hùng ghi `rear_left` (verdict `peer`, GUIDE 4.4-3). Mình học được gì từ dòng đó: …
