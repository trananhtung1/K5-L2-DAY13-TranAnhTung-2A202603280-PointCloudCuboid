# Báo cáo Day 13 — Trần Anh Tùng, 2A202603280

## 1. Kết quả gán nhãn

Task `D13-2A202603280`, Job 28, gồm 10 frame LiDAR với 7 góc camera cho mỗi frame. Kết quả kiểm tra bản xuất cuối là **92 cuboid**. Frame đầu có 12 hộp; frame ít nhất là `s01f31` với 5 hộp. Cả 10 frame đạt số hộp tối thiểu của lab. Sau lần chỉnh hộp Cyclist, số cuboid có cảnh báo giảm từ 5 xuống 4.

| Sequence | Frame thứ nhất | Số hộp | Frame thứ hai | Số hộp |
|---|---|---:|---|---:|
| 01 | `s01f09` | 12 | `s01f31` | 5 |
| 02 | `s02f16` | 9 | `s02f51` | 6 |
| 03 | `s03f11` | 9 | `s03f56` | 9 |
| 04 | `s04f30` | 12 | `s04f51` | 12 |
| 05 | `s05f14` | 12 | `s05f35` | 6 |

Môi trường thực hành gồm macOS, Chrome và CVAT 2.81.0. Các hộp được vẽ bằng **Shape** trên từng frame. Khung Top dùng để căn tâm, chiều dài, chiều rộng và hướng. Hai khung Side và Front dùng để kiểm tra chiều cao và vị trí đáy. Job 28 có lớp chiếu cuboid lên ảnh camera để đối chiếu khi chỉnh hộp.

## 2. Đọc point cloud và đối chiếu camera

Gốc tọa độ ở tâm LiDAR trên nóc xe. Trong hệ VF, **−y** hướng về trước xe, **+x** sang trái xe, **+y** ra sau và **+z** hướng lên. Khi xoay khung 3D, có thể đối chiếu với cam01 nhìn về trước hoặc cam09 nhìn về sau để xác định hướng của ảnh chụp. [Quy ước tọa độ VF](../../docs/COORDINATES.md)

| Frame đọc kỹ | Ảnh nhìn nghiêng | Ảnh nhìn từ trên xuống | Đối tượng có minh chứng |
|---|---|---|---|
| `s01f09` | `t1-s01f09-3d.png` | `t1-s01f09-top.png` | Xe con và xe tải được chọn làm mẫu ở mục 3. |
| `s01f31` | `t1-s01f31-3d.png` | `t1-s01f31-top.png` | Hai ảnh ghi lại cùng một frame ở hai góc nhìn. |

Camera cung cấp màu sắc, hình dáng bên ngoài và dấu hiệu phân biệt đầu với đuôi xe. LiDAR cung cấp vị trí trong không gian, kích thước theo mét và độ cao so với mặt đường. Trong các frame đã xem, tôi vẫn nhận ra trên point cloud những vật nhìn rõ trên camera, kể cả một số vật ở xa. Hướng cụ thể trên mép từng ảnh, độ dốc, tầm xa nhất và các cụm chưa nhận diện chưa có ghi chép riêng cho từng frame.

## 3. Ba cuboid mẫu

| Vật | Frame | Class trên CVAT | Ảnh minh chứng trong ZIP |
|---|---|---|---|
| Xe con | `s01f09` | `Car` | `t2-a-car.png` |
| Xe tải | `s01f09` | `Truck` | `t2-b-bus-truck.png` |
| Xe hai bánh có người điều khiển | `s04f30` | `Cyclist` | `t2-c-ped-cyclist.png` |

Ở hai mẫu xe, tiêu chí kiểm tra hướng là vạch đỏ dọc thân hộp chỉ về đầu xe. Ở mẫu Cyclist, phạm vi hộp gồm cả người điều khiển và xe. Góc nhìn ngang cho phép kiểm tra đáy hộp sau khi đã căn đúng hình chiếu từ trên xuống. Chưa có bản ghi đối chiếu riêng từng mẫu với bảng kích thước 4.3 hoặc kết quả vòng hiệu chuẩn chung. [Quy tắc cuboid và bảng 4.3](../../docs/GUIDELINE.md)

## 4. Hiện tượng ảnh hưởng đến gán nhãn

Ba ảnh dưới đây đều lấy từ task cá nhân `D13-2A202603280`. Cột cuối ghi cách xử lý theo guideline. Nguyên nhân vật lý của từng hiện tượng chưa được xác minh.

| Hiện tượng | Frame và bằng chứng | Hệ quả khi gán nhãn |
|---|---|---|
| Bóng che | `s03f11` · `t4-1-bong-che.png` | Với vật bị che một phần, dùng thuộc tính occluded và chỉ ước lượng phần khuất khi có đủ căn cứ. |
| Thưa ở xa | `s01f09` · `t4-2-thua-o-xa.png` | Vật ở xa có thể vẫn nhận diện được. Khi các điểm chưa đủ để xác định phạm vi hộp, ghi nhận LOW_VISIBILITY. |
| Phản xạ thấp hoặc ít điểm bất thường | Tôi chưa xác định được ví dụ rõ trong các frame đã xem. Mục này chưa có ảnh minh chứng. | Số điểm còn phụ thuộc khoảng cách và mức độ che khuất. Chưa đủ căn cứ quy phần thiếu điểm cho đặc tính bề mặt. |
| Điểm lạ | `s03f11` · `t4-4-diem-la.png` | Một cụm điểm lẻ cần được đối chiếu ở nhiều góc trước khi xác định có vật thể tại đó. |

## 5. Số đo và kết quả tự kiểm

Bản xuất sau cùng gồm [Bảng A](../../submission/D13-2A202603280-bangA.csv) và [phiếu tự kiểm](../../submission/D13-2A202603280-tukiem.csv). Hai bảng đối chiếu theo cặp `(frame, idx)`. Bảng A lưu kích thước, vị trí và góc hướng của từng hộp; phiếu tự kiểm lưu số điểm bên trong, khoảng hở ước tính với mặt đường và cảnh báo.

**Hộp Cyclist đã sửa:** tại frame `s04f51`, hộp `idx 12` có cảnh báo đáy lơ lửng. Tôi đã kiểm tra và chỉnh lại hộp trong CVAT. Sau khi Save và xuất lại bằng `sync`, phiếu mới không còn cảnh báo ở hộp này. Mẫu Cyclist chụp ở mục 3 thuộc frame `s04f30`.

**Các cảnh báo còn lại:** tôi đã kiểm tra lại toàn bộ các hộp bị cờ. Với những hộp có kích thước hơi vượt khoảng tham khảo, góc nhìn bị che khiến chỉ một phần vật thể có điểm LiDAR. Vì thế biên phía khuất vẫn có sai số khi ước lượng.

Độ cao đáy hộp được tính bằng `z − height/2` (m). Khoảng cách ngang `dist_m` được tính từ x và y; khoảng cách không gian `r` tính thêm z. Phần phân tích cá nhân chưa có bảng đối chiếu ba giá trị đáy, hai hộp gần nhất và xa nhất, kích thước theo bảng 4.3, yaw của xe hai chiều hoặc mật độ ở bốn khoảng cách. Chưa có thông tin nhận diện lần sửa thứ hai hay biên bản kiểm chéo hai số với bạn cùng lớp.

## 6. Ý nghĩa của nhãn và cách dùng AI

Hộp 3D đo bằng mét cho mô hình biết vị trí và kích thước vật thể trong không gian của xe. Với vật thưa điểm, việc kéo dài hộp ra ngoài phần quan sát được có thể đưa sai số vào chính kích thước mà mô hình dùng làm mục tiêu học. Với dữ liệu của mình, phần tôi còn chưa chắc nhất là hộp đã bao hết vật bị che khuất hay chưa.

- **Công cụ:** Codex.
- **Nội dung trao đổi:** thiết lập CVAT, tăng độ sáng điểm LiDAR, chiếu cuboid lên camera và theo dõi các checkpoint.
- **Cách sử dụng:** hỗ trợ hiển thị khi chỉnh hộp, sắp xếp và đặt tên ảnh chụp đã gửi, đồng bộ tệp CSV, biên tập báo cáo từ ghi chú thực hành và chuẩn bị bản nộp.
