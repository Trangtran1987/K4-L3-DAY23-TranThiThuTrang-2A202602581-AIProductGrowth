# Worksheet — BookingBot (AI Agent Direct Connect BĐS)

Họ tên: Trần Thị Thu Trang · MSSV: 2A202602581 · Ngày làm: 09/10/2026

> Số liệu tài chính lấy từ `TranThiThuTrang_Day22_model.xlsx` (ghi ô nguồn dạng `Tab!Ô`). Sản phẩm **chưa go-live**: mọi số "hiện tại" là số mô hình (ước tính), chưa phải số đo.

## Trạm 1 — Loại mô hình

**Câu chốt loại:** Chúng tôi là **B2B2C** vì **người trả tiền là chủ đầu tư (CĐT)**, cụ thể là sàn ký gửi/bán lại của CĐT, trả 2.000.000₫/dự án/tháng + 20.000₫/lịch AI tự chốt. **Người dùng sản phẩm lại là khách của CĐT**: người mua và chủ căn chat trực tiếp với BookingBot trên Zalo OA của sàn. Chúng tôi **chạm được end-user**, vì hội thoại, lịch xem và Deal ID nằm trong hệ thống của mình. Vì vậy đây không phải nhà cung cấp B2B thuần. Biến thể: *embedded* trong kênh mang thương hiệu CĐT (gần white-label). Rủi ro của biến thể này là CĐT dễ thay thế chúng tôi.

**Bảng đèn §3.3 (B2B2C)** (ghi đủ mọi đèn trong bảng):

✅ = đo được hôm nay · 🔧 = đo được trong 2 tuần nếu làm thêm (log, event tracking, điều khoản hợp đồng) · ❌ = chưa đo được và chưa biết cách / chưa thể có số

Event tracking dùng lại từ Day 20: `viewing_request_created`, `owner_confirmation_received`, `viewing_hold_created`, `viewing_completed`, `viewing_no_show`.

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| L · Partner activation rate ⭐ | 🔧 | Bảng `Deal` + `Booking` (trạng thái `OWNER_CONFIRMED`) theo `project_id`. Cần thêm cờ `is_internal_test` để loại lịch test của sàn. Có số từ khi pilot go-live (01/11/2026) |
| L · End-user reach trong partner | 🔧 | Phía chủ nhà: % listing `AVAILABLE` có khung giờ trống trong 7 ngày tới, lấy từ bảng `PropertyAvailability`. Phía người mua: cần CĐT chia sẻ tổng số người nhắn Zalo OA của sàn để làm mẫu số |
| L · Time-to-first-end-user | 🔧 | Ngày ký pilot (hợp đồng) → `created_at` của Booking thật đầu tiên. Đo được ngay khi ký pilot |
| O · Volume volatility | 🔧 | Đếm `AIConversation` có ý định đặt lịch theo tuần, theo CĐT. Tracking dựng được trong 2 tuần, nhưng cần ≥ 8 tuần log mới tính được CV, nên có số sớm nhất cuối 12/2026 |
| O · GM sau rev-share | 🔧 | Không có rev-share, vì CĐT trả thẳng. Mô hình: **75,7%** (`2_Pricing!B21`). Số thật cần hoá đơn Gemini + Zalo ZNS/SMS tách theo CĐT |
| O · Chi phí inference ÷ doanh thu — theo từng partner | 🔧 | Mô hình: **24,3%** (`1_Cost_Job!B65` ÷ ARPU `4_Channel_Fit!B5`). Cần gắn `project_id` vào mọi request Gemini và mọi tin ZNS/SMS |
| O · Tập trung volume | ✅ | **100%**: giai đoạn pilot chỉ có 1 CĐT (Vinhomes, theo scope spec). Đèn này đỏ theo cấu trúc, chưa có ý nghĩa cho tới khi có ≥ 3 CĐT |
| O · Chất lượng nhìn từ end-user | 🔧 | Mini-eval 29 tình huống (deadline 15/10/2026) + eval 100 hội thoại thật (30/11/2026). Spec §2.2: double-booking ≤ 1%, availability accuracy ≥ 95%. **Chưa ký SLA** với CĐT |
| G · Doanh thu/partner · partner NRR · GM tổng | ❌ | NRR cần ≥ 1 chu kỳ gia hạn (~12 tháng). Doanh thu/partner có số sau tháng thu phí đầu tiên (dự kiến 01/2027) |

## Trạm 2 — Thẻ đèn

**North Star:** Số lịch xem **AI tự chốt đúng**/tháng (đúng định nghĩa job `1_Cost_Job!B5`; đếm bằng event `owner_confirmation_received` của Day 20 mà không có chuyên viên can thiệp, trừ lịch trùng/sai). Đây cũng là đơn vị tính tiền, nên NSM tăng thì doanh thu tăng theo. Đèn bật trước của B2B2C (partner activation) là đèn số 1 vì nó là điều kiện để NSM khác 0. Hiện tại **0** (chưa go-live; mô hình ước tính 420/dự án/tháng, `1_Cost_Job!B11`). Mục tiêu **≥ 300 lịch/tháng** ở dự án pilot trước 07/01/2027 (KPI tháng 2–3, `5_90Day_Plan!C17`).

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **Partner activation rate** ⭐ | Đếm: dự án của CĐT đã go-live trên Zalo OA mà có **≥ 1 lịch xem với người mua thật được chủ nhà xác nhận** trong 30 ngày từ go-live. **Không** đếm: lịch test nội bộ của sàn/của tôi, lịch do chuyên viên tự chốt, dự án mới ký nhưng chưa gắn nút "Đặt lịch xem" | Số dự án có ≥ 1 lịch thật trong 30 ngày ÷ số dự án đã go-live | Tuần · Thu Trang (query DB) | North Star → doanh thu/CĐT → partner NRR (đèn 6) |
| 2 | L | **End-user reach phía chủ nhà (slot coverage)** | Đếm: listing `AVAILABLE` có **≥ 3 khung giờ xem trống** trong 7 ngày tới. **Không** đếm: listing `PENDING_REVIEW`/`SOLD`, khung giờ đã bị hold, khung giờ chủ nhà khai sau khi người mua đã hỏi | Listing có ≥ 3 khung trống/7 ngày ÷ tổng listing `AVAILABLE` của dự án | Tuần · Thu Trang | Containment (đèn 3), trễ ~1–2 tuần → chi phí AI/doanh thu (đèn 4) |
| 3 | O | **Containment đúng (AI tự chốt đúng)** | Đếm: lịch AI tự chốt, chủ nhà xác nhận, **không** sai/trùng. **Không** đếm: ca chuyển chuyên viên, lịch bị huỷ do lỗi AI, lịch trùng trong 7 ngày. Mẫu số chỉ gồm cuộc chat **có ý định đặt lịch**, không gồm chat chỉ hỏi thông tin. Kèm cờ chất lượng: **lỗi nghiêm trọng** = bịa căn/giá hoặc double-booking | Lịch AI tự chốt đúng ÷ cuộc chat có ý định đặt lịch | Tuần · Thu Trang (log hội thoại → Booking) | Chi phí AI/doanh thu (đèn 4) cùng tháng · GM quý · partner NRR (đèn 6) |
| 4 | O | **Chi phí AI/lịch ÷ giá hiệu dụng/lịch — theo từng CĐT** | Đếm: LLM (Gemini) + tin nhắn OTP/xác nhận/nhắc lịch + infra + retry + QA nội bộ **gắn với CĐT đó** (theo `project_id`). **Không** lấy trung bình toàn hệ thống. **Không** gồm R&D/sales | (Tổng chi phí biến đổi + QA của CĐT trong tháng ÷ số lịch AI tự chốt đúng) ÷ giá hiệu dụng/lịch của CĐT đó (mặc định 24.762₫). Đo được cả khi pilot chưa thu tiền. Khi CĐT đã trả phí, đèn này = chi phí ÷ doanh thu, và GM = 1 − đèn 4 | Tuần (ước từ log) · tháng (chốt theo hoá đơn) · Thu Trang | GM quý · LTV:CAC (Day 22: 4,0) · partner NRR |
| 5 | O | **Volume volatility theo CĐT** | Đếm: số cuộc chat có ý định đặt lịch/tuần của từng CĐT, cửa sổ trượt 8 tuần. **Không** đếm: tin spam/bot, tin nhắn tự động của sàn | Độ lệch chuẩn volume tuần ÷ volume tuần trung bình (CV), 8 tuần gần nhất | Tháng · Thu Trang | Chi phí AI/doanh thu (đèn 4) tháng sau → GM quý |
| 6 | G | **Partner NRR** | Doanh thu tháng thứ 12 của nhóm CĐT đang trả tiền ÷ doanh thu tháng đầu của chính nhóm đó (gồm thêm dự án/upsell, trừ huỷ/giảm). **Không** tính CĐT mới ký trong kỳ | Doanh thu nhóm CĐT cũ cuối kỳ ÷ doanh thu đầu kỳ | Quý · Thu Trang + 1 người ngoài kiểm | — (bảng điểm) |

Đèn chi phí AI là đèn số: **4** (Chi phí AI ÷ doanh thu theo từng CĐT).

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn [BM]/[MH]/[TB] | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | Partner activation rate | ≥ 60% | 30–60% | < 30% | [TB] | Chưa có chuẩn ngành. Dùng mốc khởi đầu của sổ tay §3.3, đo 2 chu kỳ 30 ngày rồi lấy làm baseline (dự kiến có số: chu kỳ 1 ngày 01/12/2026, chu kỳ 2 ngày 31/12/2026). Khi có < 5 dự án thì đọc theo số tuyệt đối (ví dụ 1/1, 0/1) |
| 2 | End-user reach phía chủ nhà | ≥ 60% | 30–60% | < 30% | [TB] | Chưa có chuẩn. Đây là nguyên nhân gốc Day 22 đã chỉ ra ("chủ nhà mở quá ít khung giờ"). Đo 2 chu kỳ 1 tuần đầu pilot làm baseline (có số ngày 15/11/2026), sau đó đặt 🟢 = baseline + 20 điểm % |
| 3 | Containment đúng | ≥ 57% | 43–57% | < 43% **hoặc** ≥ 1 lỗi nghiêm trọng/tuần | [MH] | 57% giữ GM ≥ 70%. 42,6% là breakeven của GM mục tiêu 60% (`2_Pricing!B33`). Xem [MH] 1 |
| 4 | Chi phí AI/lịch ÷ giá hiệu dụng | < 30% (≤ 7.430₫/lịch) | 30–40% | > 40% (> 9.905₫/lịch) | [MH] | > 40% nghĩa là GM < 60%, dưới GM mục tiêu (`2_Pricing!B32`). Mô hình hiện 24,3% (6.025₫/lịch). Xem [MH] 2 |
| 5 | Volume volatility (CV 8 tuần) | < 12% | 12–32% | > 32% | [MH] | Một tuần đột biến +2σ mà không ra thêm lịch: với CV 32% là +64%, đủ kéo GM xuống 60%. Xem [MH] 3 |
| 6 | Partner NRR | ≥ 110% | 100–110% | < 100% | [BM] | Trung vị SaaS NRR **101%** (Benchmarkit 2025, dữ liệu FY2024). Ngày kiểm tra: 27/08/2026 theo sổ tay §8.2 mục 10. ⚠️ Mở link kiểm tra lại trước khi nộp |

### Phụ lục [MH] — phép tính (≥2)

**[MH] 1 — Containment đúng (đèn 3)**

```
Đầu vào (Day 22 model):
  v = chi phí biến đổi / job thử (LLM + infra/tin nhắn + retry) = $0,1472   (2_Pricing!B29)
  q = QA nội bộ / job thử                                          = $0,0150   (2_Pricing!B30)
  e = chi phí 1 ca escalate (biến thể A, CĐT tự chịu)               = $0        (2_Pricing!B31)
  P = giá hiệu dụng / lịch = (2tr + 20k × 420) / 420 = 24.762₫      = $0,9524   (2_Pricing!B19)
Phép tính: R ≥ (v + q + e) / (P × (1 − GM))
  GM 70% → 0,1622 / (0,9524 × 0,30) = 0,1622 / 0,2857 = 56,8%
  GM 60% → 0,1622 / (0,9524 × 0,40) = 0,1622 / 0,3810 = 42,6%   (= 2_Pricing!B33)
  GM 50% → 0,1622 / (0,9524 × 0,50) = 0,1622 / 0,4762 = 34,1%   (mốc "mô hình gãy" của one-pager)
Kết quả → 🟢 ≥ 57% · 🟡 43–57% · 🔴 < 43%
  (đỏ ngay khi có ≥ 1 lỗi nghiêm trọng, vì đây là B2B2C: lỗi của bot = khủng hoảng thương hiệu của CĐT)
```

**[MH] 2 — Chi phí AI/lịch ÷ giá hiệu dụng (đèn 4)**

```
Đầu vào: Cost/Job = $0,2317 ≈ 6.025₫/lịch                       (1_Cost_Job!B66, B69)
         Giá hiệu dụng P = $0,9524 ≈ 24.762₫/lịch                (2_Pricing!B19)
         (tương đương: chi phí $97,33/tháng ÷ doanh thu $400/tháng — 1_Cost_Job!B65, 4_Channel_Fit!B5)
         Tin nhắn OTP/xác nhận/nhắc lịch = 500₫/tin (ước tính trong 1_Cost_Job!B41) — khoản lớn nhất
Phép tính: Hiện tại = 6.025 / 24.762 = 24,3% → GM 75,7%   (khớp 2_Pricing!B21)
  GM = 1 − đèn 4 → GM 70% ⇔ đèn 4 ≤ 30% ⇔ Cost/Job ≤ 0,30 × 24.762 = 7.429₫
                   GM 60% ⇔ đèn 4 ≤ 40% ⇔ Cost/Job ≤ 0,40 × 24.762 = 9.905₫
  Độ nhạy theo giá tin nhắn s (₫): chi phí/tháng ≈ $53,49 + 0,0877 × s
    chi phí ≤ $120 (30%) ⇔ s ≤ ~758₫ ; chi phí ≤ $160 (40%) ⇔ s ≤ ~1.215₫ ; chi phí ≤ $200 (50%) ⇔ s ≤ ~1.670₫ (khớp one-pager)
Kết quả → 🟢 < 30% · 🟡 30–40% · 🔴 > 40%
```

**[MH] 3 — Volume volatility (đèn 5)**

```
Đầu vào: Chi phí gần như tỷ lệ thuận với số cuộc chat thử (600/tháng, 1_Cost_Job!B9) → $97,33/tháng
         Doanh thu chỉ tăng khi có thêm lịch tự chốt; phí nền 2tr cố định
         Kịch bản xấu: CĐT chạy chiến dịch → chat tăng Δ nhưng toàn khách ý định thấp (số lịch không đổi)
Phép tính: GM ≥ 70% ⇔ 97,33 × (1 + Δ) ≤ 120 ⇔ Δ ≤ +23%
           GM ≥ 60% ⇔ 97,33 × (1 + Δ) ≤ 160 ⇔ Δ ≤ +64%
  Lấy đột biến "bình thường xấu" = +2σ ⇒ σ/μ = Δ/2:
           CV ≤ 11,6% → +2σ ≤ 23% (GM vẫn ≥ 70%) ; CV > 32% → +2σ > 64% (GM có thể < 60%)
Kết quả → 🟢 < 12% · 🟡 12–32% · 🔴 > 32%
```

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (cần ≥2).

1. ⏹ **NẾU** partner activation rate < 30% **TRONG** 60 ngày **VÀ** đã có ≥ 3 dự án go-live **THÌ** dừng ký CĐT/dự án mới. Founder dồn toàn bộ thời gian vào kích hoạt dự án đã ký: gắn nút "Đặt lịch xem" lên mọi tin đăng, hướng dẫn chuyên viên sàn chuyển khách vào Zalo OA. **KHÔNG THÌ** không được dùng "số CĐT/dự án đã ký" làm chỉ số tăng trưởng trong bất kỳ báo cáo hay pitch nào.
2. ⏹ **NẾU** containment đúng < 43% **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần ≥ 50 cuộc chat có ý định đặt lịch **THÌ** tắt chế độ tự chốt, chuyển bot về "đề xuất khung giờ, chuyên viên xác nhận" và sửa lỗi theo log trong 1 sprint (2 tuần). Riêng ≥ 1 lỗi nghiêm trọng (bịa căn/giá, double-booking) thì kích hoạt ngay, không chờ cửa sổ. **KHÔNG THÌ** không mở thêm dự án mới và không chạy chiến dịch kéo thêm traffic vào Zalo OA, vì thêm volume ở containment thấp là lỗ thêm và hỏng thương hiệu CĐT.
3. **NẾU** slot coverage phía chủ nhà < 30% **TRONG** 2 tuần liên tiếp ở một dự án **THÌ** cùng trưởng sàn chạy chiến dịch nhắc chủ nhà mở ≥ 3 khung giờ/tuần (1 tin Zalo + 1 cuộc gọi của chuyên viên), đo lại sau 2 tuần. Lý do: vấn đề nằm ở động lực của chủ nhà/sàn, không nằm ở model. **KHÔNG THÌ** không đổi sang LLM đắt hơn hay viết lại prompt để "cứu" containment, vì model không tạo ra khung giờ mà chủ nhà không mở.
4. ⏹ **NẾU** chi phí AI/lịch ÷ giá hiệu dụng của một CĐT > 40% (Cost/Job > 9.905₫) **TRONG** 1 tháng (đủ 1 kỳ hoá đơn) **THÌ** dừng gửi tin SMS/ZNS trả phí vượt mức ở CĐT đó: giảm từ 4 xuống 2 tin/lịch, chuyển nhắc lịch sang tin OA miễn phí trong cửa sổ 48h. Đồng thời đàm phán để phần tin nhắn vượt quota thành pass-through. **KHÔNG THÌ** không tăng phí 20.000₫/lịch đại trà cho mọi CĐT để bù lỗ của một CĐT.
5. **NẾU** volume volatility > 32% **TRÊN** 8 tuần gần nhất **VÀ** có ≥ 1 tuần volume tăng > 64% mà số lịch tự chốt không tăng tương ứng **THÌ** đàm phán lại hợp đồng CĐT đó: phí nền bậc thang theo số cuộc chat (ví dụ vượt 900 chat/tháng thì tính thêm) hoặc pass-through chi phí tin nhắn. **KHÔNG THÌ** không cam kết SLA cứng hay giá cố định trên lưu lượng do chiến dịch của CĐT tạo ra.
