# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** Nguyễn Bảo Sơn
> **Mã Sinh Viên / Mã Học viên:** 2A202602402
> **Chủ đề Lựa chọn:** *Trợ lý Quản lý Thư viện & Tài liệu:* Tra cứu vị trí sách, tình trạng mượn/trả và gia hạn tài liệu.

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá | Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm |
| :--- | :---: | :--- |
| **1. Multi-step Reasoning** | **4/5** | Một yêu cầu có thể cần nhiều bước liên tiếp: xác định đúng người dùng và tài liệu, tra cứu danh mục, kiểm tra vị trí sách, xem tình trạng mượn, đối chiếu hạn trả, kiểm tra điều kiện gia hạn rồi mới thực hiện hoặc đề xuất phương án khác. Tuy nhiên, phần lớn quy trình vẫn tuân theo các quy tắc nghiệp vụ tương đối rõ ràng. |
| **2. Tool Interaction** | **5/5** | Trợ lý phải kết nối với hệ thống quản lý thư viện, cơ sở dữ liệu tài liệu, tài khoản độc giả và dịch vụ xác thực. Các chức năng như tra cứu theo thời gian thực, kiểm tra bản sách còn trống và gia hạn đều cần gọi công cụ hoặc API/MCP Server bên ngoài. |
| **3. Dynamic Decision** | **5/5** | Hành động tiếp theo phụ thuộc trực tiếp vào kết quả vừa tra cứu. Ví dụ: nếu sách đang có sẵn thì cung cấp vị trí; nếu đã được mượn thì thông báo ngày dự kiến trả hoặc đề xuất đặt chỗ; nếu tài liệu đủ điều kiện thì tiến hành gia hạn, còn nếu có người đặt trước hoặc đã vượt số lần gia hạn thì phải từ chối và hướng dẫn trả sách. |
| **4. Long Horizon Goal** | **4/5** | Hệ thống cần duy trì mục tiêu và ngữ cảnh qua nhiều lượt hội thoại, chẳng hạn quản lý nhiều tài liệu cùng lúc, ghi nhớ cuốn sách người dùng đang hỏi, xử lý xác nhận gia hạn và theo dõi kết quả. Tuy vậy, đa số tác vụ có thể hoàn thành trong một phiên giao tiếp ngắn và chưa đòi hỏi tự chủ kéo dài nhiều ngày. |
| **TỔNG ĐIỂM AGENTIC FIT** | **18/20** | **Bài toán rất phù hợp để triển khai dưới dạng Agentic System.** |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (SAU KHI CHẠY TEST SUITE TRÊN API THẬT)

> ⚠️ **YÊU CẦU NGHIỆM THU:** Mở tệp `.env` điền `GEMINI_API_KEY` (hoặc `OPENAI_API_KEY`) để kết nối LLM thật trước khi thực thi `python src/app.py --all`. Bài nộp chỉ dùng Mock Offline Provider sẽ không đạt điểm nghiệm thực tế.

Dán 1 đoạn trích xuất log tiêu biểu từ file `docs/trace_waterfall.json` sinh ra từ phản hồi LLM API thật:

```json
[
  {
    "step": 1,
    "query": "Chào bạn, bạn có thể giới thiệu quy chế học vụ cơ bản của Đại học VinUni không?",
    "action_type": "FINAL_ANSWER",
    "thought": "Câu hỏi chung về quy chế học vụ, trả lời trực tiếp không cần gọi Tool.",
    "output": "[Mock Agent Response]: Xin chào! Quy chế học vụ VinUni yêu cầu sinh viên tích lũy tối thiểu 120 tín chỉ và duy trì GPA trên 2.0 để tốt nghiệp.",
    "latency_ms": 3961.4
  },
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026001"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026001",
      "data": {
        "full_name": "Nguyễn Văn An",
        "class": "AI-K4",
        "gpa": 3.85,
        "email": "an.nv@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "PGS.TS Nguyễn Văn A"
      }
    },
    "latency_ms": 3229.17
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "FINAL_ANSWER",
    "thought": "Đã có kết quả tra cứu nên tôi sẽ tổng hợp câu trả lời.",
    "output": "[Mock Agent Response]: Đã tra cứu thành công thông tin của sinh viên SV2026001.",
    "latency_ms": 2289.22
  },
  {
    "step": 1,
    "query": "Tôi là sinh viên SV2026001. Hãy đặt lịch hẹn tư vấn học vụ với PGS.TS Nguyễn Văn A vào lúc 14:00 ngày 15/09/2026.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026001",
      "datetime_str": "14:00 15/09/2026",
      "advisor_name": "PGS.TS Nguyễn Văn A"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026001-99",
      "student_id": "SV2026001",
      "datetime": "14:00 15/09/2026",
      "advisor": "PGS.TS Nguyễn Văn A",
      "message": "Đặt lịch thành công cho sinh viên SV2026001 với PGS.TS Nguyễn Văn A vào lúc 14:00 15/09/2026."
    },
    "latency_ms": 2956.55
  },
  {
    "step": 2,
    "query": "Tôi là sinh viên SV2026001. Hãy đặt lịch hẹn tư vấn học vụ với PGS.TS Nguyễn Văn A vào lúc 14:00 ngày 15/09/2026.",
    "action_type": "FINAL_ANSWER",
    "thought": "Đã nhận kết quả đặt lịch thành công, tôi sẽ thông báo kết quả cho người dùng.",
    "output": "[Mock Agent Response]: Lịch hẹn tư vấn đã được đặt thành công theo yêu cầu.",
    "latency_ms": 2094.02
  },
  {
    "step": 1,
    "query": "Tôi là sinh viên SV2026002. Hãy tra cứu cố vấn học tập của tôi, sau đó đặt lịch hẹn với cố vấn đó vào lúc 09:30 ngày 16/09/2026.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026002"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026002",
      "data": {
        "full_name": "Trần Thị Bình",
        "class": "AI-K4",
        "gpa": 3.6,
        "email": "binh.tt@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "TS. Lê Thị B"
      }
    },
    "latency_ms": 2178.9
  },
  {
    "step": 2,
    "query": "Tôi là sinh viên SV2026002. Hãy tra cứu cố vấn học tập của tôi, sau đó đặt lịch hẹn với cố vấn đó vào lúc 09:30 ngày 16/09/2026.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026002",
      "datetime_str": "09:30 16/09/2026",
      "advisor_name": "TS. Lê Thị B"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026002-99",
      "student_id": "SV2026002",
      "datetime": "09:30 16/09/2026",
      "advisor": "TS. Lê Thị B",
      "message": "Đặt lịch thành công cho sinh viên SV2026002 với TS. Lê Thị B vào lúc 09:30 16/09/2026."
    },
    "latency_ms": 2012.24
  },
  {
    "step": 3,
    "query": "Tôi là sinh viên SV2026002. Hãy tra cứu cố vấn học tập của tôi, sau đó đặt lịch hẹn với cố vấn đó vào lúc 09:30 ngày 16/09/2026.",
    "action_type": "FINAL_ANSWER",
    "thought": "Đã nhận kết quả đặt lịch thành công, tôi sẽ thông báo kết quả cho người dùng.",
    "output": "[Mock Agent Response]: Lịch hẹn tư vấn đã được đặt thành công theo yêu cầu.",
    "latency_ms": 2212.41
  },
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên có mã SV9999999.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV9999999"
    },
    "observation": {
      "status": "NOT_FOUND",
      "message": "Không tìm thấy dữ liệu sinh viên có mã 'SV9999999'"
    },
    "latency_ms": 2178.86
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên có mã SV9999999.",
    "action_type": "FINAL_ANSWER",
    "thought": "Tool trả về NOT_FOUND nên tôi phải thông báo chính xác và không bịa đặt dữ liệu.",
    "output": "Không tìm thấy dữ liệu sinh viên có mã 'SV9999999'. Vui lòng kiểm tra lại mã sinh viên.",
    "latency_ms": 2165.45
  }
]

---

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [ ] Đã điền API Key thật trong `.env` và xác nhận Agent chạy mượt mà trên LLM API thật (Gemini/OpenAI).
- **Tổng số Test Cases đã chạy thành công:** ___ / 5 test cases.
- **Số lượt gọi Tool qua MCP Server chính xác:** ___ lượt.
- **Kết quả đẩy Repo nộp bài:** [ ] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!
