# Báo cáo Day 13 — Trần Anh Tùng, 2A202603280

## 1. Kết luận trước

Task `D13-2A202603280`, Job 28. Môi trường thực hành gồm macOS, Google Chrome và CVAT 2.81.0.

Các frame theo thứ tự trong CVAT (0–9): `s01f09`, `s01f31`, `s02f16`, `s02f51`, `s03f11`, `s03f56`, `s04f30`, `s04f51`, `s05f14`, `s05f35`.

TODO(học viên): MSSV người ngồi cạnh; tổng bao nhiêu cuboid, frame nào ít nhất và vì sao; `selfcheck` cờ bao nhiêu cuboid trên tổng số; bạn tin phần nào nhất và phần nào còn yếu nhất (hướng, đáy, kích thước, class, vật xa).

Trong khung 3D, kéo chuột trái để xoay, kéo chuột phải để dời và cuộn để phóng to vùng cần xem.
Để đặt cuboid, chọn lớp và Shape, nháy đúp vào cụm điểm rồi chỉnh tâm và hướng ở Top, đáy và chiều cao ở Side và Front.
Bật Cuboid orientation để kiểm tra hướng. Job 28 có lớp chiếu cuboid trực tiếp lên camera; lệnh `preview` của lab dùng để xem lại hộp đã lưu.

## 2. Đọc frame (Bước 2)

Ảnh của hai frame đọc kỹ: `t1-s01f09-3d.png`, `t1-s01f09-top.png`, `t1-s01f31-3d.png`, `t1-s01f31-top.png`.

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| `s01f09` | TODO(học viên) | | | | | |
| `s01f31` | TODO(học viên) | | | | | |

TODO(học viên): 3 điều **chỉ camera** cho biết và 3 điều **chỉ LiDAR** cho biết, lấy từ chính hai frame trên.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| `s02f16` | TODO(học viên) | |
| `s02f51` | TODO(học viên) | |
| `s03f11` | TODO(học viên) | |
| `s03f56` | TODO(học viên) | |
| `s04f30` | TODO(học viên) | |
| `s04f51` | TODO(học viên) | |
| `s05f14` | TODO(học viên) | |
| `s05f35` | TODO(học viên) | |

## 3. Ba cuboid có hướng dẫn (Bước 3)

Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | `s01f09`, idx TODO(học viên) | Car | | | | |
| (b) xe buýt hoặc xe tải | `s01f09`, idx TODO(học viên) | Truck | | | | |
| (c) người đi bộ hoặc xe hai bánh | `s04f30`, idx TODO(học viên) | Cyclist | | | | |

TODO(học viên): vòng hiệu chuẩn: cuboid của bạn lệch so với các bạn ở điểm nào, cả lớp chốt cách hiểu guideline nào? Và các vật bạn **bỏ qua vì không đủ cơ sở** (frame, vị trí, mô tả, loại nghi vấn như UNCERTAIN_CLASS / LOW_VISIBILITY), hoặc ghi "không có".

## 4. Bốn hiện tượng chất lượng (Bước 4)

Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | `s03f11` · task cá nhân D13-2A202603280 · `t4-1-bong-che.png` | | | |
| 2. Thưa ở xa | `s01f09` · task cá nhân D13-2A202603280 · `t4-2-thua-o-xa.png` | | | |
| 3. Phản xạ thấp / ít điểm bất thường | Chưa có ảnh minh chứng | | Trong các frame đã xem, các vật thấy rõ trên camera vẫn nhận ra được trên point cloud, kể cả ở xa. | |
| 4. Điểm lạ | `s03f11` · task cá nhân D13-2A202603280 · `t4-4-diem-la.png` | | | |

## 5. Số từ cuboid của tôi (Bước 5)

Lấy từ `submission/D13-2A202603280-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| TODO(học viên) | | | | | | | |
| TODO(học viên) | | | | | | | |
| TODO(học viên) | | | | | | | |

TODO(học viên): (a) cuboid **gần nhất** và **xa nhất**: `dist_m` và r của từng cái. (b) Kích thước l, w, h của một người đi bộ (hoặc xe hai bánh) và một xe con so với bảng 4.3: chiều nào ngoài khoảng, vì sao. (c) `yaw_deg` của một xe cùng chiều phía trước và một xe ngược chiều (nếu có): có gần 270° và 90° không.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| TODO(học viên) | | |

TODO(học viên): một câu quy luật ("xa gấp … lần thì số điểm còn khoảng …") và so với đường cong ở slide bài giảng (xe con: khoảng 1968 điểm ở 0–10 m, 718 ở 10–20 m, 214 ở 20–30 m, 72 ở 30–40 m, 19 ở 60–80 m).

**Kết quả tự kiểm trên CVAT**

Tôi đã kiểm tra lại tất cả các cảnh báo CP7. Với các hộp có kích thước hơi vượt khoảng tham khảo, góc nhìn bị che khiến tôi chỉ thấy một phần vật thể trên point cloud. Tôi phải ước lượng phần bị khuất nên chưa chắc hộp đã bao hết vật.

Tôi đã kiểm tra và chỉnh lại hộp Cyclist ở frame `s04f51` (idx 12 trong phiếu trước khi sửa) sau cảnh báo đáy hộp lơ lửng.

TODO(học viên): ghi frame, idx và thay đổi của cuboid thứ hai nếu đã sửa.

TODO(học viên): đổi kiểm: MSSV người đã kiểm chéo với bạn, hai con số nào được đối chiếu với bảng A, khớp hay lệch (lệch thì đã sửa gì).

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

TODO(học viên): giải thích việc dùng mét cho nhãn 3D và cách xử lý vật thưa điểm từ góc nhìn của mô hình.

- Công cụ AI đã dùng: Codex.
- Nội dung trao đổi: thiết lập CVAT, tăng độ sáng điểm LiDAR, chiếu cuboid lên camera và theo dõi các checkpoint.
- Cách dùng kết quả: áp dụng lớp hiển thị trực tiếp khi chỉnh cuboid, gửi ảnh chụp để hỗ trợ đặt tên và sắp xếp, chuẩn bị tệp nộp bài.

TODO(học viên): điều còn chưa chắc hoặc lỗi phát hiện sau hạn, nếu có.
