# README — LAB-5: Break Task, GitHub Project & Báo Cáo Tiến Độ (Product A — 桜水産市場)

**Tên dự án:** Hệ thống hỗ trợ nghiệp vụ chợ đầu mối thủy sản — 桜水産市場 (Sakura Suisan Ichiba) · **Mã học viên:** 2834 · **Đề:** Product A (Sàn đấu giá / bán buôn thủy sản)

**Người thực hiện (PL):** Tào Thị Ánh Kiều (tiếp nối xuyên suốt từ LAB-1 đến LAB-4)

## 1. Tổng quan

- **Nguồn cơ sở (Baseline LAB-4):** Toàn bộ backlog được phân rã trực tiếp từ gói thiết kế LAB-4 (`ProductA_specs`, ngày nộp: 18/11/2026), kết hợp đối chiếu số liệu ước lượng từ LAB-1 (`ProductA_桜水産市場_Effort Estimation_20261012.xlsx`, tổng 1.206,5 MD).
- **Trạng thái `PREMISE.md`:** Có thay đổi và bổ sung quan trọng so với LAB-4 — Project Leader (PL) đã chính thức chốt phê duyệt trạng thái `Accepted` cho 13 quyết định kiến trúc (`ADR-01..ADR-13` vốn đang ở trạng thái `Proposed` tại LAB-4), sửa cấu trúc bảng tham số `rule_parameters` sang bảng có kiểu dữ liệu tường minh (`incentive_rate_tiers`) để phục vụ tính toán `完納奨励金`, và bổ sung 5 giả định kỹ thuật (`ASM-L5-01..05`) lấp các điểm chờ Q&A khách hàng.
- **Quy mô phân rã:** 7 Epic → 26 Issue chính → 64 Sub-issue (tổng 290 SWP / 1.160 giờ cho giai đoạn Implementation & Unit Test cốt lõi; tương đương 1.195,0 MD toàn vòng đời dự án khi nhân hệ số chuẩn công ty).
- **Sprint giả lập đã chạy:** Sprint 1 (`Iteration 1`: 19/11/2026 – 02/12/2026, 10 ngày làm việc) tập trung vào hạ tầng Cloud/Auth/APPI, phân nhánh tư cách `許可/承認`, luồng tiếp nhận Lot và khung dữ liệu bảng bậc thưởng `完納奨励金`.

## 2. Liên kết truy cập (Public Access)

- **GitHub Repository (Public):** https://github.com/kieutta-2834/aidd-lab5-product-a-sakura-market
- **GitHub Project Board (Public):** https://github.com/users/kieutta-2834/projects/2
- **Đường dẫn báo cáo HTML trong repo:** `reports/swp-evaluation-report.html`
- **Xác nhận quyền truy cập:** Đã kiểm tra mở thành công cả đường dẫn Repository, GitHub Project #2 (xem được đầy đủ Board/Table, các cột `SWP`, `Est. Hours`, `Iteration`, `Assignee`, `Sub-issues`) và các tệp Markdown trên cửa sổ trình duyệt ẩn danh (Incognito) mà không cần đăng nhập tài khoản GitHub.

## 3. Cấu trúc Repository

| File / Thư mục | Nội dung |
| :--- | :--- |
| `README.md` | Tài liệu tổng quan dự án, liên kết công khai, cấu trúc thư mục và bản đồ ánh xạ Spec → Epic. |
| `PREMISE.md` | Các thay đổi thiết kế, giả định bổ sung và phương án chốt 13 ADR của PL so với bản nộp LAB-4 (18/11/2026). |
| `task-breakdown.md` | Nguyên tắc chia task (ngưỡng ≤ 8 SWP), ánh xạ Spec → Issue/Sub-issue, tách riêng hạng mục đặc thù `完納奨励金` và xử lý lỗ hổng spec LAB-4. |
| `estimate.md` | Đối chiếu kết quả chấm SWP của skill với quyết định chốt của PL, quy tắc quy đổi 1 SWP = 4h, tổng hợp theo Epic và so sánh chênh lệch với LAB-1. |
| `progress-report.md` | Báo cáo tiến độ vận hành giả lập Sprint 1, bảng trạng thái issue, bảng Burndown thực tế vs kế hoạch và phân tích rủi ro trễ tiến độ. |
| `AI-report.md` | Báo cáo chi tiết quy trình sử dụng AI/Skill, cách PL review & chỉnh sửa sai sót của AI (Quality Gate) và đánh giá hiệu quả thực chiến. |
| `reports/swp-evaluation-report.html` | Báo cáo HTML trực quan xuất từ skill chấm điểm SWP (`Scope × Complexity → SWP`) cho toàn bộ 26 Issue. |

## 4. Ánh xạ Spec LAB-4 / PREMISE → Epic trên GitHub Project

| Spec (Mã / File LAB-4 hoặc dòng PREMISE) | Epic trên GitHub Project | Ghi chú & Ràng buộc nghiệp vụ đặc thù |
| :--- | :--- | :--- |
| `02-architecture/01, 04`; `ADR-01, 08, 10, 11, 12`; `scr-01..03, 25`; `PREMISE.md §1` | `#1 — EPIC-01: Hạ tầng Cloud, Bảo mật, Phân quyền & Audit` | Chốt hosting Cloud của khách (AWS Tokyo), shim `auth.uid()`, TOTP MFA bắt buộc cho admin/approver và bảo vệ dữ liệu cá nhân theo luật APPI (ADR-12). |
| `01-basic-design`; `03-database/02 (§1)`; `scr-04, 05`; `ADR-03`; `PREMISE.md §3` | `#2 — EPIC-02: Quản lý Người tham gia & Phân nhánh Tư cách` | **Ràng buộc chống loại hồ sơ (RFP 12-06):** Tuyệt đối không gộp `許可` (仲卸 — gỡ tạm dừng có điều kiện) và `承認` (売買参加者 — gỡ tạm dừng qua tái thẩm tra `maker ≠ checker`). |
| `04-detail-design/scr-07..12, 15, 16`; `03-database/02`; `ADR-02, 05`; `PREMISE.md §1` | `#3 — EPIC-03: Tiếp nhận Lot, Giao dịch (相対/せり) & Giao nhận` | Kiểm tra tư cách, hạn mức `参加上限` và tồn kho khả dụng ngay tại thời điểm xác nhận (`BR-PERM-01`); kết quả đấu giá `せり` nhập tay bắt buộc `maker ≠ checker`. |
| `04-detail-design/scr-17`; `03-database/02 (§5)`; `ADR-04, 06, 07`; `PREMISE.md §1, §2` | `#4 — EPIC-04: Khóa sổ ngày (Snapshot), Version Quy tắc & Điều chỉnh sau khóa` | Số liệu ngày đã khóa (`daily_settlements`) bất biến tuyệt đối (`BR-CLOSE-01`); mọi sửa đổi sau khóa phải sinh dòng mới `reverse + correction` có phê duyệt (ADR-07). |
| `02-architecture/02 (luồng g, i)`; `03-database/02 (§2)`; `ADR-04, 07`; `PREMISE.md §1, §2` | `#5 — EPIC-05: Nghiệp vụ Tiền thưởng Thanh toán Đủ hạn (完納奨励金)` | **Ràng buộc đặc thù Product A (Tách riêng):** Xử lý đối chiếu thanh toán đủ hạn (`消込`), bảng tỷ lệ bậc lũy tiến có version (≤ 4 lần sửa/năm), làm tròn sau thuế và sinh bút toán chênh lệch `delta` kỳ sau (`BR-INC-01`). |
| `02-architecture/03`; `06-traceability`; `ADR-09, 13`; `PREMISE.md §2 (ASM-L5-04)` | `#6 — EPIC-06: Hệ thống Thông báo, 12 Báo cáo Chuẩn & Xuất CSV (IF-ACC-01)` | Batch báo cáo ngày hoàn tất trước 11:00 JST (`FIG-LOAD-01`); xuất dữ liệu kế toán `IF-ACC-01` dạng CSV kèm `batch_code` từ snapshot đã khóa. |
| `02-architecture/02 (luồng d2)`; `scr-07, 16`; `ADR-05`; `PREMISE.md §2 (ASM-L5-03)` | `#7 — EPIC-07: Đồng bộ Offline (PWA Outbox) & Chuyển đổi Dữ liệu Lịch sử` | Offline chỉ giới hạn cho tiếp nhận Lot (cấp mã tạm `仮番号`) và ghi nhận Giao nhận; xác nhận giao dịch bắt buộc online để đảm bảo kiểm tra tư cách thời gian thực. |
