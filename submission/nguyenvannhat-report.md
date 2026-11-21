# Báo cáo cá nhân — Ngày 11
Họ tên: Nguyễn Văn Nhất              Mã học viên: 2A202603273  
Phiên bản CVAT (Help → About): v2.74.1 (CVAT chạy local, đã xác nhận với người hướng dẫn)  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 1  
Thời điểm export (ngày giờ trên CVAT): 21/11/2026, manifest tạo lúc 03:45 UTC    Số lần nộp lại sau Rework: 0  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Có | lane, road_curb (polyline); crosswalk, drivable (polygon); traffic_light, traffic_sign (rectangle) |
| Thuộc tính và giá trị | Có | So với `python -m road_lab.taxonomy`: khớp tên, kiểu, giá trị và giá trị mặc định |
| 4 tag ảnh | Có | camera, timeofday, weather, source; mỗi label Tag có thuộc tính select `value` |

Project `d11-road-nguyenvannhat` có 4 task: front (A01–A12), front-extra (A13–A16), fisheye (B01–B08), fisheye-extra (B09–B12).

Cách kết thúc polyline trên bản CVAT này: click lần lượt các điểm từ gần xe ra xa, bấm phím **N** để kết thúc.  
Cách đặt tag ảnh trên bản CVAT này: công cụ **Setup tag** ở thanh bên trái → chọn label (camera trước tiên) → **Tag**; sau đó mở **Details** trong bảng Objects để sửa `value`, vì CVAT tự điền giá trị đầu danh sách.

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 31 | 14 | 8 | 22 | 35 | 32 |
| front-extra (đã làm 0 / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| fisheye | 8 | 10 | 13 | 12 | 9 | 5 | 8 |
| fisheye-extra (đã làm 0 / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Cả 20 ảnh bắt buộc có đủ bốn tag (manifest: `images_all_tags` = 12 và 8).

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A05 | lane | vẽ L1/R1 hay không | không vẽ lane | Không có vạch sơn ở cả hai bên làn xe mình; mép đường gán `road_curb`. Lint báo QC1-02, chờ coach xác nhận |
| A06 | lane | vẽ R1 hay không | không vẽ R1 | Bên phải làn xe mình không có vạch sơn. Chờ coach xác nhận QC1-02 |
| A07 | lane | vẽ L1 hay không | không vẽ L1 | Bên trái làn xe mình không có vạch sơn. Chờ coach xác nhận QC1-02 |
| A08 | lane | gán L1 hay không | không gán L1; các vạch còn lại là R1, R2, `other` | Không có vạch nào sát bên trái làn xe mình. Chờ coach xác nhận QC1-02 |
| A09 | lane | vẽ R1 hay không | không vẽ R1 | Bên phải làn xe mình không có vạch sơn. Chờ coach xác nhận QC1-02 |
| A02, A03 | tag timeofday | `day` / `dusk` | `dusk` | Nắng thấp, bóng đổ dài, ánh sáng chiều muộn |
| A06, A08 | tag weather | `clear` / `overcast` | `overcast` | Trời nhiều mây, xám, mặt đường khô, không mưa |
| A07, A10 | tag weather | `overcast` / `rain` | `rain` | Mặt đường ướt rõ do mưa |
| B03, B04 | lane position | trái/phải theo hướng nhìn của ảnh hay theo hướng xe chạy | theo hướng nhìn của ảnh camera sau | Guideline không nói rõ cho `fisheye_rear`; hỏi lead |
| B01, B02 | lane | vẽ lane hay không | không vẽ lane | Trọng tâm ảnh là crosswalk / đèn, biển; không vẽ vạch không nhìn rõ |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: rồi (phiếu `qc1-front-A01-A06.md`, kiểm A01–A06)  Quyết định nhận được: **Rework** (4 lỗi nặng; lỗi chính 11 / 80 nhãn = 13,75 %)

| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-01 tag (A05 thiếu cả 4 tag) | 4 | Đồng ý | Đã sửa: A05 có đủ 4 tag (front / day / clear / real) |
| QC1-03 position (A04 vạch trắng liền gán L1; A06 vạch dừng gán lane) | 2 | Đồng ý | A06: đã xóa polyline trên vạch dừng. A04: **chưa sửa**, vẫn hai polyline L1 |
| QC1-05 occluded (A03) | 1 | Đồng ý | **Chưa sửa** |
| QC1-06 style, color, kind (A01 L2 `double`; A06 L1 `white`) | 2 | Đồng ý | **Chưa sửa** |
| QC1-08 đèn, màu (A06: đèn nghiêng `red`; đèn đi bộ `off`; box gộp hai đầu đèn) | 3 | Đồng ý | **Chưa sửa** |
| QC1-09 facing, group (A04 biển tên đường `mandatory`; A06 đèn `facing`) | 2 | Đồng ý | **Chưa sửa** |
| QC1-10 bỏ sót biển (A01, A03, A04) | 4 | Đồng ý | **Chưa sửa** |

Phần chưa sửa sẽ làm trong buffer, rồi nộp lại với `--revision 2` (bản cũ cất trong `r1/`).

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| nguyenvannhat-front.zip | Có (`road_check lint` đọc được 12 ảnh) | A01: 8 / 8 | A02: 6 / 6 | Có |
| nguyenvannhat-fisheye.zip | Có (`road_check lint` đọc được 8 ảnh) | B01: 5 / 5 | B02: 7 / 7 | Có |

Số shape trong file không tính 4 tag. `contains_images` = `false` cho cả hai file. Lint lúc tạo manifest: nặng 6 (đều là QC1-02 ở A05–A09, chờ coach xác nhận), chính 13, nhẹ 5.

## 6. Ảnh chụp màn hình
1. Tab Labels của project.(https://drive.google.com/drive/folders/1QS-Ho2Z1eBympSkcOROMTkoitJ76NNQT?usp=sharing)
2. Một ảnh camera trước đã gán đủ (làn, vùng lái, đèn/biển).(https://drive.google.com/drive/folders/1QS-Ho2Z1eBympSkcOROMTkoitJ76NNQT?usp=sharing)
3. Một ảnh fisheye đã gán, phóng to đoạn polyline cong.(https://drive.google.com/drive/folders/1QS-Ho2Z1eBympSkcOROMTkoitJ76NNQT?usp=sharing)
4. Một đoạn XML có <polyline label="lane">.

## 7. Tự đánh giá
Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20: **QC1-11** (fisheye): polyline chỉ có 2–3 điểm nên không bám được cung cong, và `edge_zone` không khớp với vị trí tâm hộp so với 80 % bán kính. Ở Ngày 20 tôi sẽ đặt 6–10 điểm cho mỗi vạch fisheye, dày ở vòng ngoài, và chạy lint ngay sau mỗi ảnh fisheye để kiểm `edge_zone`. Lỗi thứ hai là **QC1-04** ở A08: sáu polyline vẽ từ xa về gần. Cách tránh: luôn click điểm đầu tiên ở đáy ảnh.

Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào: Polyline lane là đích học của model phát hiện làn (row-anchor: mỗi hàng ảnh có một giá trị x của vạch), đầu ra của model này nuôi hệ thống giữ làn (LKA). Ở A06 tôi đã vẽ một polyline trên **vạch dừng nằm ngang** và gán là lane. Nếu giữ lỗi này, model học rằng một vạch ngang trước giao lộ cũng là vạch làn. Ở các hàng ảnh gần vạch dừng, đích x trải gần hết bề ngang ảnh, nên model có thể đoán ra một "làn" cắt ngang đường và LKA nhận sai biên làn ngay trước vạch người đi bộ. Tôi đã xóa polyline này.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
Ảnh A01, vạch R1 (`python -m road_lab.rowanchor nguyenvannhat-front.zip --image A01`):

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi | không có làn | không có làn | 601.3 | 543.7 | 484.7 |
| x của bạn kia |  |  |  |  |  |

Trả lời 3 câu:

1. Polyline R1 của tôi đi từ y ≈ 676 (gần xe) lên y ≈ 524 (xa), nên hàng 720 và 680 đã là "không có làn". Nếu dừng sớm 40 px, điểm cuối nằm ở y ≈ 564, và hàng **560** đổi thành "không có làn".
2. (Điền sau khi so với bạn được kiểm.) Lệch ở hàng 720, sát xe, ảnh hưởng LKA nhiều hơn, vì đó là vị trí biên làn ngay trước xe mà bộ điều khiển dùng để giữ xe giữa làn.
3. Không đổi với vạch chỉ cắt mỗi hàng một lần: `x_at_y` nội suy x từ đoạn thẳng cắt hàng đó, không phụ thuộc thứ tự điểm. Script gặp khó là những chỗ giả định điểm đầu là điểm sát xe, như kiểm tra chiều vẽ trong `road_check lint` (QC1-04) và bước lấy điểm đầu làm điểm neo.
