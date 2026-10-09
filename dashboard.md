# OPERATING DASHBOARD — RedPilot AI

**Loại mô hình:** B2B — doanh nghiệp trả phí thuê bao, đội bảo mật nội bộ của chính họ dùng, không chạm người dùng cuối · **Cập nhật:** 09/10/2026 · Trần Thị Hương – `[MSSV]`
**NORTH STAR:** Validated runs/tháng (run trên target thật của khách, báo cáo được khách xác nhận) — hiện tại **0** (chưa có khách) — mục tiêu **80** tại ngày 90

### Đèn báo sớm (Leading — nhìn hằng tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| TTFV ⭐ (ký + RoE → khách xác nhận báo cáo đầu tiên) | chưa đo | <30 · 30–60 · >60 ngày | [TB] | POC→paid, Usage depth → NRR |
| p95 AI Cost/Job ÷ Doanh thu/Job (375k ₫) | ~25% (ước tính) | <30% · 30–40% · >40% | [MH] 1 | GM → CAC payback |
| % deal chết ở security/procurement | chưa đo | <10% · 10–20% · >20% | [TB] | POC→paid, Sales cycle |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| POC → paid | chưa có | ≥50% · 35–50% · <35% | [BM] ICONIQ GTM 2026, KT 27/08/2026 | NRR, CAC payback |
| Usage depth (run dùng ÷ quota) | chưa đo | ≥60% · 30–60% · <30% | [TB] | NRR |
| Chi phí triển khai ÷ ACV | chưa đo | <15% · 15–20% · >20% | [MH] 3 | CAC payback |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| NRR | chưa có | ≥110% · 100–110% · <100% | [BM] Benchmarkit 2025 (trung vị 101%), KT 27/08/2026 |
| CAC payback ((CAC + triển khai) ÷ lãi gộp/tháng) | 8 th (mô hình) | ≤11 · 11–12 · >12 tháng | [MH] 2 + [BM] SMB <12, KT 27/08/2026 |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ NẾU TTFV >60 ngày TRÊN 3 khách gần nhất THÌ dừng nhận pilot >1 target, cắt pilot đang chạy về 1 target + 1 bộ kịch bản trong 2 tuần KHÔNG THÌ không tuyển thêm sales, không ký thêm pilot.
2. ⏹ NẾU >20% deal chết ở security/procurement TRONG 1 quý VÀ ≥5 deal đã đóng THÌ dừng outbound 2 tuần, hoàn thiện Evidence Pack (RoE mẫu, xử lý dữ liệu, mapping ISO/PCI, eval) KHÔNG THÌ không giảm giá để cứu deal.
3. ⏹ NẾU p95 AI Cost/Job >40% (>150k ₫/run) TRONG 2 tuần liên tiếp VÀ ≥100 run/tuần THÌ dừng bán quota không giới hạn, đặt trần token/run, chuyển recon sang model nhỏ trong sprint tới KHÔNG THÌ không tăng quota, không tăng giá đại trà.
4. NẾU chi phí triển khai >20% ACV TRÊN 3 khách liên tiếp THÌ đóng gói onboarding (template RoE, connector CI, wizard OpenAPI) trong 3 tuần và tính phí setup riêng KHÔNG THÌ không nhận khách mới với cùng giá trọn gói.
5. NẾU usage depth <30% SAU 60 ngày go-live THÌ CS chạy 1 run/tuần cùng đội khách trong 4 tuần, gửi báo cáo sau mỗi run KHÔNG THÌ không upsell, không giảm giá gia hạn tài khoản đó.

### Cổng gác 90 ngày (bắt đầu 09/10/2026)

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 · 08/11/2026 | Pilot đã khởi động có đo (RoE ký + log run/token bật) | ≥3 pilot | 3 RoE ký, export CRM + log `runs` | FIX: đổi ICP sang fintech có API public |
| 60 · 08/12/2026 | TTFV từng pilot | <30 ngày trên ≥2/3 pilot | Bảng TTFV từ 2 timestamp trong app | FIX 1 lần (cắt về 1 target); đã FIX → PIVOT |
| 90 · 07/01/2027 | POC → paid | ≥50% (≥2/3 pilot ký trả phí) | Hợp đồng ký + Pilot Report có số | Đã FIX → KILL; chưa FIX → PIVOT phân khúc |

**KILL CRITERIA:** Nếu đến 07/01/2027 có dưới 2 trong ≥3 pilot ký trả phí (POC→paid <50%) và TTFV trung vị vẫn >60 ngày sau một lần FIX 30 ngày cắt pilot về 1 target, dừng bán RedPilot AI dạng SaaS cho SME và chuyển runway còn lại (~1,6 tháng hiệu dụng) sang hướng khác.

**CHƯA ĐO ĐƯỢC:** 🔧 TTFV, usage depth (cần 2 timestamp + cột `report_acknowledged` — có số 08/12/2026) · 🔧 p95 Cost/Job (log token + sandbox theo `run_id` — 23/10/2026) · 🔧 deal chết ở security (`lost_reason` trong CRM — 31/12/2026) · 🔧 chi phí triển khai (timesheet theo khách — 08/12/2026) · ❌ POC→paid (chưa pilot nào kết thúc — 07/01/2027) · ❌ NRR, GM thực, CAC payback thực (cần doanh thu + hoá đơn LLM/cloud theo tenant — 03/2027, NRR 09/2027). Mọi số tài chính hiện là giả định mô hình Day 24/25.
