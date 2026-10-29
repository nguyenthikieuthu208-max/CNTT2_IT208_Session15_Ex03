**Bài 7: Giải cứu dữ liệu nhóm CampusPay**

### **Phần 1: Phân tích vấn đề**

#### **Câu 1: Bảng rủi ro**

| Hạng mục | Rủi ro | Nguyên nhân | Xếp loại |
| ----- | ----- | ----- | ----- |
| Business Plan (20 trang) | Rất cao, gần như mất 100% | **Công nghệ \+ tổ chức:** chỉ lưu trên 1 laptop, không có bản sao | Cao |
| Mã nguồn giao diện (HTML/CSS) | Cao | **Công nghệ:** USB dễ hỏng, mất, không có bản sao | Cao |
| Database test user (Excel) | Cao (mất dữ liệu 2 tuần gần nhất) | **Tổ chức:** để trên máy cá nhân, không có quy trình sao lưu định kỳ | Cao |
| Slide Pitch (PowerPoint) | Trung bình | **Tổ chức:** còn bản trên Zalo nhưng đặt tên `v7_final_FINAL` nên không rõ là bản mới nhất | Trung bình |
| Tài liệu SRS (Word) | Trung bình | **Tổ chức:** gửi qua email nhiều lần, không kiểm soát được phiên bản | Trung bình |

#### **Câu 2: Hiện tượng “23 phiên bản file”**

* **Tên kỹ thuật:** **Xung đột phiên bản (Version Conflict)** hay **phân mảnh dữ liệu (Data Fragmentation)**, tức là thiếu “một nguồn dữ liệu duy nhất” (Single Source of Truth)  
* **Nguyên nhân:** Mỗi lần sửa, thành viên gửi file qua Zalo/email, trưởng nhóm tải về, sửa rồi gửi lại. Mỗi lần gửi lại tạo ra thêm một bản sao độc lập trên một thiết bị khác, nên số bản tăng dần và không có hệ thống nào theo dõi bản nào mới nhất  
* **Hậu quả với dự án phần mềm thực tế:**  
  * Không biết bản nào đúng, dễ nộp nhầm bản cũ  
  * Hai người sửa cùng lúc sẽ ghi đè và làm mất công của nhau  
  * Tốn thời gian ghép, so sánh file thủ công.  
  * Code và tài liệu (SRS, test case) không khớp nhau, dẫn đến lỗi và sai yêu cầu  
  * Khi có sự cố (như hỏng ổ cứng) thì không thể khôi phục đầy đủ

### **Phần 2: Sửa lỗi & Nghiệm thu**

#### **Câu 3: Quy trình Hybrid Workflow**

BƯỚC 1: LOCAL              BƯỚC 2: AUTO SYNC           BƯỚC 3: CLOUD  
Máy cá nhân        \---\>    Google Drive for     \---\>   Google Drive  
(làm việc nhanh,           Desktop / OneDrive          (lưu trữ chung,  
 sửa file, Ctrl+S)         (tự đẩy bản sao             Version History,  
                            mỗi khi Ctrl+S)             tự động sao lưu)

* **Phần mềm:** Google Drive for Desktop (hoặc OneDrive), bật **Files On-Demand** để tiết kiệm SSD  
* **Thư mục được Sync** (thư mục `CampusPay` chung, chia sẻ cho cả 5 thành viên):  
  * `/Plan` (Business Plan)  
  * `/Source` (mã nguồn HTML/CSS)  
  * `/TestData` (Excel test user)  
  * `/Pitch` (slide)  
  * `/SRS` (tài liệu Word)  
* **Thư mục KHÔNG nên Sync:** thư mục game, phim, nhạc, file cài đặt, thư mục tạm/cache. Các thư mục này nặng, không liên quan đến dự án, gây tốn dung lượng và làm chậm quá trình đồng bộ  
* **Quy ước:** chỉ giữ **một file duy nhất** cho mỗi tài liệu, bỏ kiểu đặt tên `v7_final_FINAL`; dùng **Version History** để xem lại bản cũ; chia sẻ bằng link thay vì gửi file qua Zalo/email

#### **Câu 4: Bẫy dữ liệu khi gập máy**

* **Biểu tượng hiển thị:** Nếu Sync chưa xong, file hiện biểu tượng **mũi tên xoay vòng (đang đồng bộ / Syncing)** thay vì dấu **tích xanh (Synced)**.  
* **Hệ thống xử lý:** Khi gập máy, quá trình Sync **bị ngắt hoặc tạm dừng**, nên bản mới nhất **chưa lên Cloud**. Cloud vẫn giữ bản cũ. Khi mở máy lại và có mạng, hệ thống mới tiếp tục đồng bộ. Nếu máy hỏng hoặc mất trong lúc đó thì bản sửa mới nhất sẽ mất  
* **Thành viên nên làm:**  
  1. Sau khi Ctrl+S, **chờ biểu tượng chuyển sang dấu tích xanh (Synced)** rồi mới gập máy  
  2. Kiểm tra biểu tượng Drive/OneDrive ở khay hệ thống, đảm bảo hiện trạng thái “Up to date”  
  3. Đảm bảo có kết nối mạng ổn định  
  4. Có thể mở Google Drive trên web để xác nhận file đã cập nhật

