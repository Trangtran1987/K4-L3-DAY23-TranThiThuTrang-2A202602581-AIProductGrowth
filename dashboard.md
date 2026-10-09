# OPERATING DASHBOARD — BookingBot (AI Agent Direct Connect BĐS)

**Loại mô hình:** B2B2C — CĐT trả tiền; người mua & chủ căn (khách của CĐT) dùng bot trên Zalo OA của sàn · **Cập nhật:** 09/10/2026 · Trần Thị Thu Trang – 2A202602581<br>
**NORTH STAR:** Số lịch xem AI tự chốt đúng/tháng — hiện tại **0** (chưa go-live; mô hình 420/dự án) — mục tiêu **≥ 300/tháng** ở dự án pilot trước 07/01/2027

> ⚪ = chưa đo (chưa go-live). Kèm số mô hình Day 22 và màu dự kiến nếu mô hình đúng.

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| Partner activation ⭐ — dự án go-live có ≥ 1 lịch người mua thật trong 30 ngày | ⚪ 0/0 | ≥ 60% / 30–60% / < 30% | [TB] | North Star → doanh thu/CĐT → NRR |
| Slot coverage — % listing có ≥ 3 khung trống/7 ngày | ⚪ | ≥ 60% / 30–60% / < 30% | [TB] | Containment (trễ 1–2 tuần) |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| Containment đúng — lịch AI tự chốt đúng ÷ chat có ý định đặt lịch | ⚪ · mô hình 70% (🟢) | ≥ 57% / 43–57% / < 43% hoặc ≥ 1 lỗi nghiêm trọng | [MH] 1 | Đèn chi phí AI · GM quý |
| **Chi phí AI/lịch ÷ giá hiệu dụng** (đèn AI) | ⚪ · mô hình 24,3% = 6.025₫ (🟢) | < 30% / 30–40% / > 40% (> 9.905₫) | [MH] 2 | GM quý · LTV:CAC |
| Volume volatility — CV volume chat/tuần, 8 tuần | ⚪ cần 8 tuần | < 12% / 12–32% / > 32% | [MH] 3 | Đèn chi phí AI tháng sau |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| Partner NRR | ⚪ chưa có kỳ gia hạn | ≥ 110% / 100–110% / < 100% | [BM] Benchmarkit 2025, trung vị 101% · kiểm tra 27/08/2026 |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ NẾU partner activation < 30% TRONG 60 ngày VÀ ≥ 3 dự án go-live THÌ dừng ký CĐT/dự án mới, dồn sức kích hoạt dự án đã ký KHÔNG THÌ không dùng "số CĐT đã ký" làm chỉ số tăng trưởng.
2. ⏹ NẾU containment đúng < 43% TRONG 2 tuần liên tiếp VÀ ≥ 50 chat đặt lịch/tuần (≥ 1 lỗi nghiêm trọng thì kích hoạt ngay) THÌ tắt tự chốt, chuyển về "bot đề xuất, chuyên viên xác nhận", sửa trong 1 sprint KHÔNG THÌ không mở dự án mới, không kéo thêm traffic vào Zalo OA.
3. NẾU slot coverage < 30% TRONG 2 tuần liên tiếp ở một dự án THÌ cùng trưởng sàn nhắc chủ nhà mở ≥ 3 khung/tuần, đo lại sau 2 tuần KHÔNG THÌ không đổi sang LLM đắt hơn hay viết lại prompt để cứu containment.
4. ⏹ NẾU chi phí AI/lịch > 40% giá hiệu dụng (> 9.905₫) TRONG 1 tháng THÌ dừng tin SMS/ZNS trả phí vượt mức (4 → 2 tin/lịch), đàm phán pass-through phí tin nhắn KHÔNG THÌ không tăng phí 20.000₫/lịch đại trà cho mọi CĐT.
5. NẾU volume volatility > 32% TRÊN 8 tuần VÀ có tuần volume +64% mà lịch không tăng THÌ đàm phán phí nền bậc thang theo số chat KHÔNG THÌ không cam kết SLA cứng/giá cố định trên lưu lượng CĐT điều khiển.

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 (08/11/2026) | Số CĐT ký pilot **có đường đi tới end-user đã vẽ rõ** (tin đăng → nút "Đặt lịch xem" → Zalo OA → bot → chủ nhà xác nhận) | ≥ 1 ("sẽ tích hợp" không tính) | Biên bản pilot ký · sơ đồ luồng + ảnh màn hình · báo cáo mini-eval 29 tình huống | **FIX** 1 lần: đổi danh sách CĐT, xin giới thiệu ấm. Trượt lần 2 → **PIVOT** sang bán cho sàn môi giới (bảng B2B) |
| 60 (08/12/2026) | Containment đúng trên hội thoại thật | ≥ 60% trên ≥ 200 chat có ý định đặt lịch | Export log hội thoại → Booking · eval 100 hội thoại thật (30/11/2026) | 43–60% → **FIX** slot coverage (luật 3). < 43% → **PIVOT**: bỏ phí/lịch, về phí nền + chuyên viên xác nhận |
| 90 (07/01/2027) | Cost/Job thật của dự án pilot (chi phí AI/lịch) | ≤ 9.905₫/lịch (= GM ≥ 60% ở giá 24.762₫) | Hoá đơn Gemini + Zalo ZNS/SMS tách theo `project_id` · Pilot Report 15/12/2026 | Đã FIX mà vẫn vượt → **KILL** |

**KILL CRITERIA:** Dừng BookingBot dạng "CĐT trả theo lịch AI tự chốt" nếu **đến 07/01/2027** xảy ra một trong hai: containment đúng < **34%** (GM < 50%) trên ≥ 300 chat thật sau khi đã FIX slot coverage một lần, **hoặc** 0 CĐT đồng ý trả phí sau pilot.

**CHƯA ĐO ĐƯỢC:** Cả 6 đèn và North Star (chưa go-live). Cần log hội thoại → lịch, cờ `is_internal_test`, gắn `project_id` vào request Gemini và tin ZNS; số thật đầu tiên ~15/11/2026. Volatility cần 8 tuần log (~cuối 12/2026). NRR cần 1 kỳ gia hạn (sớm nhất Q4/2027). Tập trung volume = 100% theo cấu trúc (1 CĐT pilot), chưa đưa vào luật tới khi có ≥ 3 CĐT. SLA chất lượng với CĐT chưa ký, chốt cùng hợp đồng pilot.
