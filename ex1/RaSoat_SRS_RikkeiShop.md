# RÀ SOÁT TỔNG HỢP BẢN THẢO SRS – RIKKEISHOP
## Phân hệ: Giỏ hàng & Thanh toán

> **Chuẩn áp dụng:** IEEE 830-1998 · **Đặc tính vàng rà soát:** Complete (Đầy đủ), Verifiable (Kiểm chứng được)

---

## 0. Kiến thức áp dụng

| Chương IEEE 830 | Mục liên quan | Chứa nội dung gì |
|---|---|---|
| **1. Introduction** | **1.3** Definitions, Acronyms, Abbreviations | Thuật ngữ, định nghĩa, từ viết tắt |
| **2. Overall Description** | **2.2** Product Functions | Bức tranh tổng thể các chức năng: danh sách Actor – Use Case, Use Case Diagram |
| **3. Specific Requirements** | **3.2** Functional Requirements · **3.3** Performance Requirements | Yêu cầu chi tiết có mã ID; sơ đồ minh họa chi tiết (Sequence, Activity) đặt cạnh yêu cầu mà nó minh họa |

| Đặc tính vàng | Câu hỏi kiểm tra |
|---|---|
| **Complete** | Yêu cầu đã mô tả phản ứng của hệ thống với **mọi** đầu vào, gồm cả đầu vào **không hợp lệ** chưa? |
| **Verifiable** | Có thể viết một test case cho kết quả **Đạt/Không đạt** rõ ràng bằng con số đo được không? |

---

## BƯỚC 1 — Xác định đúng vị trí IEEE 830

| Nội dung | Vị trí đề xuất | Đúng/Sai — vị trí đúng nếu sai |
|---|---|---|
| Mục A (định nghĩa "Giỏ hàng") | 3.2 Functional Requirements | **Sai — đúng là 1.3 Definitions, Acronyms, Abbreviations** (Chương 1 – Introduction). Định nghĩa là thuật ngữ để mọi bên hiểu thống nhất, không phải yêu cầu chức năng. |
| Mục B (3 yêu cầu chức năng) | 3.2 Functional Requirements | Đúng |
| Danh sách Actor/Use Case tổng thể | **2.2 Product Functions** (Chương 2 – Overall Description) | — *Lý do:* đây là cái nhìn **tổng quan** hệ thống làm được gì và cho ai, chưa đi vào chi tiết từng yêu cầu. |
| Sơ đồ Sequence áp mã giảm giá (minh họa REQ-02) | **3.2 Functional Requirements**, đặt ngay dưới REQ-02 | — *Lý do:* sơ đồ mô tả **chi tiết luồng tương tác** của một yêu cầu cụ thể nên phải nằm cùng chỗ với yêu cầu đó (Chương 3 – Specific Requirements). |

---

## BƯỚC 2 — Chuẩn hóa yêu cầu theo đặc tính vàng

| Mã | Đặc tính vàng bị vi phạm | Viết lại đạt chuẩn |
|---|---|---|
| **REQ-02** | **Complete (Đầy đủ).** Yêu cầu chỉ mô tả trường hợp mã **hợp lệ** (happy path). Nó bỏ sót cách hệ thống phản ứng khi mã **không tồn tại, hết hạn, hết lượt dùng** hoặc **đơn chưa đạt giá trị tối thiểu**. Nó cũng không nói số tiền giảm được tính và giới hạn thế nào. | **REQ-02 – Áp dụng mã giảm giá.** Khi khách hàng nhập mã giảm giá và nhấn "Áp dụng", hệ thống **phải** kiểm tra mã qua PromoService: <br>**(a) Mã hợp lệ:** trừ số tiền giảm vào tổng hóa đơn, trong đó số tiền giảm = mức giảm của mã và **không vượt quá** tổng tiền hàng (tổng sau giảm ≥ 0đ). Hiển thị dòng "Giảm giá: –X đ" và tổng mới. <br>**(b) Mã không hợp lệ:** **không** thay đổi tổng hóa đơn và hiển thị đúng thông báo theo từng trường hợp: <br>• Mã không tồn tại → "Mã giảm giá không tồn tại." <br>• Mã hết hạn → "Mã giảm giá đã hết hạn vào ngày dd/mm/yyyy." <br>• Mã hết lượt sử dụng → "Mã giảm giá đã hết lượt sử dụng." <br>• Đơn chưa đạt giá trị tối thiểu → "Đơn hàng cần tối thiểu Y đ để dùng mã này." <br>Mỗi đơn hàng chỉ áp dụng **tối đa 1** mã giảm giá. <br>*(Sơ đồ Sequence áp mã giảm giá được đặt ngay dưới yêu cầu này.)* <br><br>**Tiêu chí kiểm chứng:** với giỏ hàng 500.000đ, chạy 5 test case: mã giảm 50.000đ còn hạn cho tổng = **450.000đ**; mã không tồn tại, mã hết hạn, mã hết lượt và mã yêu cầu đơn tối thiểu 1.000.000đ đều giữ tổng = **500.000đ** và hiển thị **đúng** thông báo tương ứng. |
| **REQ-03** | **Verifiable (Kiểm chứng được).** "Nhanh chóng" và "ổn định" là từ cảm tính, không có con số nên không thể viết test case Đạt/Không đạt. *(Đây là yêu cầu hiệu năng, nên chuyển sang **3.3 Performance Requirements**.)* | **REQ-03 (PER-01) – Hiệu năng xử lý thanh toán.** Từ lúc khách hàng nhấn "Thanh toán" đến lúc hiển thị kết quả (thành công/thất bại), hệ thống **phải** phản hồi trong **≤ 3 giây** cho **95%** giao dịch (p95), khi có **500 người dùng thanh toán đồng thời**. Tỷ lệ giao dịch lỗi do hệ thống (HTTP 5xx, timeout) phải **< 0,1%**. Phân hệ thanh toán phải có độ sẵn sàng **≥ 99,9%/tháng** (thời gian ngừng ≤ 43 phút/tháng). <br><br>**Tiêu chí kiểm chứng:** kiểm thử tải bằng JMeter với 500 người dùng ảo trong 30 phút, báo cáo có **p95 ≤ 3 giây** và **tỷ lệ lỗi < 0,1%**; số liệu giám sát 1 tháng vận hành cho thấy độ sẵn sàng **≥ 99,9%**. |

---

## Tóm tắt kết quả rà soát

| # | Vấn đề | Cách xử lý |
|---|---|---|
| 1 | Định nghĩa "Giỏ hàng" đặt sai ở 3.2 | Chuyển lên **1.3 Definitions** |
| 2 | Danh sách Actor/Use Case chưa gắn vào tài liệu | Đặt tại **2.2 Product Functions** |
| 3 | Sơ đồ Sequence áp mã chưa gắn vào tài liệu | Đặt tại **3.2**, ngay dưới **REQ-02** |
| 4 | REQ-02 thiếu tình huống mã không hợp lệ | Bổ sung 4 trường hợp lỗi và giới hạn tiền giảm → đạt **Complete** |
| 5 | REQ-03 dùng từ cảm tính | Gắn chỉ số p95, số người dùng đồng thời, tỷ lệ lỗi, độ sẵn sàng → đạt **Verifiable** |
