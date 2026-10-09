# Worksheet — RedPilot AI

Họ tên: Trần Thị Hương · MSSV: `[ĐIỀN MSSV]` · Ngày làm: 09/10/2026

## Số liệu đầu vào (từ mô hình tài chính Day 24 & Value Metric / Cost/Job Day 25)

> Số tiền đã nhân cùng một hệ số để ẩn số thật (xem README). Đây là **giả định mô hình**, chưa phải số đo thực tế — sản phẩm chưa có khách trả phí.

| Dữ liệu | Giá trị | Đơn vị · kỳ đo | Nguồn |
|---|---|---|---|
| ARPA (ARPU theo tài khoản) | 15.000.000 | ₫ / khách / tháng (gói Team) | Bảng giá Day 25 |
| ACV | 180.000.000 | ₫ / khách / năm = ARPA × 12 | Suy ra |
| Gross Margin mục tiêu (GM) | 60% | % doanh thu, gồm LLM + sandbox + hạ tầng | Mô hình Day 24 |
| CAC | 72.000.000 | ₫ / khách mới (sales + POC + marketing) | Mô hình Day 24 |
| CAC Payback mục tiêu | ≤ 12 | tháng (ngưỡng SMB, HANDBOOK §8.2 mục 11) | Day 24 + [BM] |
| Runway | 9 | tháng, tính từ 10/2026 | Mô hình Day 24 |
| Value Metric | 1 *validated run* | lượt mô phỏng tấn công hoàn tất trên 1 target trong scope, báo cáo được khách xác nhận; gói Team = 40 run/tháng | Day 25 |
| Doanh thu / Job | 375.000 | ₫ / run = 15.000.000 ÷ 40 | Suy ra |
| Cost/Job | 95.000 | ₫ / run (LLM ≈ 70.000 + sandbox/compute ≈ 25.000) — **ước tính** từ các lượt chạy thử | Day 25 |

**Runway thật sự:** sales cycle B2B ≈ 19 tuần ≈ 4,4 tháng ([BM] ICONIQ GTM 2026) → 9 − 4,4 ≈ **4,6 tháng** để chứng minh mô hình. Đây là lý do cổng ngày 90 phải ra quyết định cứng.

## Trạm 1 — Loại mô hình

**A. Ba câu hỏi**

1. **Ai trả tiền?** Doanh nghiệp (fintech, ví điện tử, TMĐT quy mô SME có API public) — ký hợp đồng thuê bao theo năm.
2. **Ai dùng?** Đội bảo mật / DevSecOps / QA nội bộ của chính doanh nghiệp đó — là người trả tiền, không phải khách hàng của họ.
3. **Có trung gian chạm người dùng cuối không?** Không. Sản phẩm không có giao diện hay dữ liệu nào hướng tới khách hàng cuối của doanh nghiệp.

**B. Câu chốt loại:** Chúng tôi là **B2B** vì doanh nghiệp trả phí thuê bao, đội bảo mật nội bộ của chính doanh nghiệp đó trực tiếp sử dụng, và sản phẩm không chạm tới người dùng cuối của doanh nghiệp.

**C. Bảng đèn §3.2 (B2B)** — ghi đủ mọi đèn trong bảng, thêm Cost/Job vì là đèn chi phí AI:

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Time-to-first-value (TTFV) ⭐ | 🔧 | Chưa có khách. Cần 2 trường trong CRM: `contract_signed_at` và `first_report_ack_at` (thời điểm security lead của khách bấm xác nhận báo cáo đầu tiên trong app) |
| Pipeline coverage | 🔧 | Đang ghi cơ hội trên Google Sheet; cần thêm cột giá trị cơ hội + trạng thái "đủ điều kiện" (đã có budget owner + scope) |
| % deal chết ở security/procurement | 🔧 | Cần trường `lost_reason` bắt buộc khi đóng deal, với giá trị chuẩn: security review / legal-RoE / procurement / giá / khác |
| POC → paid | ❌ | Chưa có POC nào kết thúc. Có số sớm nhất khi 3 pilot đầu kết thúc (dự kiến 12/2026) |
| Sales cycle (tuần) | 🔧 | Cần ngày "cơ hội đủ điều kiện" và ngày ký trong CRM |
| Usage depth trong tài khoản | 🔧 | Bảng `runs` trong backend có `tenant_id`, `status`, `created_at`; cần thêm cột `report_acknowledged` và job tổng hợp theo tuần |
| Chi phí triển khai ÷ ACV | 🔧 | Cần timesheet theo khách (giờ onboarding, viết scope/RoE, tích hợp CI) × đơn giá giờ công |
| Tập trung doanh thu | ❌ | Chưa có doanh thu |
| NRR | ❌ | Cần ≥ 4 quý dữ liệu doanh thu theo khách |
| Gross Margin | ❌ | Chỉ có ước tính Day 24; cần hoá đơn LLM + cloud thật tách theo tenant |
| CAC payback | ❌ | Chưa có khách trả phí; cần chi phí sales/marketing thật theo tháng |
| *(thêm)* AI Cost/Job | 🔧 | Ước tính 95.000 ₫/run từ lượt chạy thử; cần log `tokens_in/out` + phút sandbox theo `run_id` |

## Trạm 2 — Thẻ đèn

**North Star:** **Validated runs / tháng** — số lượt mô phỏng tấn công chạy trên target thật của khách (trong scope RoE đã ký), hoàn tất, và có báo cáo được security lead của khách bấm xác nhận. **Không** đếm: run trên range nội bộ / demo (crAPI), run lỗi giữa chừng, run chưa được khách xác nhận. — hiện tại **0** (chưa có khách) — mục tiêu **80 run/tháng tại ngày 90** (= 2 khách trả phí × 40 run).

*Vì sao:* trùng với Value Metric (khách trả tiền theo run), và một run chỉ được tính khi khách thừa nhận nó có giá trị — không đếm số lần gọi LLM.

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **TTFV** ⭐ | Số ngày từ ngày ký pilot/hợp đồng **và** RoE đến ngày security lead xác nhận báo cáo validated đầu tiên. **Không** tính: "đã cài agent", run trên range demo, báo cáo chỉ có false positive | `first_report_ack_at − contract_signed_at` (ngày), xét từng khách | Mỗi khách, cập nhật hằng tuần · Delivery lead | #4 POC→paid, #5 Usage depth → #7 NRR |
| 2 | L | **p95 AI Cost/Job ÷ Doanh thu/Job** | Chi phí LLM (token) + sandbox/compute của một validated run, lấy **p95** các run trong tuần. **Không** tính run nội bộ/demo, chi phí lương | p95(cost/run) ÷ 375.000 ₫ (%) | Tuần · Engineer phụ trách backend | GM thực tế → #8 CAC payback |
| 3 | L | **% deal chết ở security/procurement** | Deal đóng-thua có `lost_reason` ∈ {security review, legal/RoE, procurement}. **Không** tính deal thua vì giá, vì không có budget | Deal thua ở 3 khâu đó ÷ tổng deal đóng (thắng + thua) trong quý | Tháng · Founder/Sales | #4 POC→paid, Sales cycle |
| 4 | O | **POC → paid** | Pilot 30 ngày đã kết thúc và chuyển thành hợp đồng trả phí ≥ 6 tháng. **Không** tính pilot đang chạy, gia hạn pilot miễn phí | Pilot chuyển paid ÷ pilot đã kết thúc | Tháng (xem theo quý) · Founder | #7 NRR, #8 CAC payback |
| 5 | O | **Usage depth** | Run validated thực dùng so với quota đã mua, theo từng tài khoản trả phí/pilot. **Không** tính run lỗi, run do đội mình chạy hộ | Validated runs trong tháng ÷ quota run tháng (%) | Tuần · Customer success | #7 NRR (gia hạn & mở rộng) |
| 6 | O | **Chi phí triển khai ÷ ACV** | Giờ công onboarding (scope, RoE, tích hợp CI, tinh chỉnh kịch bản) × đơn giá + chi phí tích hợp, trong 60 ngày đầu. **Không** tính chi phí sales trước ký | Chi phí triển khai ÷ 180.000.000 ₫ (%) | Mỗi khách · Delivery lead | #8 CAC payback |
| 7 | G | **NRR** | Doanh thu định kỳ sau 12 tháng của nhóm khách đầu kỳ (gồm mở rộng, trừ giảm gói/churn). **Không** tính khách mới trong kỳ | MRR cuối kỳ của cohort ÷ MRR đầu kỳ của cohort | Quý · Founder | — |
| 8 | G | **CAC payback** | Số tháng lãi gộp để thu hồi CAC + chi phí triển khai của một khách | (CAC + chi phí triển khai) ÷ (ARPA × GM) | Quý · Founder | — |

Tổng 8 đèn: 3 Leading · 3 Operating · 2 Lagging. **Đèn chi phí AI là đèn số: 2.**

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | TTFV | < 30 ngày | 30–60 ngày | > 60 ngày | [TB] | Chưa có baseline: dùng mốc khởi đầu HANDBOOK §3.2 vì pilot dài 30 ngày — quá 60 ngày nghĩa là khách đi hết pilot mà chưa thấy giá trị; đo 2 chu kỳ (3 pilot đầu) rồi đặt lại, dự kiến có số 08/12/2026 |
| 2 | p95 AI Cost/Job ÷ Doanh thu/Job | < 30% (< 112.500 ₫) | 30–40% | > 40% (> 150.000 ₫) | [MH] | Trần 40% suy từ GM 60% (phụ lục [MH] 1); vàng từ 75% trần để còn đệm cho chi phí bị lệch (ICONIQ 2026: 92% công ty AI thấy chi phí AI khó dự đoán) |
| 3 | % deal chết ở security/procurement | < 10% | 10–20% | > 20% | [TB] | Khách mua công cụ tấn công vào chính hệ thống của họ nên review bảo mật/pháp lý là cửa chính; quá 1/5 deal chết ở đây là thiếu Evidence Pack chứ không phải thiếu nhu cầu; baseline sau quý đầu (31/12/2026) |
| 4 | POC → paid | ≥ 50% | 35–50% | < 35% | [BM] | ICONIQ *State of GTM 2026*: POC → paid ≈ 50% (2026), ≈ 36% (2025) — dưới mức 2025 là kém hơn mặt bằng ngành; ngày kiểm tra 27/08/2026 (theo HANDBOOK §8.2, mục 8) |
| 5 | Usage depth | ≥ 60% | 30–60% | < 30% | [TB] | Khách dùng < 30% quota sau 60 ngày sẽ không gia hạn với giá này; mốc khởi đầu HANDBOOK §3.2, đo 2 tháng đầu của mỗi pilot rồi đặt lại (dự kiến 31/12/2026) |
| 6 | Chi phí triển khai ÷ ACV | < 15% | 15–20% | > 20% | [MH] | Trần 20% là mức để CAC + triển khai vẫn hoàn vốn trong 12 tháng (phụ lục [MH] 3); chặt hơn mốc 30% chung của HANDBOOK vì CAC của mình đã ăn 8 tháng lãi gộp |
| 7 | NRR | ≥ 110% | 100–110% | < 100% | [BM] | Benchmarkit 2025 (FY2024): trung vị SaaS NRR 101% — dưới 100% là đang co lại trong tài khoản cũ; ngày kiểm tra 27/08/2026 (theo HANDBOOK §8.2, mục 10) |
| 8 | CAC payback | ≤ 11 tháng | 11–12 tháng | > 12 tháng | [MH] + [BM] | Mục tiêu ≤ 12 tháng là ngưỡng SMB (Bessemer/Skok, ngày kiểm tra 27/08/2026 theo HANDBOOK §8.2 mục 11); quy ra CAC tối đa ở phụ lục [MH] 2 |

### Phụ lục [MH] — phép tính

**[MH] 1 — p95 AI Cost/Job ÷ Doanh thu/Job (đèn 2)**

```
Đầu vào: Doanh thu/Job = 15.000.000 ₫ ÷ 40 run = 375.000 ₫/run
         GM mục tiêu = 60% (GM tính trên COGS = LLM + sandbox/compute, cùng phạm vi với Cost/Job)
         Cost/Job ước tính hiện tại = 95.000 ₫/run
Phép tính: Cost/Job tối đa = 375.000 × (1 − 0,60) = 150.000 ₫/run → 150.000 ÷ 375.000 = 40%
           Ngưỡng vàng = 75% trần = 0,75 × 150.000 = 112.500 ₫ → 30%
           Hiện tại: 95.000 ÷ 375.000 = 25,3% (ước tính, chưa đo p95)
Kết quả → 🟢 < 30% (< 112.500 ₫) · 🟡 30–40% · 🔴 > 40% (> 150.000 ₫)
```

**[MH] 2 — CAC tối đa / CAC payback (đèn 8)**

```
Đầu vào: ARPA = 15.000.000 ₫/tháng · GM = 60% · CAC payback tối đa = 12 tháng
Phép tính: Lãi gộp/khách/tháng = 15.000.000 × 60% = 9.000.000 ₫
           CAC + triển khai tối đa = 9.000.000 × 12 = 108.000.000 ₫
           CAC mô hình = 72.000.000 ₫ → payback chỉ riêng CAC = 72 ÷ 9 = 8 tháng
           Vàng từ 11 tháng = 99.000.000 ₫ (còn đệm 1 tháng trước trần 12)
Kết quả → 🟢 ≤ 11 tháng (≤ 99 triệu) · 🟡 11–12 tháng · 🔴 > 12 tháng (> 108 triệu ₫ CAC + triển khai)
```

**[MH] 3 — Chi phí triển khai ÷ ACV (đèn 6)**

```
Đầu vào: Ngân sách thu hồi trong 12 tháng = 108.000.000 ₫ (từ [MH] 2) · CAC = 72.000.000 ₫ · ACV = 180.000.000 ₫
Phép tính: Chi phí triển khai tối đa = 108.000.000 − 72.000.000 = 36.000.000 ₫ → 36 ÷ 180 = 20% ACV
           Xanh: (72 + 27) ÷ 9 = 11 tháng ↔ triển khai 27.000.000 ₫ = 15% ACV
Kết quả → 🟢 < 15% (< 27 triệu ₫) · 🟡 15–20% · 🔴 > 20% (> 36 triệu ₫)
```

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (có 3).

1. **⏹ Luật 1 (TTFV)** — **NẾU** TTFV > 60 ngày **TRÊN** 3 khách/pilot gần nhất **THÌ** dừng nhận pilot mới có phạm vi > 1 ứng dụng / 1 API gateway, cắt mọi pilot đang chạy về đúng 1 target và 1 bộ kịch bản (OWASP API Top 10) trong 2 tuần — Delivery lead làm. **KHÔNG THÌ** không tuyển thêm sales và không ký thêm pilot để bù — thêm khách vào phễu triển khai đang tắc chỉ làm tắc nặng hơn.
2. **⏹ Luật 2 (security/procurement)** — **NẾU** > 20% deal chết ở khâu security review / legal-RoE / procurement **TRONG** 1 quý **VÀ** quý đó đã đóng ≥ 5 deal **THÌ** dừng outbound 2 tuần để hoàn thiện Evidence Pack: mẫu RoE, chính sách xử lý dữ liệu khách, bảng mapping ISO 27001/PCI DSS, kết quả eval false-positive trên range crAPI — Founder làm. **KHÔNG THÌ** không giảm giá để cứu deal — deal chết ở review bảo mật không chết vì giá.
3. **⏹ Luật 3 (chi phí AI)** — **NẾU** p95 AI Cost/Job > 40% doanh thu/job (> 150.000 ₫/run) **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần ≥ 100 validated run **THÌ** dừng bán gói/quota run không giới hạn, đặt trần token mỗi run và chuyển bước recon/enumeration sang model nhỏ ngay sprint tới — Engineer backend làm. **KHÔNG THÌ** không tăng quota run cho khách và không tăng giá đại trà — sẽ mất khách nhẹ để bù chi phí do vài khách nặng gây ra.
4. **Luật 4 (triển khai)** — **NẾU** chi phí triển khai > 20% ACV (> 36 triệu ₫) **TRÊN** 3 khách liên tiếp **THÌ** đóng gói onboarding thành sản phẩm trong 3 tuần (template scope + RoE, connector CI/CD sẵn, wizard nhập OpenAPI spec) và báo giá phí setup riêng cho khách mới. **KHÔNG THÌ** không nhận thêm khách mới với cùng mức giá trọn gói — mỗi khách mới làm biên xấu thêm.
5. **Luật 5 (usage depth)** — **NẾU** usage depth < 30% quota **SAU** 60 ngày kể từ go-live của một tài khoản **THÌ** Customer success đặt lịch chạy 1 run/tuần cùng đội bảo mật của khách trong 4 tuần và gửi báo cáo tổng hợp finding sau mỗi run. **KHÔNG THÌ** không bán thêm module/gói lớn hơn và không giảm giá gia hạn cho tài khoản đó — bán thêm vào tài khoản chưa dùng hết là cách nhanh nhất mất cả hợp đồng.

## Trạm 5 — Cổng gác & chưa đo được

Ngày bắt đầu: 09/10/2026 → Ngày 30 = 08/11/2026 · Ngày 60 = 08/12/2026 · Ngày 90 = 07/01/2027.

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng vật lý | Nếu trượt |
|---|---|---|---|---|
| 30 (08/11/2026) | Số pilot đã khởi động **có đo** (RoE ký + log `contract_signed_at` và token/run bật) | ≥ 3 pilot | 3 RoE đã ký, ảnh chụp bảng CRM, file export log `runs` | **FIX**: đổi ICP sang fintech có API public, 30 ngày |
| 60 (08/12/2026) | TTFV của từng pilot | < 30 ngày trên ≥ 2/3 pilot | Bảng TTFV theo khách (2 timestamp từ app) | **FIX** (1 lần): cắt pilot về 1 target; nếu đã FIX TTFV → **PIVOT** sang bán dịch vụ pentest có AI hỗ trợ |
| 90 (07/01/2027) | POC → paid | ≥ 50% (≥ 2/3 pilot ký trả phí) | Hợp đồng đã ký + Pilot Report có số finding validated | Đã FIX trước đó → **KILL** theo Kill Criteria; chưa FIX → **PIVOT** phân khúc |

**KILL CRITERIA:** Nếu đến ngày 07/01/2027 có dưới 2 trong ≥ 3 pilot chuyển sang hợp đồng trả phí (POC → paid < 50%) **và** TTFV trung vị vẫn > 60 ngày, sau một lần FIX 30 ngày cắt pilot về 1 target, thì dừng bán RedPilot AI dạng SaaS thuê bao cho SME và đưa toàn bộ runway còn lại (~1,6 tháng hiệu dụng) sang một hướng khác.

| Chỉ số | Trạng thái | Cần bổ sung | Người phụ trách | Hạn có số |
|---|---|---|---|---|
| TTFV | 🔧 | 2 timestamp trong CRM/app (`contract_signed_at`, `first_report_ack_at`) | Delivery lead | 08/12/2026 |
| p95 AI Cost/Job | 🔧 | Log `tokens_in/out` + phút sandbox theo `run_id`, job tính p95 tuần | Engineer backend | 23/10/2026 |
| % deal chết security/procurement | 🔧 | Trường `lost_reason` bắt buộc trong CRM | Founder | 31/12/2026 |
| Usage depth | 🔧 | Cột `report_acknowledged` + job tổng hợp tuần theo tenant | Engineer backend | 08/12/2026 |
| Chi phí triển khai ÷ ACV | 🔧 | Timesheet theo khách | Delivery lead | 08/12/2026 |
| POC → paid | ❌ | Chưa có pilot kết thúc | Founder | 07/01/2027 |
| NRR · GM thực · CAC payback | ❌ | Cần ≥ 1–4 quý doanh thu thật và hoá đơn LLM/cloud theo tenant | Founder | 30/09/2027 (NRR), 31/03/2027 (GM, payback) |
