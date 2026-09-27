# XÂY DỰNG VÀ RÀ SOÁT KHUNG SRS – RIKKEIFOOD
## Phân hệ: Đặt món tại bàn

> **Chuẩn áp dụng:** IEEE 830-1998 · **Đặc tính vàng:** Unambiguous, Verifiable, Complete

---

## 0. Kiến thức áp dụng

| Loại yêu cầu | Câu hỏi nhận diện | Vị trí IEEE 830 thường gặp |
|---|---|---|
| **Chức năng (Functional)** | Hệ thống **làm gì**? Có actor thực hiện một hành động cụ thể. | 3.2 Functional Requirements |
| **Phi chức năng (Non-functional)** | Hệ thống làm **tốt đến mức nào**, hoặc **trong điều kiện nào**? | 3.3 Performance · 3.6 System Attributes (Security, Usability…) · 2.4 Constraints |

**Khung mục lục SRS sau khi tổ chức lại:**

```
1. Introduction
   1.1 Purpose · 1.2 Scope · 1.3 Definitions (QR bàn, Thiết bị bếp…) · 1.4 References · 1.5 Overview
2. Overall Description
   2.1 Product Perspective
   2.2 Product Functions ............ ← Use Case Diagram tổng thể
   2.3 User Characteristics (Khách hàng, Nhân viên bếp)
   2.4 Constraints .................. ← Ghi chú 4 (trình duyệt hỗ trợ)
   2.5 Assumptions & Dependencies
3. Specific Requirements
   3.1 External Interfaces
   3.2 Functional Requirements ...... ← Ghi chú 1, Ghi chú 3
   3.3 Performance Requirements ..... ← Ghi chú 2
   3.4 Logical Database Requirements  ← ERD Đơn hàng – Món ăn
   3.5 Design Constraints
   3.6 Software System Attributes ... ← Ghi chú 5 (Security)
```

---

## BƯỚC 1 — Phân loại 5 ghi chú và xác định vị trí IEEE 830

| Ghi chú | Loại yêu cầu (Chức năng/Phi chức năng) | Vị trí IEEE 830 đề xuất |
|---|---|---|
| Ghi chú 1 | Chức năng (Functional) | 3.2 Functional Requirements |
| Ghi chú 2 | **Phi chức năng (Non-functional) – Hiệu năng** *(nói về tốc độ hiển thị, không phải hành động mới)* | **3.3 Performance Requirements** |
| Ghi chú 3 | **Chức năng (Functional)** *(actor Nhân viên bếp thực hiện hành động "xác nhận chế biến xong")* | **3.2 Functional Requirements** |
| Ghi chú 4 | **Phi chức năng (Non-functional) – Ràng buộc môi trường vận hành** *(giới hạn nền tảng trình duyệt hệ thống phải chạy được)* | **2.4 Constraints** |
| Ghi chú 5 | **Phi chức năng (Non-functional) – Bảo mật** *(mã hóa dữ liệu khi truyền)* | **3.6 Software System Attributes – Security** |

---

## BƯỚC 2 — Xác định vị trí cho 2 sơ đồ đã có sẵn

| Sơ đồ | Vị trí IEEE 830 đề xuất | Lý do |
|---|---|---|
| Use Case Diagram tổng thể | **2.2 Product Functions** (Chương 2 – Overall Description) | Sơ đồ cho **cái nhìn tổng quan**: phân hệ có những chức năng nào và ai sử dụng (2 Actor: Khách hàng, Nhân viên bếp). Nó chưa đi vào chi tiết từng yêu cầu nên thuộc phần mô tả tổng thể. |
| ERD Đơn hàng – Món ăn | **3.4 Logical Database Requirements** (Chương 3 – Specific Requirements) | ERD đặc tả **cấu trúc dữ liệu logic**: các thực thể Đơn hàng, Món ăn, thuộc tính và quan hệ giữa chúng. Đây là yêu cầu chi tiết về lưu trữ, đúng nội dung của mục 3.4. |

---

## BƯỚC 3 — Viết lại các ghi chú vi phạm đặc tính vàng

| Ghi chú | Đặc tính vàng bị vi phạm | Viết lại đạt chuẩn (có chỉ số/điều kiện cụ thể) |
|---|---|---|
| **Ghi chú 2** | **Verifiable (Kiểm chứng được).** Từ "nhanh chóng" không có con số, nên QA không thể viết test case Đạt/Không đạt. | **PER-01 – Thời gian hiển thị thực đơn.** Sau khi khách quét mã QR trên bàn, hệ thống **phải** hiển thị đầy đủ thực đơn của nhà hàng (tên món, giá, ảnh thu nhỏ) trong **≤ 2 giây** cho **95%** lượt truy cập (p95), với mạng di động 4G (tốc độ tải xuống ≥ 10 Mbps) và **300 khách truy cập đồng thời** trên mỗi nhà hàng. Nếu quá **5 giây** chưa tải xong, hệ thống hiển thị thông báo "Đang tải thực đơn, vui lòng chờ" kèm nút "Thử lại". <br><br>**Tiêu chí kiểm chứng:** kiểm thử tải bằng JMeter với 300 người dùng ảo, giả lập mạng 4G, báo cáo có p95 ≤ 2 giây. |
| **Ghi chú 4** | **Unambiguous (Không mơ hồ).** "Hoạt động tốt", "phổ biến" và "quá cũ" mỗi người hiểu một kiểu. Dev, QA và khách hàng sẽ tranh cãi trình duyệt nào phải hỗ trợ. *(Hệ quả là yêu cầu cũng không kiểm chứng được.)* | **CON-01 – Trình duyệt được hỗ trợ.** Hệ thống **phải** hiển thị đúng bố cục và thực hiện đủ mọi chức năng trên các trình duyệt di động sau: **Chrome for Android ≥ 110**, **Safari iOS ≥ 16**, **Samsung Internet ≥ 20**, ở màn hình rộng **360–430 px**. Với trình duyệt **thấp hơn** các phiên bản trên, hệ thống **phải** hiển thị thông báo "Trình duyệt của bạn chưa được hỗ trợ, vui lòng cập nhật lên phiên bản mới nhất" thay vì hiển thị lỗi giao diện. <br><br>**Tiêu chí kiểm chứng:** chạy bộ test chức năng (quét QR → xem thực đơn → đặt món) trên 3 trình duyệt ở đúng phiên bản tối thiểu thì **100%** test case đạt. Mở trên Chrome 100 thì hiển thị **đúng** thông báo chưa hỗ trợ. |
