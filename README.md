# Đề tài: Trang tuyển dụng nhân sự CNTT

> **Nhóm 1:**
> * **Hoàng Văn Chính - B23DCKH011**
> * **Nguyễn Ngọc Dương - B23DCCN221**
> * **Dương Thùy An - B21DCCN132**
> * **Lê Thị Thùy Trang - B23DCKH119**
> * **Đỗ Minh Hoàng - B23DCCN328**

## A. Giới thiệu đề tài

Hiện tại nhu cầu tuyển dụng kỹ sư phần mềm, QA, DevOps, Data… đang rất lớn, đồng thời ứng viên cũng cần nền tảng minh bạch để tìm đúng công việc phù hợp. Nhóm xây dựng **Hệ thống tuyển dụng nhân sự ngành CNTT** (Job Board + AI Matching) nhằm:

* Giúp **người tìm việc** khám phá cơ hội việc làm, lọc theo tiêu chí chi tiết và được **đề xuất tự động** dựa trên CV/năng lực.
* Hỗ trợ **nhà tuyển dụng (HR)** đăng tin, quản lý ứng viên, xem hồ sơ/CV, chấm điểm phù hợp và tối ưu quy trình tuyển.

Hệ thống hướng tới trải nghiệm hiện đại, thời gian phản hồi nhanh, tính năng AI diễn giải lý do phù hợp (explainable matching).

## B. Đối tượng sử dụng

| Đối tượng                              | Mô tả                                                                                | Mục tiêu chính                                                                         |
| -------------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| **Người tìm việc**                     | Ứng viên ngành CNTT (Fresher/Junior/Mid/Senior; FE/BE/Full‑stack, QA, DevOps, Data…) | Tìm, lọc, theo dõi việc làm; nộp hồ sơ nhanh; nhận gợi ý phù hợp; theo dõi trạng thái. |
| **Nhà tuyển dụng (HR/Hiring Manager)** | Doanh nghiệp, phòng HR, Tech Lead                                                    | Đăng tin, quản lý pipeline ứng viên, chấm điểm, trao đổi, xuất báo cáo.                |

## C. Thiết kế chi tiết của hệ thống

### I. Tính năng CRUD

#### 1. Người tìm việc (Job Seeker)

* **Create**:
  * Tạo tài khoản: thông tin cơ bản, sdt, email, kỹ năng, mức lương mong muốn, CV
  * Bookmark/Saved Jobs; Tạo **Job Alert** theo tiêu chí.
  * Ứng tuyển (Application): nộp file CV

* **Read**:
  * Danh sách việc làm, chi tiết tin tuyển; **lọc** theo: title/stack (React, Node), seniority, lương, loại hình (Full‑time/Part-time/Remote), địa điểm, JD keyword.
  * Đề xuất việc làm từ AI + **giải thích lý do** (skills trùng khớp, kinh nghiệm, từ khóa, similarity score).
  * Trạng thái hồ sơ (chưa duyệt, hẹn phỏng vấn, từ chối), lịch phỏng vấn.

* **Update**:
  * Sửa profile, kỹ năng, mức lương mong muốn; cập nhật CV; bật/tắt hiển thị tìm việc.
  * Quản lý Job Alert; xác nhận lịch phỏng vấn từ HR.

* **Delete**:
  * Xóa hồ sơ/ứng tuyển; gỡ bookmark;

#### 2. Nhà tuyển dụng (HR)

* **Create**:
  * Tạo công ty, **đăng tin tuyển** (JD, yêu cầu kỹ năng, salary range, skill keywords), pipeline stages.
  * Hẹn lịch phỏng vấn và gửi đến ứng viên; ghi chú nội bộ.

* **Read**:
  * Danh sách ứng viên theo tin tuyển; **chấm điểm phù hợp** (AI score + HR score);
  * Chi tiết file CV của ứng viên

* **Update**:
  * Sửa JD, trạng thái tin (Đang mở/Tạm dừng/Đóng);
  * Cập nhật trạng thái ứng viên (từ chối, hẹn phỏng vấn, chưa duyệt)
  * Cập nhật kết quả vòng phỏng vấn.

* **Delete**:
  * Ẩn/Xóa tin tuyển;

### II. Dòng chảy nghiệp vụ (User Flows)

1. **Ứng viên khám phá & ứng tuyển**: Tạo hồ sơ → upload CV → lọc job → xem chi tiết → đề xuất từ AI → Ứng tuyển → theo dõi trạng thái → nhận lịch phỏng vấn.
2. **HR đăng tin & sàng lọc**: Tạo công ty/JD → publish → nhận ứng viên → AI chấm điểm & gom nhóm theo kỹ năng → HR review → mời phỏng vấn → phản hồi → offer.
3. **AI gợi ý hai chiều**:

   * Cho ứng viên: gợi ý jobs phù hợp.
   * Cho HR: gợi ý ứng viên phù hợp từ toàn bộ pool + tiêu chí bắt buộc (must‑have).


---
---

### III. Tính năng AI Matching (khung kỹ thuật)

* **CV Parsing**: trích xuất text từ PDF/DOCX; OCR khi cần; chuẩn hóa format.
* **Entity & Skill Extraction**: NER (tên công nghệ, framework, ngôn ngữ, năm kinh nghiệm, học vấn, chứng chỉ); **chuẩn hóa skill ontology** (ví dụ: JS → JavaScript; ReactJS ≈ React).
* **Vector Embedding**: mã hóa CV và JD vào không gian vector; dùng semantic search để tính **similarity score** (cosine similarity); kết hợp **rule‑based** (must‑have: location, visa, level, shift).
* **Ranking**: score tổng hợp = w1*semantic + w2*keyword overlap + w3*experience gap + penalty(missing must‑have).
* **Explainability**: top‑k kỹ năng trùng, đoạn CV khớp với yêu cầu JD (highlight); lý do trượt.
* **Feedback Loop**: học từ hành vi HR (shortlist/ reject) để điều chỉnh trọng số; A/B test.
* **Chống gian lận**: phát hiện **keyword stuffing**, mô tả CV bất thường; cảnh báo.

---

### IV. Mô hình dữ liệu (ER khái quát)

**Thực thể chính**

* `User`(id, name, email, role[seeker|hr|admin], phone, status, created_at)
* `Company`(id, name, website, size, industry, location, verified)
* `JobPost`(id, company_id, title, level, employment_type, location, salary_min, salary_max, currency, description, requirements, skills[], status, created_at, expires_at)
* `CV`(id, user_id, file_url, parsed_text, skills[], years_experience, education, projects, updated_at)
* `Application`(id, job_id, user_id, cv_id, cover_letter, stage, score_ai, score_hr, notes, created_at)
* `Interview`(id, application_id, schedule_at, mode, interviewer, feedback, result)
* `Message`(id, from_user, to_user, application_id, body, created_at, read)
* `SavedJob`(id, user_id, job_id, created_at)
* `Notification`(id, user_id, type, payload, read, created_at)
* `MatchFeature`(id, application_id, feature_name, feature_value)

**Khóa ngoại**: `JobPost.company_id → Company.id`; `Application.job_id → JobPost.id`; `Application.user_id → User.id`; `CV.user_id → User.id`.

