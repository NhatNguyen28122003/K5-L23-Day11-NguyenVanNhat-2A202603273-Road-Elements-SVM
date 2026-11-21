# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên):2A202603291
        Người kiểm (học viên):  2A202603273
Ngày: 20/11/2026                         Phiên bản guideline: Road v1.0   
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 81 

## Tổng hợp theo mã

| Mã | Loại lỗi | Số mục kiểm | Số lỗi xác nhận | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 ảnh | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 6 ảnh | 0* | nặng |
| QC1-03 position | sai thuộc tính / thừa | 13 lane | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 13 lane | **1** | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 13 lane | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 13 lane | 0* | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 16 hình vùng/ranh giới¹ | **3** | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 16 đèn | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 28 box đèn/biển | **2** | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 12 biển | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | **0** (không có ảnh B) | 0 | chính |
| QC1-12 file | lỗi file | 1 ZIP/XML hiện có | 0* | nhẹ / nặng |

¹ QC1-07: 14 `drivable`, 1 `crosswalk`, 1 `road_curb` được đối chiếu.  
\* QC1-02: A05 không có L1/R1; A06 không có R1, nhưng vạch sơn tương ứng không thấy rõ nên **không tự kết luận thiếu vạch** — cần coach xác nhận theo checkpoint. QC1-06: R2 tại A04 nằm cạnh dải đường có hai đường viền trắng, cách hiểu `kind=double` cần xác nhận. QC1-12: XML mở/đọc được; chưa có manifest và file fisheye nên **chưa xác nhận chất lượng bàn giao cuối cùng**. Các điều này không làm thay đổi quyết định Rework đã có vì lỗi chính vượt ngưỡng.

## Danh sách lỗi (mỗi lỗi một dòng)

| # | Mã | Ảnh | Nhãn (label, thứ tự trong XML) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-07 | A01 | `drivable` #4 (`alternative`) | chính | Polygon nằm ở **dải ngoài vạch vàng R2**, kéo về phía lan can/lề phải; vùng này là lề/shoulder, không phải làn thay thế cùng chiều. Đỉnh khoảng `(1280,636)` đến `(1280,702)`. Xóa hoặc chỉnh polygon để không bao lề. |
| 2 | QC1-07 | A02 | `drivable` #2 (`alternative`) | chính | Polygon phía ngoài **vạch trắng liền R1** ở mép phải, khoanh cả dải shoulder sát bãi đất, không phải làn chạy bên cạnh. Điểm gần `(1165,719)` → `(1280,720)`. Xóa phần ngoài vạch hoặc không gán vùng này là `alternative`. |
| 3 | QC1-09 | A03 | `traffic_sign` #2, box `(792.77,310.21)–(829.70,346.19)` | chính | Biển **hình thoi màu cam báo công trường** đang đặt `group=prohibitory`; theo guideline biển công trường cam thuộc **`warning`**. Đổi `group`. |
| 4 | QC1-07 | A05 | `drivable` #1 (`direct`) | chính | Polygon dừng ở khoảng **y=527–546**, trong khi mặt đường xe đi được còn kéo xuống trước đầu xe (gần khoảng y≈600). Vùng gần bị bỏ sót đáng kể; kéo ranh giới polygon xuống sát mép nắp capo, không phủ lên xe. |
| 5 | QC1-04 | A06 | `lane` #1 (`L1`) | chính | **Vẽ ngược chiều:** điểm đầu `(508.89,353.83)` ở xa, điểm cuối `(440.45,416.51)` ở gần hơn. Guideline yêu cầu **gần xe → xa xe**; đảo thứ tự các điểm Polyline. |
| 6 | QC1-09 | A06 | `traffic_sign` #4, box `(561.25,304.53)–(591.08,336.99)` | chính | Biển công trường **hình thoi màu cam** đang đặt `group=prohibitory`; phải là **`warning`**. Đổi `group`. |
## Ca mơ hồ

| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A04 | `lane R2` có `kind=double`, nhưng vùng mép phải là **hai đường trắng bao một dải màu xanh** (có thể là viền làn xe đạp), không phải hai nét sát nhau của một vạch đôi. Giữ `double` hay chỉ lấy nét sát làn xe và để `single`? | Chưa có; **không tính lỗi** trong số 6 lỗi chính |
| A05 | Ảnh đường phố không thấy vạch sơn L1/R1 rõ ràng; có được miễn QC1-02 cho các phía không có vạch sơn không? | Chưa có; **không tự tính lỗi nặng** |
| A06 | Không thấy R1 rõ ràng ở mép phải làn hiện tại; có được miễn QC1-02 không? | Chưa có; **không tự tính lỗi nặng** |

## Quyết định

**Lỗi nặng đã xác nhận:** **0**  
**Lỗi chính đã xác nhận:** **6 / 81 nhãn = 7,41 %**  
**Lỗi nhẹ đã xác nhận:** **0** (trong mẫu A01–A06)  
**Ca chờ lead:** **3** (không ảnh hưởng kết luận vì đã vượt 5%)  
**Quyết định:** **Rework**.

**Lý do (theo luật của phiếu):** Không thấy lỗi nặng chắc chắn trong 6 ảnh kiểm, nhưng **6/81 = 7,41 % > 5 %**. Theo `qc_severity_road_v1.0.md`, chỉ cần tỷ lệ lỗi chính vượt 5% thì Rework.
