# Workshop 4 - 3 Dependencies + Plan B + Critical Path

**Product:** AI agent hội thoại tiếng Việt phân khoa cho bệnh nhân ngoại trú  
**Student:** Nguyễn Trọng Tiến

---
## External Dependencies & Critical Path (Investor Package)

### 3 External Dependencies có thể giết dự án trong 30 ngày tới

---

#### Dependency 1: OpenAI API tăng Rate Limit hoặc tăng Giá Đột Ngột

**Worst-case scenario:** OpenAI đột ngột giảm rate limit hoặc tăng giá 2 đến 3×, khiến chi phí vận hành vượt ngân sách pilot và agent bị timeout trong giờ cao điểm tại bệnh viện.

**Plan B:** Chuyển sang các phương án thay thế như Claude Haiku (Anthropic) hoặc Gemini (Google),cả hai đều có chất lượng tương đương GPT-4o mini cho tác vụ hội thoại tiếng Việt, hỗ trợ OpenAI-compatible SDK nên chỉ cần đổi `base_url` + `api_key`, không cần viết lại logic.

**Cost của Plan B:**
- Tiền: Claude Haiku 3.5 ~$0.80/$4 per 1M token / Gemini 2.0 Flash ~$0.10/$0.40 per 1M token — cả hai rẻ hơn GPT-4o mini ngay cả trước khi tăng giá
- Thời gian: 1 đến 2 ngày swap + kiểm thử lại safety layer và conversation flow

---

#### Dependency 2: Bộ Y tế / Sở Y tế yêu cầu Quy định phần mềm hỗ trợ chẩn đoán y tế

**Worst-case scenario:** Cơ quan quản lý yêu cầu sản phẩm phải có giấy phép phần mềm y tế trước khi triển khai thực tế tại bệnh viện, làm đứng pilot vô thời hạn.

**Plan B:** Tái định vị sản phẩm từ "AI hỗ trợ chẩn đoán" sang "chatbot điều hướng hành chính không y tế" (chỉ hỏi và routing, không đưa kết luận bệnh lý), kèm cập nhật disclaimer UI + tài liệu nội bộ, không cần giấy phép y tế.

**Cost của Plan B:**
- Tiền: ~0 VND (chỉ thay copy + disclaimer UI)
- Thời gian: 3–4 cập nhật UI + soạn disclaimer pháp lý

---

#### Dependency 3: API Hệ Thống HIS Bệnh Viện Từ Chối Tích Hợp hoặc Không Có API

**Worst-case scenario:** Bệnh viện pilot dùng HIS nội bộ đóng (không có REST API), IT từ chối mở kết nối ngoài vì chính sách bảo mật, khiến tính năng booking slot real-time trong cột NEXT không thể triển khai đúng hạn.

**Plan B:** Bỏ tính năng booking slot, thay bằng cấp số thứ tự khám bệnh, sau khi agent xác định đúng chuyên khoa, hệ thống tự cấp một mã số hàng chờ cho khoa đó (lưu local, không cần HIS), bệnh nhân cầm số đến thẳng quầy đúng khoa, nhân viên tiếp nhận quét mã xác nhận.

**Cost của Plan B:**
- Tiền: ~0 VND (chỉ cần bảng số thứ tự per khoa lưu trong DB nội bộ, không phụ thuộc hệ thống ngoài)
- Thời gian: 3 đến 4 ngày xây module cấp số + hiển thị mã QR + 1 ngày train nhân viên

---

### Critical Path cho cột NOW

**5–7 task chính:**

| Task | Mô tả | Blocks |
|------|-------|--------|
| **T1** Thiết kế conversation flow | Viết prompt template 3–5 vòng hỏi triệu chứng, mapping sang các chuyên khoa theo database bệnh viện | T3 |
| **T2** Xây Safety Rule Layer | Viết keyword list red flag cấp cứu, logic chajy trước LLM | T5 |
| **T3** Tích hợp LLM API + Prompt Engineering | Kết nối OpenAI/Claude API, chạy conversation loop | T4 |
| **T4** Logic phân loại chuyên khoa | Map output LLM → danh sách khoa cụ thể của bệnh viện, xử lý edge case | T5 |
| **T5** Backend API + Deploy staging | Expose endpoint, tích hợp T2 safety layer, deploy lên môi trường test | T6 |
| **T6** User Testing nội bộ (5-10 ca thật) | Test với nhân viên đóng vai bệnh nhân, đo accuracy routing + emergency case | T7 |
| **T7** Pilot live tại bệnh viện | Go-live tại quầy tiếp nhận, monitor realtime, thu kết quả feedback | — |

**Blocking dependencies:**
- T1 → T3 (không có flow thì không thể integrate LLM)
- T3 → T4 (LLM output cần trước khi map chuyên khoa)
- T2 → T5 (safety layer phải sẵn trước khi deploy bất kỳ version nào)
- T4 → T5 (logic phân loại phải xong trước khi build backend)
- T5 → T6 → T7 (tuần tự bắt buộc)

**Critical Path:**

```
T1 → T3 → T4 → T5 → T6 → T7
               ↑
               T2          
```

> T2 chạy song song với T1–T3, merge vào T5. Nếu T2 trễ thì T5 bị block.  
> Tổng thời gian Critical Path ước tính: 2 tháng (T1: 7 ngày, T3: 7 ngày, T4: 3 ngày, T5: 4 ngày, T6: 7 ngày, T7: ongoing).