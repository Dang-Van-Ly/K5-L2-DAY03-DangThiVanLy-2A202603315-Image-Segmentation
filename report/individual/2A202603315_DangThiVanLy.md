# Báo cáo thực hành Day 3 — Đặng Thị Vân Ly

- Mã học viên theo lớp: 2A202603315
- Ngày / CVAT local: 11/11/2026 / CVAT local
- Công cụ đã dùng: Brush, Polygon (không dùng SAM)

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Sáu checkpoint chưa export kịp trong buổi lab; dữ liệu chưa được Save trên CVAT. Sẽ hoàn thành và cập nhật trước deadline.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh đầu tiên của task `medium_instance`, phần trung tâm ảnh — một chiếc xe ô tô (car) đứng gần nhất, chiếm khoảng 1/4 chiều rộng ảnh, nằm ở vị trí giữa-trái.
- Class và quy tắc tôi dùng để chọn biên: Chọn class `car`. Vẽ mask theo đường viền phần xe **nhìn thấy được** — dừng ở chỗ xe bị cắt bởi mép ảnh hoặc bị vật khác che. Không vẽ phần bánh xe bị khuất dưới mặt đường; không kéo mask sang thân xe bên cạnh dù hai xe sát nhau.
- Không dùng gợi ý tự động; tự vẽ toàn bộ bằng Brush trong CVAT.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `easy_semantic` — ảnh thứ 2, khu vực ranh giới giữa vỉa hè (sidewalk) và mặt đường (road) ở góc dưới bên phải.
- Lỗi thuộc loại: sai lớp / biên
- Bằng chứng tôi nhìn thấy: Phần bó vỉa (curb) màu xám sáng bị tô nhầm vào class `road` thay vì `sidewalk`. Nhìn kỹ thấy bó vỉa nằm ở rìa hè, chức năng là phân cách hè với đường, nên thuộc `sidewalk`.
- Quy tắc và hành động sửa: Theo quy tắc "ranh road–sidewalk theo chức năng/bó vỉa, không theo màu" — chỉnh lại mask `sidewalk` phủ đến hết bó vỉa, đẩy biên `road` lùi lại. Dùng Brush xóa phần nhầm rồi vẽ lại.
- Sau sửa đã Save và export lại: Đã Save trong CVAT và export lại thành `easy_semantic.zip`.

Chưa chạy script tự đánh giá; chưa có điểm.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `easy_semantic` ảnh 1 — dải phân cách giữa đường | (1) Tô là `road` vì nằm trong mặt đường; (2) Tô là `sidewalk` vì có bề mặt bê tông và chức năng phân cách | Quy tắc nói theo chức năng; dải phân cách không dành cho xe chạy trực tiếp | Quyết định: tô `sidewalk`. Hỏi coach: dải bê tông giữa đường có phải luôn là sidewalk không? |
| `medium_instance` ảnh 2 — người đi bộ bị cắt nửa người ở mép ảnh | (1) Vẫn tạo object `person` cho phần nhìn thấy; (2) Bỏ qua vì quá nhỏ/thiếu | Quy tắc: chỉ vẽ phần nhìn thấy; vật bị cắt vẫn là một instance | Quyết định: tạo object `person` cho phần nhìn thấy dù chỉ còn nửa người. |
| `hard_panoptic` ảnh 1 — tán cây che một phần mái nhà | (1) Vẽ `building` đến mép tán cây; (2) Vẽ `building` ước lượng qua tán cây vì biết mái nhà còn tiếp tục | Quy tắc: không vẽ phần bị che; chỉ vẽ phần nhìn thấy | Quyết định: dừng mask `building` ở ranh nhìn thấy, không đoán phần bị tán cây che. |
