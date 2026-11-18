# PROGRESS REPORT — Báo Cáo Tiến Độ Sprint 1 (Product A — 桜水産市場)

**Tên Iteration trên GitHub Project:** `Sprint 1 (Iteration 1)` · **Khoảng thời gian giả lập:** 19/11/2026 – 02/12/2026 (10 ngày làm việc chính thức, 8 giờ/ngày)

**Mục tiêu Sprint 1:** Hoàn thiện nền tảng hạ tầng Cloud/Shim DB (`ISS-01-01`), Xác thực MFA & APPI (`ISS-01-02`, `ISS-01-03`), Phân nhánh tư cách `許可/承認` (`ISS-02-01`, `ISS-02-02`), Lịch chợ JST (`ISS-02-03`), Tiếp nhận Lot (`ISS-03-01`) và Khung dữ liệu bảng bậc thưởng `完納奨励金` (`ISS-05-01`).

## 1. Tổng hợp trạng thái công việc (Tính đến hết ngày 02/12/2026)

| Trạng thái trên Board | Số Issue | Sub-issue | Tổng SWP | Tổng Giờ | Ghi chú chi tiết |
| :--- | :---: | :---: | :---: | :---: | :--- |
| `Done` | 5 | 13 | 48 | 192 | Hoàn thành `ISS-01-01`, `ISS-01-02`, `ISS-02-01`, `ISS-02-03`, `ISS-03-01` (đã pass `verify:security` & smoke test). |
| `In Review` | 2 | 3 | 18 | 72 | `ISS-01-03` (APPI masking & audit log 7 năm — 8 SWP) và `ISS-02-02` (Gỡ tạm dừng rẽ nhánh `許可/承認` — 10 SWP) đang chờ Tech Lead review PR. |
| `In Progress` | 1 | 2 | 10 | 40 | `ISS-05-01` (Thiết kế DB & UI cấu hình bảng tỷ lệ bậc thưởng `完納奨励金` — 10 SWP) hoàn thành 60% backend, đang hoàn thiện màn hình nhập bậc tỷ lệ. |
| `Todo / Backlog (Sprint 1)` | 0 | 0 | 0 | 0 | Các Issue còn lại (18 Issue / 214 SWP) đã được xếp lịch sang Sprint 2 và Sprint 3. |
| **TỔNG SPRINT 1** | **8** | **18** | **76** | **304** | **Tỷ lệ hoàn thành đạt 63,2% (48/76 SWP); nếu tính cả 2 Issue đang In Review đạt 86,8%.** |

## 2. Bảng theo dõi Burndown (Sprint 1: 19/11 – 02/12/2026)

| Ngày làm việc | Ngày thực tế | Kế hoạch lý tưởng (SWP) | Thực tế còn lại (SWP) | Chênh lệch | Sự kiện / Diễn biến chính trong ngày |
| :--- | :---: | :---: | :---: | :---: | :--- |
| Day 0 | 18/11/2026 | 76,0 | 76,0 | 0,0 | Sprint Planning & chốt `PREMISE.md` LAB-5. |
| Day 1 | 19/11/2026 | 68,4 | 76,0 | +7,6 | Khởi tạo AWS IaC & cấu hình GitHub Actions CI/CD. |
| Day 2 | 20/11/2026 | 60,8 | 68,0 | +7,2 | Hoàn thành `ISS-02-01` (Schema phân nhánh tư cách `許可/承認`, -8 SWP). |
| Day 3 | 23/11/2026 | 53,2 | 60,0 | +6,8 | Hoàn thành `ISS-02-03` (Lịch chợ & Job J-01 mở ngày 02:00 JST, -8 SWP). |
| Day 4 | 24/11/2026 | 45,6 | 60,0 | +14,4 | **Chậm nhịp:** Gặp lỗi xung đột connection pool khi cài đặt shim `auth.uid()` (`ISS-01-01`). |
| Day 5 | 25/11/2026 | 38,0 | 48,0 | +10,0 | Xử lý xong shim `auth.uid()` với `set_config(..., true)` theo transaction (`ISS-01-01` Done, -12 SWP). |
| Day 6 | 26/11/2026 | 30,4 | 48,0 | +17,6 | Tập trung code luồng MFA tablet chung (`ISS-01-02`) và tiếp nhận Lot (`ISS-03-01`). |
| Day 7 | 27/11/2026 | 22,8 | 36,0 | +13,2 | Hoàn thành `ISS-01-02` (TOTP MFA & đổi ca nhanh tablet, -12 SWP). |
| Day 8 | 30/11/2026 | 15,2 | 28,0 | +12,8 | Hoàn thành `ISS-03-01` (Tiếp nhận & đánh giá Lot + `lot_qty_movements`, -8 SWP). |
| Day 9 | 01/12/2026 | 7,6 | 28,0 | +20,4 | Đẩy `ISS-01-03` và `ISS-02-02` sang In Review; vướng làm rõ biên bậc lũy tiến `完納奨励金` (`ISS-05-01`). |
| Day 10 | 02/12/2026 | 0,0 | 28,0 | +28,0 | **Kết thúc Sprint 1:** 48 SWP Done, 18 SWP In Review (chờ merge), 10 SWP In Progress chuyển sang đầu Sprint 2. |

**Giải thích nguyên nhân thực tế lệch kế hoạch:**

1. **Độ trễ kỹ thuật tại Day 4–5 (+14,4 SWP):** Chuyển đổi từ Supabase Auth sang shim `auth.uid()` qua biến phiên PostgreSQL gặp rò rỉ context người dùng giữa các request dùng chung connection pool; Tech Lead mất 1,5 ngày chuyển sang khóa theo transaction (`is_local = true`) và chạy lại 14 bài test `verify:security`.
2. **Dồn ứ ở khâu Review cuối Sprint (+18 SWP ở In Review):** `ISS-01-03` (APPI PII masking) và `ISS-02-02` (gỡ tạm dừng rẽ nhánh `条件付き` vs `再審査`) hoàn tất code vào Day 9 nhưng theo quy định Quality Gate bắt buộc Tech Lead review kỹ luật chống loại hồ sơ RFP `12-06` nên chưa kịp merge trước 17:00 ngày 02/12.
3. **Vướng mắc nghiệp vụ đặc thù `完納奨励金` (`ISS-05-01`, 10 SWP):** Do khách hàng chưa gửi file CSV bảng tỷ lệ thực tế (Phụ lục `C.1`, `Q-07`), đội Dev phải xây dựng giao diện nhập động cho phép cấu hình số bậc linh hoạt (1–10 bậc thay vì cố định 5 bậc), phát sinh thêm 12 giờ công.

## 3. Đánh giá & Kế hoạch ứng phó Rủi ro trễ tiến độ

| Rủi ro trễ tiến độ | Issue bị ảnh hưởng | Ảnh hưởng tới mốc nào | Hành động khắc phục & Giảm thiểu | Người phụ trách & Thời hạn |
| :--- | :--- | :--- | :--- | :--- |
| 1. Khách hàng chậm phản hồi `Q-07` & Phụ lục `C.1` (bảng tỷ lệ `完納奨励金`) | `ISS-05-01`, `ISS-05-02`, `ISS-05-05` (`EPIC-05`) | Mốc hoàn thành Engine tính thưởng cuối Sprint 2 (16/12/2026) và Test Vector `C.3`. | Sử dụng bộ dữ liệu giả lập chuẩn hóa theo `ASM-L5-01` để hoàn thiện logic và unit test trong Sprint 2; thiết kế màn hình `ISS-05-01` hỗ trợ import CSV để nạp dữ liệu thật ngay khi khách gửi. | **PL (Kiều)** gửi email nhắc khách chốt `Q-07` trước 04/12/2026; **BE Dev** hoàn thành test giả lập trước 11/12/2026. |
| 2. Tồn đọng review PR tại nút cổ chai Tech Lead (18 SWP đang In Review) | `ISS-01-03`, `ISS-02-02` và các PR của `EPIC-04` | Mốc khởi động Sprint 2 (03/12/2026) — các task giao dịch `相対/せり` phụ thuộc vào `ISS-02-02`. | Ưu tiên dành trọn buổi sáng ngày đầu Sprint 2 để merge 2 PR tồn đọng; áp dụng quy tắc SLA Review ≤ 24 giờ và phân quyền cho Senior BE Dev tham gia review chéo. | **Tech Lead** hoàn tất merge `ISS-01-03` & `ISS-02-02` trước 12:00 ngày 03/12/2026. |
| 3. Khách hàng đổi yêu cầu sang xác nhận giao dịch Offline (`Q-09` Blocking) | `ISS-07-01`, `ISS-07-02` (`EPIC-07`) | Mốc đóng băng tính năng trước UAT (Sprint 3, 30/12/2026). | Giữ vững lập luận kỹ thuật theo `BR-PERM-01` (bắt buộc kiểm tra tư cách thời gian thực tại server); nếu khách vẫn yêu cầu, đề xuất cơ chế "Lưu nháp chờ xác nhận" và lập Change Request (+60–100 MD). | **PL (Kiều) & BrSE** chốt biên bản xác nhận `Q-09` với khách hàng trước 09/12/2026. |
| 4. Sai lệch làm tròn số học & đếm trùng sau điều chỉnh (`reverse/correction`) | `ISS-04-03`, `ISS-05-04`, `ISS-06-02` | Mốc nghiệm thu bộ 12 báo cáo chuẩn & xuất kế toán `IF-ACC-01` (Sprint 3). | Khóa chặt quy ước mọi truy vấn báo cáo chỉ đọc qua view `trades_effective` (`ADR-07`) và snapshot `daily_settlements` (`ADR-06`); QA viết sẵn bộ test đối chiếu tổng kiểm tự động chạy trên CI. | **QA Lead & BE Dev** hoàn thiện bộ test đối soát snapshot trước 18/12/2026. |
