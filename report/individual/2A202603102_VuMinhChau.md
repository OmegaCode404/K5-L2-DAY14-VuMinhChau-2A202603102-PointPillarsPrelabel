# Báo cáo Day 14 — Vũ Minh Chau — <MSSV>

> Sao chép file này thành `report/individual/<MSSV>_<HoTen>.md` (không dấu), ví dụ `2A20260000_NguyenVanAn.md`.

- Mã học viên dùng với `--student`: `<ID>`
- `seed_tag` trong `frames_A.txt`: `<...>`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: tăng `score_thr`.
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Tôi thấy box thừa nhiều; tăng ngưỡng điểm có thể loại bớt các dự đoán yếu, giảm số box cần xóa.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | 47 | 5 | 4 | 21.1 |
| `best_config.json` | 7 | 11 | 2 | 14.1 |

- Tham số đã đổi trong `best_config.json`: `score_thr` từ `0.1` lên `0.5`; các tham số khác giữ mặc định.
- Kết quả leave-one-out:
  s02f33: cấu hình chọn {'score_thr': '0.5'} → 3.6 (tốt nhất có thể 2.9)
  s03f23: cấu hình chọn {'score_thr': '0.3'} → 9.9 (tốt nhất có thể 5.1)
  s05f13: cấu hình chọn {'score_thr': '0.5'} → 5.4 (tốt nhất có thể 4.4)

- Cấu hình không hoàn toàn ổn định trên từng frame: leave-one-out chọn `score_thr=0.3` cho `s03f23` nhưng `0.5` cho hai frame còn lại. Tôi vẫn chọn `0.5` vì có chi phí tổng thấp nhất trên cả tập A (14.1 so với 21.1 của cấu hình gốc), dù số box cần thêm tăng từ 5 lên 11.

## 3. Trước/sau (bước 8)

![so sánh](../../d14_<ID>/compare/<frame>.jpg)

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
