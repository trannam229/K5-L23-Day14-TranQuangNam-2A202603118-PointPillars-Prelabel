# Báo cáo Day 14 — Trần Quang Nam — 2A202603118

- Mã học viên dùng với `--student`: `118`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: tổ hợp `score_thr=0.4` + `min_points=5`.
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Cấu hình gốc trên tập A cần xóa 55, thêm 4, sửa 0 (chi phí 20.5). Riêng phần xóa đã là 0.3 × 55 = 16.5, khoảng 80% chi phí, nên giảm box thừa là cách giảm công sửa nhanh nhất. Trong ảnh png_A, phần lớn box thừa có score thấp (Cyclist trong bụi cây, Car trên hàng rào cây), còn xe thật rõ ràng thường có score cao, nên tăng `score_thr` lên 0.4 sẽ loại được nhiều box thừa nhất. `min_points=5` bổ sung để bỏ các box gần như trống (trên mặt đường trống, ở xa nơi LiDAR thưa) mà vẫn có score khá. Rủi ro: một số xe thật ở xa hoặc bị che có score dưới 0.4 sẽ bị lọc mất, mỗi xe mất tốn 1.0, đắt hơn 3 lần một lần xóa; còn box thừa nằm trong bụi cây mà score cao thì cả hai ngưỡng đều không loại được.
- Ghi chú: câu chữ mục này được trợ lý AI chỉnh theo nháp và lựa chọn tham số của tôi.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Công cụ hỗ trợ sửa tập A (khai báo theo RULES): lượt sửa đầu do trợ lý AI (Claude Code) làm bằng script phân tích LiDAR + chiếu box lên camera (thư mục `day14-qc`, ngoài repo): xóa 50 box thừa, thêm 4 box sót (2 Bus, 1 Car, 1 Cyclist), đặt đáy hộp sát mặt đường. Tôi đã kiểm lại từng frame trên CVAT trước khi xuất `ref_A.json`.
- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_118/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_<ID>/frames_B.txt --config d14_<ID>/best_config.json --gt d14_<ID>/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
