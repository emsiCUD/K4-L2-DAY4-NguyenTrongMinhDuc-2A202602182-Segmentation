# Báo cáo Day 5


- Mã học viên theo lớp: 2A202602182
- Ngày / CVAT local: 2026-09-17 / CVAT local
- Công cụ đã dùng: Brush, Polygon 

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Tất cả 9 ZIP đã qua kiểm cấu trúc bằng `scripts/inspect_submissions.py` — không lỗi cấu trúc (`OK` toàn bộ).

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000373353.jpg` — person đi bộ gần phía bên phải ảnh (nhóm người qua đường cạnh taxi vàng).
- Class và quy tắc tôi dùng để chọn biên: `person` — vẽ theo đúng phần cơ thể nhìn thấy, dừng mask ở mép quần áo/tay chân, không đoán phần bị người đi cùng hoặc xe che khuất.
- Nếu dùng gợi ý sau đó: không dùng.
- Nếu không dùng gợi ý: "không dùng" — quyết định gán nhãn dựa trên quy tắc chung "vẽ sát phần nhìn thấy" của phiếu quy tắc.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `hard_panoptic`, khả năng cao ở `000000460147.jpg` (ảnh xe tải chở nhiều xe con chồng lên nhau — nơi class `car` có PQ thấp nhất 0.865, 8 FP theo scorer).
- Lỗi thuộc loại: biên (tô tràn) và thiếu-thừa vật (thiếu object).
- Bằng chứng tôi nhìn thấy: mask một số xe tràn ra ngoài thân xe/nền xung quanh; có vật chưa được vẽ khi soát lại panel Objects.
- Quy tắc và hành động sửa: chỉnh lại biên mask sát thân xe theo phần nhìn thấy (dùng Eraser/Brush sửa vùng tràn), vẽ thêm object còn thiếu.
- Sau sửa đã Save và export lại chưa? Đã Save, export lại và up bản `hard_panoptic.zip` hiện tại (bản đã kiểm OK, scorecard 30/30, PQ 0.982) — tức bản đang nộp đã là bản sau khi sửa.

Kết quả tự đánh giá đã chạy qua `scoring/score.py` + `scorecard.py` với gold do coach cung cấp (`tiers_gt.zip`), scorecard ba tier: **79.9 / 82** (easy_semantic 17.9/20, medium_instance 32.0/32, hard_panoptic 30.0/30). Chi tiết: easy_semantic yếu nhất ở `building` (IoU 0.652) và `sidewalk` (IoU 0.687); hard_panoptic có PQ thấp nhất ở `car` (0.865, 8 FP) kèm cờ REVIEW_HIGH_AGREEMENT (metric 0.982 ≥ 0.95, chỉ là tín hiệu cần coach xem lại, không phải kết luận gian lận). Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth (`tiers_gt.zip`, thư mục `groundtruth/`) vào fork — đã kiểm `.gitignore` chặn cả hai.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` — `817bca71-00000000.jpg`, góc dưới bên trái (ranh building/sidewalk, khu vực có bụi cây và garage) | (a) tính ranh theo mép tường/móng nhà; (b) tính ranh theo mép ngoài lối đi/bậc thềm lát đá | IoU class này thấp nhất trong Easy (building 0.652, sidewalk 0.687) — dấu hiệu cho thấy đây đúng là điểm còn lệch | Câu hỏi cho coach: ở góc này, ranh building/sidewalk nên tính theo mép tường nhà hay theo mép lối đi lát đá? |
| 2. `hard_panoptic` — `000000460147.jpg`, xe tải chở nhiều xe con chồng lên nhau | (a) tách từng xe con thành instance `car` riêng dù bị xe khác che một phần; (b) gộp thành một vùng vì các xe dính sát nhau, khó xác định ranh từng xe | class `car` có PQ thấp nhất (0.865) và 8 FP nhiều nhất trong 12 class — khớp đúng vùng này | Câu hỏi cho coach: các xe con chồng lên xe tải nên tách riêng từng xe thành instance hay gộp chung vì quá sát nhau khó xác định ranh chính xác? |
| 3. `medium_instance` — `000000181542.jpg`, người ngồi trong xe buýt nhìn qua cửa kính | (a) vẫn tính là `person` riêng vì nhìn thấy hình dạng người qua kính; (b) không tô vì ảnh mờ/qua lớp kính phản chiếu, không chắc chắn đó là người hay chỉ là bóng/phản chiếu | Ảnh mờ (motion blur) và có lớp kính xe che nên khó xác định ranh giới chính xác của người bên trong | Câu hỏi cho coach: người ngồi trong xe buýt nhìn qua cửa kính, ảnh bị mờ — có nên tính là 1 instance `person` riêng không, hay bỏ qua vì không đủ rõ để xác định ranh giới? |
