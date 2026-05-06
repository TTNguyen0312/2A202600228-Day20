# Workshop 2 - Roadmap Now Next Later

**Product:** AI agent hội thoại tiếng Việt phân khoa cho bệnh nhân ngoại trú  
**Student:** Nguyễn Trọng Tiến

---
## Now / Next / Later

### NOW
**Vấn đề 1:** Bệnh nhân không biết mình cần khám khoa gì, tự phán đoán sai, đến nhầm chuyên khoa, mất thời gian và tăng tải cho tiếp tân.

**Đang code:** Agent hội thoại sàng lọc đa vòng (3–5 vòng) bằng tiếng Việt, gợi ý 1 chuyên khoa phù hợp.



**Vấn đề 2:** AI không phân biệt được ca cấp cứu với triệu chứng thông thường, một ca miss có thể gây hại bệnh nhân và đóng cửa toàn bộ sản phẩm.

**Đang code:** Safety rule layer với keyword matching red flag, chạy trước LLM, hiển thị cảnh báo đỏ + escalate cấp cứu.


---

### NEXT


**Vấn đề 1:** Bệnh nhân biết khoa cần đến nhưng không biết slot nào còn trống, phải gọi điện hoặc đến trực tiếp hỏi, tạo bottleneck tại quầy tiếp nhận.

**Sẽ làm:** Booking confirmation flow hiển thị bác sĩ + slot khả dụng real-time, bệnh nhân xác nhận đặt lịch ngay trong hội thoại.

**Vấn đề 2:** AI đưa ra gợi ý khoa ngay cả khi confidence thấp, bệnh nhân tin theo gợi ý sai, nhân viên không có thông tin để can thiệp kịp thời.

**Sẽ làm:** Escalation flow tự động sau 3 vòng nếu confidence < 70%, chuyển transcript sang nhân viên tiếp nhận.

---

### LATER

**Vấn đề:** Bệnh nhân đã đặt được lịch đúng khoa nhưng không biết đường đi trong bệnh viện, lạc trong khuôn viên, đến trễ, làm trễ cả lịch bác sĩ.

**Có thể làm:** Tích hợp bản đồ indoor dẫn đường đến khoa khám trực tiếp trong hội thoại.

---
