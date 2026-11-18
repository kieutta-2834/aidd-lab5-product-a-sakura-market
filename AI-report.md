# AI REPORT — Báo Cáo Sử Dụng AI & Kiểm Soát Chất Lượng (LAB-5)

Tài liệu này ghi nhận trung thực quy trình phối hợp giữa Project Leader (PL) và AI Agent trong toàn bộ quá trình phân rã backlog, ước lượng SWP/giờ, vận hành GitHub Project và lập báo cáo tiến độ cho dự án **Product A — 桜水産市場**.

## 1. Các Skill, Công cụ & Cấu trúc Prompt đã sử dụng

1. **Chặng 1 — Chốt tiền đề (`PREMISE.md`) & Rà soát lỗ hổng LAB-4:**
   - *Công cụ/Skill:* LLM Context Reasoning đọc trực tiếp `05-adr/00-tong-hop-de-xuat.md` và `REVIEW-REPORT.md` của LAB-4.
   - *Cấu trúc Prompt:* Yêu cầu AI liệt kê toàn bộ 13 ADR đang ở trạng thái `Proposed`, đối chiếu với 9 điểm review (`RV-01..09`), đề xuất phương án chốt `Accepted` cho từng ADR và lập bảng giả định bổ sung (`ASM-L5-01..05`) lấp các câu hỏi Blocking (`Q-07`, `Q-09`, `Q-12`, `Q-23`) mà không ghi đè quy định cứng của RFP.
2. **Chặng 2 — Phân rã Backlog (`task-breakdown.md`) & Tách ràng buộc đặc thù `完納奨励金`:**
   - *Công cụ/Skill:* Task Decomposition Prompt kết hợp ràng buộc độ hạt từ `validate-estimate.py` (LAB-1).
   - *Cấu trúc Prompt:* Yêu cầu phân rã 7 Epic → Issue → Sub-issue sao cho mọi task thực thi nằm trong ngưỡng **1–8 SWP**, đồng thời tách riêng toàn bộ luồng nghiệp vụ `完納奨励金` (`F-18`) thành `EPIC-05` độc lập với 5 Issue chuyên biệt (bảng bậc lũy tiến, đối chiếu `消込`, batch J-05, chênh lệch `delta` kỳ sau, test vector `C.3`).
3. **Chặng 3 — Chấm điểm SWP bằng Skill, Quy đổi giờ & Đưa lên GitHub Project:**
   - *Công cụ/Skill:* Skill chấm điểm SWP (`Scope S1–S4 × Complexity C1–C4 → SWP`) xuất báo cáo HTML (`reports/swp-evaluation-report.html`) + GitHub CLI / GraphQL API để tạo Issue, Sub-issue và gán custom fields (`SWP`, `Est. Hours`, `Iteration`).
   - *Cấu trúc Prompt:* Chấm điểm từng Issue dựa trên spec màn hình/DB tương ứng, xuất báo cáo HTML trực quan, sau đó quy đổi theo tỷ lệ chuẩn `1 SWP = 4 giờ` (`0,5 MD`) và đối chiếu chênh lệch từng Epic với baseline `1.206,5 MD` của LAB-1.
4. **Chặng 4 — Giả lập Sprint 1 & Tổng hợp Báo cáo Tiến độ (`progress-report.md`):**
   - *Công cụ/Skill:* Sprint Simulation & Progress Reporting Prompt đọc dữ liệu trạng thái thực tế từ GitHub Project #2.
   - *Cấu trúc Prompt:* Tổng hợp bảng trạng thái `Done / In Review / In Progress` của 8 Issue (76 SWP) trong Sprint 1 (19/11 – 02/12/2026), tính toán bảng Burndown từng ngày và phân tích 4 rủi ro trễ tiến độ kèm hành động khắc phục cụ thể.

## 2. Quy trình Con người Review Output của AI & Xử lý Sai sót (Quality Gate)

1. **Chốt chặn 1 — Rà soát nghiệp vụ đặc thù `完納奨励金`:**
   - *Vấn đề phát hiện:* Ở vòng chấm SWP tự động đầu tiên, AI chỉ chấm `ISS-05-02` ở mức **8 SWP** vì chỉ nhìn công thức nhân tỷ lệ mà bỏ sót logic đối chiếu công nợ thanh toán đủ hạn (`消込`), kiểm tra nợ quá hạn liên kỳ và làm tròn xuống sau thuế `floor(× tỷ lệ × 1.10)`.
   - *Cách xử lý:* PL nâng điểm `ISS-05-02` lên **13 SWP** (tách 3 sub-issue) và nâng `ISS-05-04` (xử lý `delta` kỳ sau) từ **5 SWP lên 8 SWP**.
2. **Chốt chặn 2 — Bắt lỗi AI gộp task lớn vượt ngưỡng độ hạt:**
   - *Vấn đề phát hiện:* AI gom toàn bộ Bộ 12 báo cáo chuẩn `RPT-01..12` & Batch J-04 (`ISS-06-02`) thành 1 Issue nặng **34 SWP** (136 giờ), vi phạm quy tắc chia nhỏ task ≤ 8 SWP.
   - *Cách xử lý:* PL yêu cầu giữ nguyên tổng điểm 34 SWP nhưng tách thành **6 Sub-issue nhỏ** (mỗi sub-issue 5–6 SWP, tương đương 2 báo cáo + batch) để hoàn thành gọn trong Sprint 2 tuần.
3. **Chốt chặn 3 — Kiểm chứng tính trung thực của Báo cáo Tiến độ:**
   - *Vấn đề phát hiện:* Ở bản nháp đầu tiên của `progress-report.md`, AI có xu hướng "tô hồng" tiến độ (cho rằng Sprint 1 hoàn thành tới 95% kế hoạch, bỏ qua các nút thắt kỹ thuật).
   - *Cách xử lý:* PL yêu cầu điều chỉnh kịch bản Burndown để phản ánh đúng thực tế dự án: độ trễ 1,5 ngày do lỗi connection pool của shim `auth.uid()` (`ISS-01-01`), tồn đọng 18 SWP ở `In Review` do phải kiểm tra kỹ luật chống loại hồ sơ RFP `12-06` (`ISS-02-02`), và việc dời 10 SWP của `ISS-05-01` sang đầu Sprint 2 do khách chưa giao file tỷ lệ `C.1` (`Q-07`).
4. **Chốt chặn 4 — Đối chiếu chéo số liệu (Cross-Artifact Consistency):**
   - *Cách xử lý:* PL tự tay kiểm tra khớp số liệu giữa GitHub Project #2, `estimate.md`, `reports/swp-evaluation-report.html` và `progress-report.md` (đảm bảo tổng 26 Issue = 290 SWP = 1.160 giờ; Sprint 1 = 8 Issue = 76 SWP).

## 3. Đánh giá Hiệu quả & Điểm Cần Cải Thiện Khi Áp Dụng Thực Tế

- **Điểm hiệu quả:** Việc nạp context theo đúng thứ bậc thẩm quyền (`RFP → LAB-1 Estimate → LAB-4 ADR/Detail Design`) giúp AI duy trì chính xác toàn bộ hệ thống mã định danh (`F-xx`, `SCR-xx`, `ADR-xx`, `Gxx`), tiết kiệm 60–70% thời gian lập WBS và phát hiện sớm mâu thuẫn giữa DB-02 (`rule_parameters` key/value) với `ADR-04` (bảng có kiểu cho `完納奨励金`).
- **Điểm cần cải thiện khi làm dự án thật:**
  - Với bộ tài liệu trên 50 tệp, không thể nạp toàn bộ vào một phiên làm việc do giới hạn context window; cần tổ chức kho tri thức dạng index/tóm tắt gọn nhẹ trước khi gọi AI phân rã task.
  - Trong dự án thật nên tích hợp GitHub Actions / GraphQL script để tự động đồng bộ các trường custom (`SWP`, `Est. Hours`, `Iteration`) lên GitHub Project và xuất biểu đồ Burndown tự động thay vì cập nhật thủ công từng thẻ.
