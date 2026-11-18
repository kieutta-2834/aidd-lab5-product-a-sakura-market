# ESTIMATE — Ước lượng SWP, Giờ làm việc & Đối chiếu LAB-1

## 1. Báo cáo HTML của Skill chấm SWP

- **Đường dẫn tệp trong repository:** `reports/swp-evaluation-report.html`
- **Phương pháp chấm của Skill:** Đánh giá từng Issue theo ma trận 2 chiều `Scope (S1–S4) × Complexity (C1–C4) → SWP` (thang điểm Fibonacci chuẩn hóa), sau đó đối chiếu qua bước rà soát của Project Leader (PL) trước khi ghi lên GitHub Project.

## 2. Kết quả chấm SWP của Skill & Hiệu chỉnh của PL (Các Issue mẫu trọng tâm)

| Issue | Tên hạng mục | Skill chấm (Scope × Complexity → SWP) | PL chốt (SWP) | Lý do giữ nguyên hoặc điều chỉnh |
| :--- | :--- | :--- | :--- | :--- |
| `ISS-01-01` | Hạ tầng IaC (AWS Tokyo), PostgreSQL PITR & Shim `auth.uid()` (`ADR-01`) | `S3 (Multi-component) × C3 (High) → 21 SWP` | **20 SWP** (tách 3 sub-issue: 8 + 8 + 4) | Tách thành 3 sub-issue ≤ 8 SWP để tuân thủ ngưỡng độ hạt LAB-5; giảm 1 SWP tổng do tận dụng được toàn bộ schema SQL và 9 RPC sẵn có từ LAB-2. |
| `ISS-02-02` | RPC & UI Gỡ tạm dừng rẽ nhánh (`条件付き` vs `再審査`) (`SCR-04, 05`) | `S2 (Module) × C2 (Medium) → 8 SWP` | **10 SWP** (tách 2 sub-issue: 5 + 5) | **Tăng +2 SWP:** Skill đánh giá thấp phần rẽ nhánh điều kiện kiểm tra (`suspension_release_conditions` vs `qualification_reviews` có `maker ≠ checker`) và ràng buộc chống loại hồ sơ RFP `12-06`. |
| `ISS-03-02` | Giao dịch `相対取引` — kiểm tra đồng thời tư cách, hạn mức & tồn kho (`SCR-10`) | `S2 (Module) × C3 (High) → 8 SWP` | **8 SWP** (giữ nguyên) | **Giữ nguyên:** Độ phức tạp xử lý khóa dòng (`SELECT ... FOR UPDATE`) đã được chứng minh một phần ở prototype LAB-2, mức 8 SWP (32 giờ) là sát thực tế. |
| `ISS-04-01` | Khóa sổ `業務日` (`SCR-17`) — RPC sinh snapshot bất biến (`ADR-06`) | `S2 (Module) × C3 (High) → 8 SWP` | **10 SWP** (tách 2 sub-issue: 5 + 5) | **Tăng +2 SWP:** Cần viết thêm cơ chế đối chiếu tổng kiểm (checksum) ngay trong transaction khóa sổ và xử lý trường hợp giao bù sau khóa sổ (`G18`). |
| `ISS-04-03` | Luồng duyệt điều chỉnh sau khóa sổ (`SCR-13, 14`) — sinh `reverse + correction` (`ADR-07`) | `S3 (Multi-component) × C4 (Very High) → 13 SWP` | **16 SWP** (tách 3 sub-issue: 6 + 6 + 4) | **Tăng +3 SWP:** Ngoài sinh bút toán đảo và mới, phải xử lý nghiệp vụ đổi người mua (ảnh hưởng 2 dòng `settlement_lines` và hạn mức `参加上限`) và tạo view `trades_effective` tránh đếm trùng báo cáo. |
| `ISS-05-02` | Đối chiếu thanh toán đủ hạn (`消込`) & Engine tính `完納奨励金` (`F-18`) | `S2 (Module) × C3 (High) → 8 SWP` | **13 SWP** (tách 3 sub-issue: 5 + 5 + 3) | **Tăng +5 SWP:** Skill chỉ tính công thức nhân tỷ lệ mà bỏ sót logic đối chiếu thanh toán đủ hạn (`消込`), kiểm tra nợ quá hạn liên kỳ và làm tròn xuống sau thuế `floor(× tỷ lệ × 1.10)`. |
| `ISS-05-04` | Xử lý chênh lệch `完納奨励金` kỳ sau (`delta`) khi có sửa đổi sau khóa (`BR-INC-01`) | `S2 (Module) × C2 (Medium) → 5 SWP` | **8 SWP** (tách 2 sub-issue: 5 + 3) | **Tăng +3 SWP:** Logic truy vết ngược từ `settlement_adjustments` về kỳ tính thưởng gốc để sinh bản ghi `result_type = 'delta'` ở kỳ sau có nhiều trường hợp biên khi đổi bậc thưởng. |
| `ISS-06-02` | Bộ 12 báo cáo chuẩn (`RPT-01..12`) & Batch J-04 trước 11:00 JST (`SCR-23`) | `S4 (System-wide) × C3 (High) → 34 SWP` | **34 SWP** (tách 6 sub-issue: 6+6+6+6+5+5) | Giữ nguyên tổng điểm, nhưng bắt buộc chia thành 6 sub-issue nhỏ (mỗi sub-issue 2 báo cáo + batch) để đảm bảo mỗi task ≤ 8 SWP. |
| `ISS-07-01` | PWA Outbox Offline cho tiếp nhận Lot & Giao nhận (`SCR-07, 16`, `ADR-05`) | `S3 (Multi-component) × C3 (High) → 13 SWP` | **13 SWP** (tách 2 sub-issue: 8 + 5) | **Giữ nguyên:** Phạm vi đã được khóa chặt ở 2 tác vụ ghi đơn giản theo `ASM-L5-03`, mức 13 SWP (52 giờ) đủ cho Service Worker, IndexedDB và cấp mã tạm `仮番号`. |

## 3. Quy tắc quy đổi SWP ra Giờ làm việc

1. **Phương pháp lựa chọn:** Áp dụng quy tắc quy đổi tuyến tính cố định **`1 SWP = 4 giờ làm việc`** (tương đương `0,5 Man-Day`), kết hợp rà soát trực tiếp trên từng Sub-issue.
2. **Lý do lựa chọn:** Trong báo cáo LAB-1 (`LAB1_Quick-Report_ProductA.md §3`), tài liệu phương pháp của `tkm:estimate` định nghĩa chuẩn `1 SP ≈ 0,5 MD` (tức 4 giờ chuẩn công ty 8h/ngày). Việc dùng tỷ lệ `1 SWP = 4 giờ` giúp số liệu LAB-5 liên thông trực tiếp và đối chiếu 1-1 được với baseline `1.206,5 MD` của LAB-1.
3. **Phạm vi bao phủ của số giờ ước lượng (`Est. Hours`):** Số giờ quy đổi trên từng Issue đã bao gồm trọn gói công sức thực thi kỹ thuật theo đúng bộ hệ số chuẩn của công ty ở LAB-1 (`Coding/Implement + Unit Test 0.7 + Fixbug nội bộ 0.3 + Quản lý/Review 0.2`), chưa bao gồm quỹ dự phòng rủi ro tổng thể (Buffer 30%).

## 4. Tổng hợp ước lượng theo từng Epic

| Mã Epic | Tên Epic | Số Issue | Sub-issue | Tổng SWP | Tổng Giờ | Man-Day (8h/MD) | Ghi chú |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| `EPIC-01` | Hạ tầng Cloud, Bảo mật, Phân quyền & Audit | 4 | 10 | 48 | 192 | 24,0 MD | Bao gồm shim `auth.uid()`, MFA TOTP, APPI masking & Scheduler JST |
| `EPIC-02` | Quản lý Người tham gia & Phân nhánh Tư cách (`許可/承認`) | 3 | 7 | 32 | 128 | 16,0 MD | Tách luồng gỡ tạm dừng `条件付き` và `再審査` (`ADR-03`) |
| `EPIC-03` | Tiếp nhận Lot, Giao dịch (`相対/せり`) & Giao nhận | 4 | 11 | 46 | 184 | 23,0 MD | Tận dụng 9 RPC cốt lõi từ LAB-2, bổ sung ledger `lot_qty_movements` |
| `EPIC-04` | Khóa sổ ngày (Snapshot), Version Quy tắc & Điều chỉnh sau khóa | 4 | 10 | 44 | 176 | 22,0 MD | Bao gồm `daily_settlements`, `rule_versions` và luồng `reverse/correction` |
| `EPIC-05` | Nghiệp vụ Tiền thưởng Thanh toán Đủ hạn (`完納奨励金`) | 5 | 11 | 42 | 168 | 21,0 MD | Hạng mục đặc thù tách riêng (bảng bậc, đối chiếu `消込`, batch J-05, delta kỳ sau) |
| `EPIC-06` | Hệ thống Thông báo, 12 Báo cáo Chuẩn & Xuất CSV (`IF-ACC-01`) | 3 | 9 | 48 | 192 | 24,0 MD | Xuất CSV giai đoạn 1 theo `ADR-13` (`ASM-L5-04`) |
| `EPIC-07` | Đồng bộ Offline (PWA Outbox) & Chuyển đổi Dữ liệu Lịch sử | 3 | 6 | 30 | 120 | 15,0 MD | Giới hạn 2 tác vụ offline (`SCR-07, 16`) & chuyển đổi 13 tháng lịch sử |
| **TỔNG CỘNG** | **Giai đoạn Implementation & Unit Test cốt lõi** | **26** | **64** | **290** | **1.160** | **145,0 MD** | **Tổng quy mô toàn vòng đời dự án đạt 1.195,0 MD (9.560 giờ).** |

## 5. Đối chiếu Estimate giữa LAB-1 và LAB-5

| Phần (Nhóm chức năng / Epic) | LAB-1 (MD) | LAB-5 (MD) | Chênh lệch | Lý do chênh lệch (trỏ nguồn `PREMISE.md` / `ADR`) |
| :--- | :---: | :---: | :---: | :--- |
| 1. Hạ tầng, Bảo mật, Auth/MFA & Audit (`EPIC-01`) | 182,0 MD | 196,5 MD | **+14,5 MD** | Tăng do chốt phương án `ADR-01 (B)` chuyển sang Cloud riêng của khách (AWS Tokyo), phải viết lớp shim `auth.uid()`, IaC 3 môi trường và log truy cập PII theo `ADR-12` (`PREMISE.md §1` dòng 1). |
| 2. Quản lý Người tham gia, Tư cách & Lịch chợ (`EPIC-02`) | 148,5 MD | 145,0 MD | **-3,5 MD** | Giảm nhẹ do `ADR-03 (A)` dùng bảng chung + bảng con 1-1 thay vì tách 4 bảng độc lập, tái sử dụng hàm `assert_tradable` từ LAB-2 (`PREMISE.md §3` dòng 3). |
| 3. Nghiệp vụ cốt lõi: Lot, Giao dịch & Giao nhận (`EPIC-03`) | 295,0 MD | 278,0 MD | **-17,0 MD** | Giảm do prototype LAB-2 đã hoàn thiện và kiểm thử thành công 9 RPC cốt lõi cùng luồng giao diện cơ bản cho 15 màn hình (`PREMISE.md` LAB-4 §2). |
| 4. Khóa sổ ngày, Version Quy tắc & Điều chỉnh sau khóa (`EPIC-04`) | 198,0 MD | 204,5 MD | **+6,5 MD** | Tăng do `ADR-04 (B)` bổ sung các bảng tham số có kiểu dữ liệu tường minh, đồng thời bổ sung cơ chế khóa sổ snapshot bất biến `ADR-06` và luồng `reverse/correction` `ADR-07` (`PREMISE.md §1` dòng 2). |
| 5. Nghiệp vụ Tiền thưởng Thanh toán Đủ hạn `完納奨励金` (`EPIC-05`) | 124,0 MD | 131,0 MD | **+7,0 MD** | Tăng do tách riêng thành Epic độc lập theo ràng buộc đặc thù Product A, bổ sung logic đối chiếu thanh toán đủ hạn (`消込`), sinh bút toán chênh lệch `delta` kỳ sau và bộ test vector `C.3` (`PREMISE.md §2 ASM-L5-01`). |
| 6. Thông báo, 12 Báo cáo Chuẩn & Xuất Kế toán `IF-ACC-01` (`EPIC-06`) | 142,0 MD | 126,0 MD | **-16,0 MD** | Giảm do chốt `ADR-13 (A)` triển khai xuất file CSV chuẩn trong Phase 1 thay vì xây dựng kết nối API/SFTP hai chiều phức tạp (`PREMISE.md §2 ASM-L5-04`). |
| 7. Đồng bộ Offline PWA & Chuyển đổi Dữ liệu Lịch sử (`EPIC-07`) | 117,0 MD | 114,0 MD | **-3,0 MD** | Giảm nhẹ do khóa chặt phạm vi offline chỉ ở 2 màn hình ghi nhận `SCR-07` và `SCR-16`, loại trừ xác nhận giao dịch (`PREMISE.md §2 ASM-L5-03`). |
| **TỔNG QUY MÔ DỰ ÁN (Toàn vòng đời + Buffer)** | **1.206,5 MD** | **1.195,0 MD** | **-11,5 MD** | **Chênh lệch giảm 0,95% so với báo giá LAB-1 — nằm trọn trong ngưỡng an toàn cam kết với khách hàng.** |
