# Agent Skills — Bộ Kỹ năng Tùy chỉnh cho AI Coding Assistant

Đây là kho lưu trữ bộ kỹ năng (Skills) toàn cục được thiết kế và tùy chỉnh riêng cho trợ lý lập trình AI (như Antigravity/Gemini Agent). Bộ kỹ năng này giúp chuẩn hóa quy trình làm việc từ khâu lập kế hoạch, code, kiểm thử cho đến viết tài liệu cho bất kỳ dự án phần mềm nào.

## 🚀 Danh sách 4 Kỹ năng (Skills) Cốt lõi

Bộ kỹ năng này bao gồm 4 công cụ chuyên biệt hoạt động bổ trợ lẫn nhau:

### 1. `implementation-planner` (Lập kế hoạch triển khai)
* **Vai trò:** Phân tích yêu cầu tính năng mới, spec hoặc bug phức tạp để phác thảo sơ đồ thực hiện.
* **Đầu ra:** File kế hoạch `implementation_plan.md` chi tiết để người dùng review và phê duyệt, kèm danh sách công việc `task.md` (TODO list) để theo dõi tiến độ.
* **Nguyên tắc:** Thực hiện trước khi sửa code nhằm giảm thiểu rủi ro sai lệch kiến trúc.

### 2. `thoughtful-coder` (Lập trình cẩn trọng)
* **Vai trò:** Hướng dẫn Agent chỉnh sửa mã nguồn một cách "ngoại khoa" (surgical changes) — chỉ chỉnh sửa những dòng thực sự cần thiết cho task.
* **Thứ tự ưu tiên:** `Đúng đắn (Correctness)` $\rightarrow$ `Diff tối thiểu (Minimal diff)` $\rightarrow$ `Đồng bộ với dự án (Consistency)` $\rightarrow$ `Dễ xác minh (Verifiable)` $\rightarrow$ `Đơn giản (Simplicity)`.
* **Nguyên tắc:** Tận dụng tối đa các helper có sẵn, tránh tự vẽ thêm các cấu trúc trừu tượng phức tạp, dọn sạch import thừa sau khi code.

### 3. `architecture-docs` (Quản lý tài liệu kiến trúc)
* **Vai trò:** Đồng bộ và cập nhật tự động bộ nhớ hướng dẫn đại lý nằm trong thư mục `.agents/` của dự án dựa trên cấu trúc thực tế của codebase.
* **Đầu ra:** Các tài liệu hướng dẫn chuẩn hóa (`README.md`, `PROJECT_CONTEXT.md`, `AGENT_RULES.md`, `TESTING_GUIDE.md`).

### 4. `create-readme` (Viết tài liệu README dự án)
* **Vai trò:** Viết hoặc cập nhật file `README.md` chính của dự án dựa trên các bằng chứng thực tế trong code (tệp config, docker, lệnh chạy...).
* **Nguyên tắc:** Giới hạn tài liệu ngắn gọn, dễ hiểu và thực tế, không tự vẽ ra tính năng hoặc hướng dẫn không có thật.

---

## 💡 Quy trình Làm việc & Quy tắc Bắt buộc (Core Workflow)

Mọi Agent (bao gồm cả các subagent chạy ngầm) khi bắt đầu nhận nhiệm vụ trong bất kỳ repository nào đều phải tuân thủ quy trình sau:

### 🛠️ Nguyên tắc Khởi tạo Bộ nhớ Đại lý (`.agents/` Bootstrapping)
> [!IMPORTANT]
> Quy tắc tối quan trọng: Trước khi thực hiện bất kỳ công việc lập kế hoạch (`implementation-planner`) hoặc viết code (`thoughtful-coder`), Agent bắt buộc phải kiểm tra xem dự án đã có thư mục `.agents/` chưa. 
> * **Nếu chưa có:** Phải gọi ngay skill `architecture-docs` để phân tích và khởi tạo cấu trúc tài liệu `.agents/` cho dự án.
> * **Lý do:** Điều này giúp thiết lập ranh giới an toàn, quy ước code và cấu hình Docker của dự án ngay từ đầu, đảm bảo các subagent chạy ngầm sau đó đều có chung một nguồn tài liệu hướng dẫn và hoạt động chính xác.

### 🔍 Ưu tiên CodeGraph (CodeGraph First)
* Agent luôn phải ưu tiên sử dụng các MCP tools của **CodeGraph** (`codegraph_context`, `codegraph_search`, `codegraph_callers`,...) để tìm kiếm cấu trúc code trước khi đọc file thủ công bằng lệnh shell/grep để tiết kiệm tài nguyên và tăng độ chính xác.

---

## 📦 Cấu trúc Thư mục Kỹ năng

```text
skills/
├── architecture-docs/    # Chỉ dẫn viết tài liệu hướng dẫn kiến trúc đại lý
│   └── SKILL.md
├── create-readme/        # Chỉ dẫn tạo file README.md cho dự án
│   └── SKILL.md
├── implementation-planner/# Chỉ dẫn lập kế hoạch và thiết kế hệ thống
│   └── SKILL.md
└── thoughtful-coder/     # Chỉ dẫn code cẩn thận, tinh gọn và an toàn
    └── SKILL.md
```
