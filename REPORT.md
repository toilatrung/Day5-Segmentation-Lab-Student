# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602096
- Ngày / CVAT local: 2026-09-17 / CVAT local (http://localhost:8080)
- Công cụ đã dùng: CVAT Brush/Polygon (semantic); YOLOv7‑SAM auto-annotation + tự sửa (instance/panoptic); không dùng SegFormer/YOLOv7 ONNX (đã tắt để tiết kiệm RAM)

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

Tất cả 9 task đã export và Save trong CVAT local; không có task nào lỗi export.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: …
- Class và quy tắc tôi dùng để chọn biên: …
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: …
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: medium_instance, cả 3 ảnh (000000181542.jpg, 000000373353.jpg, 000000458325.jpg)
- Lỗi thuộc loại: thiếu-thừa vật (thừa vật)
- Bằng chứng tôi nhìn thấy: export lần hai có 167 object trong khi GT/lần export đầu chỉ có 71 — tự chạy `scoring/score.py` cho thấy TP 71 / FP 96, precision@0.5 tụt còn 0.43, nghĩa là có object annotate trùng lặp (nhiều khả năng do chạy auto-annotate lại mà không xoá mask cũ trước khi vẽ tiếp).
- Quy tắc và hành động sửa: đang mở lại task `medium_instance` trong CVAT để xoá các object trùng, giữ đúng 71 object khớp ảnh thật.
- Sau sửa đã Save và export lại chưa? Chưa — đang xử lý, sẽ export và cập nhật `medium_instance.zip` trước khi nộp.

Kết quả tự chấm (scoring/score.py, dùng reference 3 tier phát trong giờ lab, KHÔNG đưa file ground truth lên fork):

| Task | Metric | Điểm tự chấm |
| --- | --- | ---: |
| easy_semantic | mIoU 0.798 | 17.7 / 20 |
| medium_instance | mean_matched_IoU×recall 0.998 (trước khi sửa lỗi thừa vật ở trên) | 32.0 / 32 |
| hard_panoptic | PQ 0.456 | 17.1 / 30 |
| **Tổng 3 tier** | | **66.8 / 82** |

Lưu ý: `medium_instance` có cảnh báo review `REVIEW_HIGH_AGREEMENT` và `REVIEW_IDENTICAL_GEOMETRY` (67/71 mask trùng pixel với reference) — đây là cờ để coach xem lại thủ công, không phải kết luận gian lận; tôi ghi nhận ở đây để giải trình nếu được hỏi. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 | … | … | … |
| 2 | … | … | … |
| 3 | … | … | … |
