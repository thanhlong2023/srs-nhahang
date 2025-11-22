# CHƯƠNG 1: GIỚI THIỆU

## 1.1. Mục đích của tài liệu

### 1.1.1. Mục đích chính

Tài liệu Đặc tả Yêu cầu Phần mềm (Software Requirements Specification - SRS) này trình bày **chi tiết, đầy đủ và chính xác** các yêu cầu cho việc phát triển **Hệ thống Quản lý Nhà hàng PTIT (PTIT-RMS)**. Đây là tài liệu nền tảng để:

1. **Đội ngũ phát triển** hiểu rõ các yêu cầu nghiệp vụ và kỹ thuật cần thực hiện
2. **Nhà quản lý dự án** lập kế hoạch, phân bổ nguồn lực và giám sát tiến độ
3. **Kiểm thử viên** thiết kế test case và đảm bảo chất lượng sản phẩm
4. **Người dùng cuối** (quản lý nhà hàng, nhân viên) xác nhận hệ thống đáp ứng đúng nhu cầu thực tế
5. **Stakeholders** đánh giá tính khả thi và giá trị đầu tư của dự án

### 1.1.2. Phạm vi tài liệu

Tài liệu này bao gồm:

- **Phân tích nghiệp vụ**: Mô tả chi tiết quy trình vận hành nhà hàng hiện tại, các vấn đề tồn tại và giải pháp đề xuất
- **Yêu cầu chức năng**: Đặc tả đầy đủ các chức năng mà hệ thống phải cung cấp
- **Yêu cầu phi chức năng**: Các ràng buộc về hiệu năng, bảo mật, khả năng mở rộng
- **Thiết kế hệ thống**: Kiến trúc tổng thể, cơ sở dữ liệu, giao diện người dùng
- **Ràng buộc và giả định**: Các điều kiện tiên quyết và hạn chế của dự án

### 1.1.3. Đối tượng sử dụng tài liệu

| Đối tượng                  | Mục đích sử dụng               | Phần quan tâm chính  |
| -------------------------- | ------------------------------ | -------------------- |
| **Product Owner**          | Xác nhận yêu cầu nghiệp vụ     | Chương 1, 3, 5       |
| **System Analyst**         | Phân tích và thiết kế hệ thống | Tất cả các chương    |
| **Software Developer**     | Phát triển phần mềm            | Chương 3, 4, 5, 6    |
| **QA/Tester**              | Thiết kế test case             | Chương 3, 5          |
| **Database Administrator** | Thiết kế CSDL                  | Chương 4             |
| **UI/UX Designer**         | Thiết kế giao diện             | Chương 6             |
| **Project Manager**        | Quản lý dự án                  | Chương 1, 2, Phụ lục |

---

## 1.2. Bối cảnh dự án

### 1.2.1. Giới thiệu về Nhà hàng PTIT

**Nhà hàng PTIT** là một nhà hàng vừa và nhỏ với quy mô:

- **Diện tích**: 200-300m² trên 2 tầng
- **Sức chứa**: 80-100 khách đồng thời
- **Số lượng bàn**: 25-30 bàn (bao gồm bàn đơn, bàn đôi, bàn nhóm)
- **Thực đơn**: 60-80 món ăn thuộc các danh mục: Khai vị, Món chính, Tráng miệng, Đồ uống
- **Nhân sự**: 15-20 người (Quản lý, Thu ngân, Phục vụ, Bếp trưởng, Đầu bếp, Phụ bếp)
- **Khách hàng**: 150-200 khách/ngày (bữa trưa và tối)
- **Doanh thu**: Trung bình 30-50 triệu/ngày

**Loại hình kinh doanh:**

- Ăn tại chỗ (chiếm 85-90% doanh thu)
- Đặt bàn trước (chiếm 10-15%)
- Tiềm năng mở rộng: Giao hàng tận nơi, đặt món online

### 1.2.2. Tình hình hoạt động hiện tại

Hiện tại, nhà hàng PTIT đang vận hành theo mô hình **thủ công và bán tự động**:

#### A. Quy trình phục vụ khách hàng

```
[Khách đến] → [Nhân viên dẫn bàn] → [Gọi món (ghi giấy)] →
[Chuyển phiếu cho bếp] → [Bếp chế biến] → [Mang món ra] →
[Khách dùng bữa] → [Yêu cầu thanh toán] → [Thu ngân tính tiền thủ công] →
[Xuất hóa đơn viết tay/Excel] → [Khách ra về]
```

**Thời gian trung bình**: 5-8 phút từ gọi món đến nhận được món đầu tiên

#### B. Các công cụ đang sử dụng

1. **Ghi chép đơn hàng**:
   - Nhân viên phục vụ ghi trên giấy A5 in sẵn
   - Dễ bị nhòe, mất, khó đọc chữ viết tay
2. **Quản lý bàn ăn**:
   - Sơ đồ bàn vẽ trên bảng trắng
   - Dùng nam châm/sticker để đánh dấu trạng thái
3. **Quản lý tồn kho**:
   - File Excel riêng lẻ
   - Cập nhật thủ công cuối ngày
   - Không đồng bộ real-time
4. **Thanh toán**:
   - Máy tính bỏ túi
   - Hóa đơn viết tay hoặc in từ template Word/Excel
5. **Báo cáo doanh thu**:
   - Thu ngân tổng hợp cuối ngày bằng Excel
   - Gửi email cho quản lý

### 1.2.3. Các vấn đề đang tồn tại

#### Vấn đề 1: Sai sót trong đơn hàng (35% trường hợp)

**Biểu hiện:**

- Ghi sai món, sai số lượng (15%)
- Quên món, thiếu món (12%)
- Nhầm lẫn giữa các bàn (8%)

**Nguyên nhân:**

- Chữ viết tay khó đọc
- Giấy ghi chép bị nhòe, rách
- Tiếng ồn trong giờ cao điểm
- Nhân viên mới chưa quen

**Hậu quả:**

- Khách hàng không hài lòng
- Lãng phí nguyên liệu
- Mất thời gian xử lý khiếu nại
- Ảnh hưởng uy tín nhà hàng

**Ví dụ thực tế:**

```
Khách gọi: "2 phần phở bò, 1 phở gà, không hành"
Nhân viên ghi: "2 phở bò, 1 gà" (quên ghi chú "không hành")
→ Khách phàn nàn → Phải nấu lại → Tốn 10 phút
```

#### Vấn đề 2: Thiếu đồng bộ giữa các bộ phận (40% trường hợp)

**Biểu hiện:**

- Bếp không nhận được phiếu đặt món kịp thời (20%)
- Phục vụ không biết món đã sẵn sàng chưa (15%)
- Thu ngân phải hỏi lại phục vụ về đơn hàng (5%)

**Nguyên nhân:**

- Phiếu giấy dễ thất lạc
- Không có thông báo real-time
- Bếp và phòng phục vụ cách xa nhau

**Hậu quả:**

- Khách chờ lâu (trung bình 15-20 phút)
- Món bị nguội trước khi phục vụ
- Căng thẳng giữa các bộ phận

**Ví dụ thực tế:**

```
10:30 - Khách gọi món
10:32 - Nhân viên ghi phiếu
10:40 - Phiếu mới đến bếp (trễ 8 phút vì nhân viên quên mang vào)
10:55 - Món xong nhưng phục vụ không biết
11:05 - Khách phàn nàn → Món đã nguội
```

#### Vấn đề 3: Khó kiểm soát tồn kho nguyên liệu (50% trường hợp)

**Biểu hiện:**

- Không biết chính xác số lượng nguyên liệu còn lại (30%)
- Phát hiện hết hàng khi đã nhận đơn từ khách (15%)
- Lãng phí do nguyên liệu hết hạn (5%)

**Nguyên nhân:**

- Cập nhật Excel thủ công cuối ngày
- Không trừ tự động khi chế biến món
- Không có cảnh báo sắp hết hàng
- Nhiều người cập nhật → dữ liệu không nhất quán

**Hậu quả:**

- Chi phí nhập hàng cao (20-30% lãng phí)
- Phải từ chối khách khi hết món
- Tồn kho ứ đọng, hết hạn

**Số liệu thực tế:**

- Tỷ lệ lãng phí nguyên liệu: 25-30%/tháng
- Chi phí nhập hàng thừa: 5-8 triệu/tháng
- Số lần từ chối khách do hết món: 3-5 lần/tuần

#### Vấn đề 4: Thiếu hiệu quả trong xử lý hóa đơn (25% trường hợp)

**Biểu hiện:**

- Tính tiền mất nhiều thời gian (5-8 phút/hóa đơn)
- Sai sót trong tính toán (thuế, giảm giá)
- Khó tra cứu hóa đơn cũ

**Nguyên nhân:**

- Tính thủ công bằng máy tính bỏ túi
- Không lưu trữ điện tử
- Hóa đơn giấy dễ thất lạc

**Hậu quả:**

- Khách chờ lâu khi thanh toán (giờ cao điểm)
- Dễ bị tranh chấp về giá
- Khó đối soát doanh thu

#### Vấn đề 5: Báo cáo doanh thu chậm và không chính xác (100% trường hợp)

**Biểu hiện:**

- Báo cáo chỉ có vào cuối ngày (23:00-00:00)
- Dữ liệu không real-time
- Khó phân tích xu hướng kinh doanh

**Nguyên nhân:**

- Thu ngân phải tổng hợp thủ công
- Nhiều nguồn dữ liệu rời rạc
- Không có biểu đồ trực quan

**Hậu quả:**

- Quản lý không nắm được tình hình kinh doanh theo giờ
- Khó ra quyết định kịp thời (nhập hàng, điều chỉnh thực đơn)
- Mất thời gian làm báo cáo (2-3 giờ/ngày)

### 1.2.4. Nhu cầu số hóa

Dựa trên các vấn đề trên, nhà hàng PTIT cần một hệ thống quản lý toàn diện để:

✅ **Tự động hóa quy trình** từ gọi món → chế biến → thanh toán  
✅ **Đồng bộ thông tin** giữa các bộ phận real-time  
✅ **Quản lý tồn kho** chính xác và tự động  
✅ **Báo cáo doanh thu** tức thời với biểu đồ trực quan  
✅ **Nâng cao trải nghiệm khách hàng** với dịch vụ nhanh chóng, chính xác

---

## 1.3. Vấn đề cần giải quyết

### 1.3.1. Bảng tổng hợp vấn đề và giải pháp

| STT | Vấn đề hiện tại                                                                                    | Tác động                                                                  | Mức độ ưu tiên          | Giải pháp của PTIT-RMS                                                                                                             |
| --- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Sai sót đơn hàng thủ công** <br/>• Ghi sai món: 15% <br/>• Quên món: 12% <br/>• Nhầm bàn: 8%     | • Khách không hài lòng <br/>• Lãng phí nguyên liệu <br/>• Mất uy tín      | ⭐⭐⭐⭐⭐ <br/>Rất cao | **Số hóa toàn bộ quy trình đặt món** <br/>• Giao diện chọn món trực quan <br/>• Tự động lưu vào database <br/>• Không còn ghi giấy |
| 2   | **Thiếu đồng bộ giữa bộ phận** <br/>• Phiếu giấy thất lạc <br/>• Không có thông báo real-time      | • Khách chờ lâu (15-20 phút) <br/>• Món nguội <br/>• Căng thẳng nhân viên | ⭐⭐⭐⭐⭐ <br/>Rất cao | **Thông báo real-time qua WebSocket** <br/>• Bếp nhận đơn ngay lập tức <br/>• Phục vụ biết món sẵn sàng <br/>• Đồng bộ tự động     |
| 3   | **Khó kiểm soát tồn kho** <br/>• Cập nhật thủ công <br/>• Không có cảnh báo <br/>• Lãng phí 25-30% | • Chi phí cao <br/>• Từ chối khách <br/>• Nguyên liệu hết hạn             | ⭐⭐⭐⭐ <br/>Cao       | **Quản lý kho tự động** <br/>• Trừ nguyên liệu khi tạo đơn <br/>• Cảnh báo sắp hết <br/>• Báo cáo nhập-xuất-tồn                    |
| 4   | **Thanh toán chậm** <br/>• 5-8 phút/hóa đơn <br/>• Tính thủ công <br/>• Dễ sai sót                 | • Hàng đợi dài <br/>• Khách không hài lòng <br/>• Tranh chấp về giá       | ⭐⭐⭐⭐ <br/>Cao       | **Thanh toán tự động** <br/>• Tính tiền tức thời <br/>• Nhiều phương thức <br/>• In hóa đơn tự động                                |
| 5   | **Báo cáo chậm** <br/>• Chỉ có cuối ngày <br/>• Làm thủ công 2-3 giờ <br/>• Không real-time        | • Không nắm tình hình <br/>• Quyết định chậm <br/>• Mất thời gian         | ⭐⭐⭐ <br/>Trung bình  | **Báo cáo real-time** <br/>• Dashboard trực quan <br/>• Biểu đồ tự động <br/>• Xuất Excel/PDF                                      |
| 6   | **Khó đánh giá nhân viên** <br/>• Không có số liệu <br/>• Đánh giá chủ quan                        | • Khó phân ca <br/>• Không công bằng <br/>• Thiếu động lực                | ⭐⭐ <br/>Thấp          | **Báo cáo hiệu suất** <br/>• Số bàn phục vụ <br/>• Doanh thu mang lại <br/>• KPI rõ ràng                                           |

### 1.3.2. Mục tiêu cụ thể của hệ thống

#### Mục tiêu về Hiệu quả vận hành

| Chỉ số                          | Hiện tại     | Mục tiêu      | Cải thiện |
| ------------------------------- | ------------ | ------------- | --------- |
| Thời gian từ gọi món → nhận món | 15-20 phút   | **8-10 phút** | ⬇️ 50%    |
| Thời gian thanh toán            | 5-8 phút     | **1-2 phút**  | ⬇️ 70%    |
| Tỷ lệ sai sót đơn hàng          | 35%          | **< 5%**      | ⬇️ 85%    |
| Thời gian làm báo cáo           | 2-3 giờ/ngày | **Real-time** | ⬇️ 100%   |

#### Mục tiêu về Chi phí

| Chỉ số               | Hiện tại        | Mục tiêu                                   | Tiết kiệm          |
| -------------------- | --------------- | ------------------------------------------ | ------------------ |
| Lãng phí nguyên liệu | 25-30%          | **< 10%**                                  | 💰 5-8 triệu/tháng |
| Chi phí giấy tờ      | 2-3 triệu/tháng | **< 0.5 triệu**                            | 💰 2-3 triệu/tháng |
| Thời gian nhân viên  | 100%            | **70%** (tiết kiệm 30% để phục vụ tốt hơn) | ⏰ 30% năng suất   |

#### Mục tiêu về Trải nghiệm khách hàng

- ✅ Giảm thời gian chờ **50%**
- ✅ Tăng độ chính xác đơn hàng lên **95%+**
- ✅ Hỗ trợ đặt bàn online 24/7
- ✅ Gửi hóa đơn điện tử qua email/SMS
- ✅ Chương trình khách hàng thân thiết

---

## 1.4. Phạm vi dự án

### 1.4.1. Trong phạm vi (In-Scope) - Phiên bản 1.0

Hệ thống PTIT-RMS phiên bản 1.0 sẽ bao gồm các module sau:

#### A. Module Quản lý Bàn ăn & Khách hàng

**Quản lý Bàn ăn:**

- ✅ Tạo/Sửa/Xóa thông tin bàn ăn (số bàn, khu vực, sức chứa)
- ✅ Sơ đồ bàn trực quan theo tầng/khu vực (Floor plan)
- ✅ Hiển thị trạng thái real-time:
  - 🟢 Trống (Available)
  - 🔴 Đang phục vụ (Occupied)
  - 🟡 Đặt trước (Reserved)
- ✅ Lịch sử sử dụng bàn theo thời gian
- ✅ Thống kê hiệu suất theo bàn/khu vực

**Đặt bàn trước (Reservation):**

- ✅ Đặt bàn qua web interface
- ✅ Thông tin: Khách hàng, số người, thời gian, ghi chú đặc biệt
- ✅ Xác nhận đặt bàn qua SMS/Email
- ✅ Nhắc nhở tự động trước 1 giờ
- ✅ Tự động hủy nếu khách không đến sau 15 phút
- ✅ Quản lý trạng thái: Chờ xác nhận → Đã xác nhận → Hoàn tất/Hủy

**Quản lý Khách hàng:**

- ✅ CRUD thông tin khách hàng (Họ tên, SĐT, Email)
- ✅ Phân loại khách: Lẻ / VIP / Doanh nghiệp
- ✅ Lịch sử đơn hàng và tổng chi tiêu
- ✅ Tự động nâng cấp VIP (chi tiêu > 10 triệu)
- ✅ Ghi chú riêng cho từng khách (sở thích, dị ứng...)
- ✅ Tìm kiếm khách hàng theo SĐT/tên

#### B. Module Quản lý Thực đơn & Nguyên liệu

**Quản lý Thực đơn:**

- ✅ CRUD món ăn (Tên, mô tả, giá, hình ảnh)
- ✅ Phân loại món: Khai vị, Món chính, Món phụ, Tráng miệng, Đồ uống
- ✅ Quản lý giá bán và giá vốn
- ✅ Upload hình ảnh món (max 5MB, JPG/PNG)
- ✅ Trạng thái món: Đang kinh doanh / Tạm ngưng / Ngưng bán
- ✅ Đánh dấu món phổ biến (Popular items)
- ✅ Thời gian chế biến ước tính
- ✅ Tìm kiếm và lọc món theo danh mục/giá

**Quản lý Nguyên liệu:**

- ✅ CRUD nguyên liệu (Tên, đơn vị tính, giá nhập)
- ✅ Theo dõi số lượng tồn kho real-time
- ✅ Ngưỡng cảnh báo sắp hết cho từng nguyên liệu
- ✅ Thông tin nhà cung cấp và hạn sử dụng
- ✅ Nhập hàng, xuất hàng, điều chỉnh kho
- ✅ Lịch sử nhập/xuất chi tiết (audit trail)

**Công thức món ăn (Recipe):**

- ✅ Liên kết món với nguyên liệu cần dùng
- ✅ Định lượng chính xác (VD: 0.3kg thịt bò, 0.1kg hành)
- ✅ Tự động tính giá vốn món ăn
- ✅ Kiểm tra tồn kho trước khi cho phép gọi món
- ✅ Cảnh báo món không đủ nguyên liệu

#### C. Module Quản lý Đơn hàng & Thanh toán

**Tạo và Quản lý Đơn hàng:**

- ✅ Tạo đơn hàng mới cho bàn ăn
- ✅ Chọn món từ thực đơn (với hình ảnh)
- ✅ Điều chỉnh số lượng món (1-99)
- ✅ Ghi chú đặc biệt cho món (VD: "Không hành", "Ít cay")
- ✅ Tính tổng tiền tạm tính tự động
- ✅ Tự động trừ nguyên liệu khi tạo đơn
- ✅ Gửi thông báo đến bếp qua WebSocket

**Workflow trạng thái đơn hàng:**

```
Đang chế biến (Preparing)
    ↓
Sẵn sàng phục vụ (Ready)
    ↓
Đang dùng bữa (Serving)
    ↓
Hoàn tất (Completed)

    ↓ (chỉ từ Preparing)
Đã hủy (Cancelled - cần lý do)
```

**Cập nhật trạng thái:**

- ✅ Bếp trưởng xác nhận món đã xong → "Ready"
- ✅ Phục vụ xác nhận đã mang món ra → "Serving"
- ✅ Thu ngân xác nhận thanh toán → "Completed"
- ✅ Hủy đơn (chỉ khi Preparing, cần lý do cụ thể)
- ✅ Log mọi thay đổi trạng thái (audit log)

**Thanh toán:**

- ✅ Nhiều phương thức: Tiền mặt, Thẻ ngân hàng, Momo, ZaloPay, VNPay
- ✅ Áp dụng mã giảm giá/voucher
- ✅ Tính thuế VAT 8% tự động
- ✅ Tính: **Tổng = (Tổng món + VAT) - Giảm giá**
- ✅ Xuất hóa đơn (in giấy hoặc gửi email/SMS)
- ✅ Ghi nhận giao dịch đầy đủ
- ✅ Cập nhật bàn về trạng thái "Trống"
- ✅ Tích hợp Payment Gateway (cho thẻ/ví điện tử)

**Hủy đơn hàng:**

- ✅ Chỉ cho phép hủy đơn ở trạng thái "Preparing"
- ✅ Bắt buộc nhập lý do hủy
- ✅ Hoàn nguyên nguyên liệu vào kho
- ✅ Ghi log đầy đủ (ai hủy, khi nào, lý do gì)
- ✅ Cần phê duyệt từ Quản lý nếu món đã chế biến

#### D. Module Báo cáo & Thống kê

**Báo cáo Doanh thu:**

- ✅ Doanh thu theo thời gian (giờ/ngày/tuần/tháng/quý/năm)
- ✅ Doanh thu theo khu vực bàn
- ✅ Doanh thu theo nhân viên phục vụ
- ✅ Doanh thu theo phương thức thanh toán
- ✅ So sánh với kỳ trước (tăng/giảm %)
- ✅ Biểu đồ trực quan (Line chart, Bar chart, Pie chart)
- ✅ Xuất báo cáo: Excel, PDF, CSV

**Báo cáo Tồn kho:**

- ✅ Báo cáo Nhập-Xuất-Tồn theo kỳ
- ✅ Nguyên liệu sắp hết (dưới ngưỡng)
- ✅ Nguyên liệu tồn lâu (> 30 ngày không xuất)
- ✅ Nguyên liệu sắp hết hạn (< 7 ngày)
- ✅ Giá trị tồn kho (theo giá nhập)
- ✅ Chi phí nguyên liệu theo món/theo kỳ

**Báo cáo Món ăn:**

- ✅ Top món bán chạy (theo số lượng/doanh thu)
- ✅ Món ít người gọi (cân nhắc gỡ khỏi menu)
- ✅ Doanh thu theo danh mục món
- ✅ Tỷ suất lợi nhuận theo món (giá bán - giá vốn)
- ✅ Thời gian chế biến trung bình

**Báo cáo Nhân viên:**

- ✅ Số bàn phục vụ theo nhân viên
- ✅ Doanh thu mang lại theo nhân viên
- ✅ Số đơn hàng xử lý
- ✅ Đánh giá hiệu suất (KPI)
- ✅ Xếp hạng nhân viên theo tháng

**Dashboard tổng quan:**

- ✅ Doanh thu hôm nay (real-time)
- ✅ Số đơn hàng đã xử lý
- ✅ Số bàn đang phục vụ
- ✅ Cảnh báo nguyên liệu sắp hết
- ✅ Top 5 món bán chạy nhất
- ✅ Biểu đồ doanh thu theo giờ trong ngày

#### E. Module Quản lý Hệ thống

**Quản lý Người dùng:**

- ✅ CRUD tài khoản nhân viên
- ✅ 5 vai trò chính:
  - **Admin**: Full quyền, cấu hình hệ thống
  - **Manager**: Xem báo cáo, quản lý khách hàng, phê duyệt
  - **Cashier**: Thu ngân, thanh toán, xuất hóa đơn
  - **Server**: Phục vụ, tạo đơn hàng, cập nhật trạng thái
  - **Chef**: Bếp, xác nhận chế biến, quản lý thực đơn/kho
- ✅ Phân quyền chi tiết theo chức năng (RBAC)
- ✅ Quản lý ca làm việc

**Audit Log:**

- ✅ Ghi nhận mọi thao tác quan trọng:
  - Tạo/sửa/xóa đơn hàng
  - Thay đổi giá món
  - Thanh toán
  - Nhập/xuất kho
- ✅ Thông tin: Ai, làm gì, khi nào, ở đâu (IP)
- ✅ Không cho phép sửa/xóa log
- ✅ Tìm kiếm và lọc log

**Cấu hình hệ thống:**

- ✅ Thuế VAT (mặc định 8%)
- ✅ Giờ mở cửa/đóng cửa nhà hàng
- ✅ Thời gian cảnh báo đặt bàn (mặc định 1 giờ trước)
- ✅ Thời gian tự động hủy đặt bàn (mặc định 15 phút)
- ✅ Ngưỡng VIP (mặc định 10 triệu)
- ✅ Email/SMS template
- ✅ Thông tin nhà hàng (tên, địa chỉ, logo, hotline)

**Backup & Restore:**

- ✅ Backup tự động hàng ngày (2:00 AM)
- ✅ Backup thủ công bất kỳ lúc nào
- ✅ Khôi phục dữ liệu từ file backup
- ✅ Lưu trữ backup tối thiểu 30 ngày

#### F. Tính năng Tự động

**Tự động Trừ nguyên liệu:**

- ✅ Khi đơn hàng chuyển sang "Preparing"
- ✅ Tính tổng nguyên liệu cần dùng theo công thức
- ✅ Trừ số lượng tương ứng trong kho
- ✅ Ghi log vào inventory_transactions
- ✅ Rollback nếu có lỗi

**Cảnh báo Tồn kho:**

- ✅ Real-time khi nguyên liệu < ngưỡng
- ✅ Gửi notification trong hệ thống
- ✅ Gửi email cho Quản lý và Bếp trưởng
- ✅ Hiển thị badge cảnh báo trên menu

**Báo cáo Định kỳ:**

- ✅ Gửi báo cáo cuối ngày qua email (23:00)
- ✅ Nội dung: Doanh thu, món bán chạy, nguyên liệu xuất nhiều
- ✅ Gửi cho Quản lý và chủ nhà hàng
- ✅ Tự động lưu vào archive

**Marketing Tự động:**

- ✅ Gửi SMS/Email cảm ơn sau thanh toán
- ✅ Tặng mã giảm giá ngẫu nhiên (10-20%)
- ✅ Nhắc nhở khách hàng lâu không đến (> 3 tháng)
- ✅ Chúc mừng sinh nhật khách hàng VIP

**Tự động Cập nhật trạng thái:**

- ✅ Bàn "Reserved" → "Occupied" khi đến giờ hẹn
- ✅ Bàn "Occupied" → "Available" sau thanh toán
- ✅ Hủy đặt bàn tự động sau 15 phút không đến

### 1.4.2. Ngoài phạm vi (Out-of-Scope) - Để lại cho Phase 2/3

Các tính năng sau **KHÔNG** được triển khai trong phiên bản 1.0:

❌ **Tích hợp Hệ thống Kế toán bên ngoài:**

- SAP, Fast, MISA, Excel Accounting
- Lý do: Cần thời gian nghiên cứu API của từng hệ thống

❌ **Ứng dụng Mobile Native cho Khách hàng:**

- iOS/Android app để khách tự đặt món
- Lý do: Ưu tiên web app trước, mobile app sau
- Tạm thời: Khách có thể dùng mobile browser

❌ **Hệ thống Giao hàng tận nơi (Delivery):**

- Quản lý shipper, theo dõi đơn hàng giao
- Tích hợp bản đồ, tính phí ship
- Lý do: Nhà hàng chưa có dịch vụ delivery

❌ **Kitchen Display System (KDS) chuyên dụng:**

- Màn hình cảm ứng lớn cho bếp
- Máy in phiếu bếp nhiệt (thermal printer)
- Lý do: Chi phí phần cứng cao, ưu tiên phần mềm trước

❌ **Chương trình Tích điểm phức tạp:**

- Quy đổi điểm sang quà tặng
- Hệ thống đổi quà, tích điểm đa cấp
- Lý do: Nghiệp vụ phức tạp, cần thiết kế kỹ
- Tạm thời: Chỉ có phân loại khách (Lẻ/VIP)

❌ **Quản lý Nhà cung cấp & Mua hàng:**

- Đặt hàng tự động với nhà cung cấp
- Quản lý hợp đồng, đơn giá, công nợ
- Lý do: Hiện tại nhập hàng thủ công qua điện thoại

❌ **Quản lý Nhân sự (HR) đầy đủ:**

- Chấm công, tính lương, quản lý ca chi tiết
- Đào tạo, đánh giá năng lực
- Lý do: Quy mô nhỏ, chấm công thủ công

❌ **Thanh toán Online trực tiếp:**

- Khách thanh toán trước khi đến nhà hàng
- Liên kết ví điện tử của khách
- Lý do: Cần xác thực pháp lý, bảo mật cao

❌ **Booking Online công khai:**

- Khách không cần đăng ký vẫn đặt được
- Widget đặt bàn nhúng vào website
- Lý do: Ưu tiên khách hàng quen trước

❌ **Hệ thống Review & Rating:**

- Khách đánh giá món, dịch vụ
- Hiển thị đánh giá công khai
- Lý do: Cần kiểm duyệt, xử lý review ảo

❌ **Social Media Integration:**

- Đăng món lên Facebook/Instagram tự động
- Login bằng tài khoản mạng xã hội
- Lý do: Chưa cần thiết giai đoạn đầu

❌ **Multi-language (Đa ngôn ngữ):**

- Tiếng Anh, Tiếng Trung, Tiếng Nhật
- Lý do: Khách hàng chủ yếu là người Việt

❌ **Quản lý Sự kiện & Tiệc:**

- Đặt tiệc sinh nhật, hội nghị, buffet
- Quản lý menu set, báo giá riêng
- Lý do: Nhà hàng chưa có dịch vụ này

❌ **AI/Machine Learning:**

- Dự đoán nhu cầu nguyên liệu
- Gợi ý món cho khách dựa trên lịch sử
- Lý do: Cần tích lũy dữ liệu trước

❌ **Multi-branch (Đa chi nhánh):**

- Quản lý nhiều nhà hàng cùng lúc
- Đồng bộ dữ liệu giữa các chi nhánh
- Lý do: Hiện chỉ có 1 nhà hàng

**Lưu ý:** Các tính năng trên có thể được triển khai trong **Phase 2** (sau 6-12 tháng) hoặc **Phase 3** (sau 12-24 tháng) tùy theo nhu cầu thực tế và ngân sách.

---

## 1.5. Định nghĩa, Từ viết tắt và Thuật ngữ

### 1.5.1. Từ viết tắt (Abbreviations)

| Từ viết tắt  | Tiếng Anh đầy đủ                              | Tiếng Việt                      | Giải thích                               |
| ------------ | --------------------------------------------- | ------------------------------- | ---------------------------------------- |
| **SRS**      | Software Requirements Specification           | Đặc tả Yêu cầu Phần mềm         | Tài liệu mô tả chi tiết yêu cầu hệ thống |
| **PTIT-RMS** | PTIT Restaurant Management System             | Hệ thống Quản lý Nhà hàng PTIT  | Tên chính thức của dự án                 |
| **CRUD**     | Create, Read, Update, Delete                  | Tạo, Đọc, Cập nhật, Xóa         | 4 thao tác cơ bản với dữ liệu            |
| **UI**       | User Interface                                | Giao diện Người dùng            | Phần giao diện mà người dùng tương tác   |
| **UX**       | User Experience                               | Trải nghiệm Người dùng          | Cảm nhận của người dùng khi sử dụng      |
| **API**      | Application Programming Interface             | Giao diện Lập trình Ứng dụng    | Cách các phần mềm giao tiếp với nhau     |
| **REST**     | Representational State Transfer               | -                               | Kiến trúc API phổ biến                   |
| **RBAC**     | Role-Based Access Control                     | Kiểm soát Truy cập theo Vai trò | Phân quyền dựa trên vai trò              |
| **ERD**      | Entity-Relationship Diagram                   | Biểu đồ Thực thể - Mối quan hệ  | Sơ đồ thiết kế cơ sở dữ liệu             |
| **UC**       | Use Case                                      | Trường hợp Sử dụng              | Mô tả chức năng từ góc độ người dùng     |
| **VAT**      | Value Added Tax                               | Thuế Giá trị Gia tăng           | Thuế 8% áp dụng cho hóa đơn              |
| **SMS**      | Short Message Service                         | Dịch vụ Tin nhắn Ngắn           | Tin nhắn điện thoại                      |
| **KDS**      | Kitchen Display System                        | Hệ thống Hiển thị Bếp           | Màn hình hiển thị đơn hàng cho bếp       |
| **POS**      | Point of Sale                                 | Điểm Bán hàng                   | Hệ thống thu ngân                        |
| **QR Code**  | Quick Response Code                           | Mã Phản hồi Nhanh               | Mã vạch 2D có thể quét bằng điện thoại   |
| **KPI**      | Key Performance Indicator                     | Chỉ số Hiệu suất Chính          | Chỉ số đo lường hiệu quả công việc       |
| **SLA**      | Service Level Agreement                       | Thỏa thuận Mức độ Dịch vụ       | Cam kết về chất lượng dịch vụ            |
| **ACID**     | Atomicity, Consistency, Isolation, Durability | -                               | Tính chất của giao dịch database         |
| **HTTPS**    | HyperText Transfer Protocol Secure            | -                               | Giao thức truyền tải web an toàn         |
| **JWT**      | JSON Web Token                                | -                               | Chuẩn mã hóa thông tin xác thực          |
| **GDPR**     | General Data Protection Regulation            | -                               | Quy định bảo vệ dữ liệu cá nhân (EU)     |

### 1.5.2. Thuật ngữ Nghiệp vụ (Business Terms)

| Thuật ngữ                                  | Định nghĩa                                                    | Ví dụ                                                     |
| ------------------------------------------ | ------------------------------------------------------------- | --------------------------------------------------------- |
| **Bàn ăn (Table)**                         | Khu vực phục vụ khách hàng, được đánh số và phân theo khu vực | Bàn A3, Bàn VIP-01                                        |
| **Đơn hàng (Order)**                       | Tập hợp các món ăn mà khách hàng gọi cho một bàn ăn           | Đơn hàng #123 cho Bàn A3                                  |
| **Món ăn (Menu Item)**                     | Sản phẩm thức ăn/đồ uống trong thực đơn                       | Phở bò đặc biệt, Trà đào cam sả                           |
| **Nguyên liệu (Ingredient)**               | Thành phần cần thiết để chế biến món ăn                       | Thịt bò, hành tây, nước mắm                               |
| **Công thức (Recipe)**                     | Danh sách nguyên liệu và định lượng để chế biến 1 món         | Phở bò = 0.3kg thịt bò + 0.5kg bánh phở + ...             |
| **Tồn kho (Inventory)**                    | Số lượng nguyên liệu hiện có trong kho                        | Thịt bò: còn 15kg                                         |
| **Đặt bàn (Reservation)**                  | Khách hàng đặt trước bàn ăn cho thời gian cụ thể              | Đặt bàn 6 người lúc 19:00 ngày 25/11                      |
| **Khách hàng thân thiết (Loyal Customer)** | Khách đã có thông tin trong hệ thống và quay lại nhiều lần    | Anh Nguyễn Văn A - VIP                                    |
| **Khách VIP**                              | Khách có tổng chi tiêu >= 10 triệu đồng                       | Được ưu tiên và giảm giá đặc biệt                         |
| **Ca làm việc (Shift)**                    | Khoảng thời gian nhân viên làm việc                           | Ca sáng (7h-15h), Ca tối (15h-23h)                        |
| **Giờ cao điểm (Peak Hour)**               | Thời gian nhà hàng đông khách nhất                            | 11h30-13h30 (trưa), 18h-20h (tối)                         |
| **Hóa đơn (Bill/Invoice)**                 | Chứng từ thanh toán chi tiết các món đã gọi                   | Hóa đơn #456 ngày 25/11/2025                              |
| **Mã giảm giá (Discount Code/Voucher)**    | Mã ưu đãi giúp giảm giá đơn hàng                              | WELCOME10 (giảm 10%)                                      |
| **Workflow**                               | Quy trình xử lý có các bước tuần tự                           | Đơn hàng: Preparing → Ready → Serving → Completed         |
| **Dashboard**                              | Màn hình tổng quan hiển thị các chỉ số quan trọng             | Dashboard hiển thị doanh thu hôm nay, số bàn đang phục vụ |
| **Real-time**                              | Cập nhật tức thời, không có độ trễ                            | Tồn kho cập nhật real-time khi có đơn hàng                |
| **Audit Log**                              | Nhật ký ghi lại mọi thao tác quan trọng trong hệ thống        | "Admin sửa giá món Phở bò từ 50k → 55k lúc 10:30"         |
| **Rollback**                               | Hoàn tác giao dịch khi có lỗi                                 | Nếu trừ nguyên liệu lỗi thì rollback đơn hàng             |

### 1.5.3. Thuật ngữ Kỹ thuật (Technical Terms)

| Thuật ngữ                           | Định nghĩa                                           | Giải thích                                             |
| ----------------------------------- | ---------------------------------------------------- | ------------------------------------------------------ |
| **3-Tier Architecture**             | Kiến trúc 3 tầng: Presentation, Business Logic, Data | Tách biệt giao diện, xử lý nghiệp vụ và lưu trữ        |
| **WebSocket**                       | Giao thức truyền tin 2 chiều giữa client và server   | Dùng để gửi thông báo real-time từ server đến client   |
| **ORM (Object-Relational Mapping)** | Ánh xạ đối tượng - quan hệ                           | Sequelize giúp làm việc với database bằng code object  |
| **Transaction**                     | Giao dịch database đảm bảo tính toàn vẹn             | Tất cả thành công hoặc tất cả thất bại                 |
| **Foreign Key**                     | Khóa ngoại liên kết giữa 2 bảng                      | order.customer_id tham chiếu đến customers.customer_id |
| **Index**                           | Chỉ mục giúp tăng tốc truy vấn database              | Index trên phone_number để tìm khách hàng nhanh        |
| **Cache**                           | Bộ nhớ đệm lưu dữ liệu tạm thời                      | Redis cache menu để giảm truy vấn database             |
| **Load Balancer**                   | Bộ cân bằng tải phân phối request                    | Nginx phân phối request đến nhiều server               |
| **Backup**                          | Sao lưu dữ liệu để phòng mất mát                     | Backup database hàng ngày lúc 2h sáng                  |
| **Migration**                       | Script thay đổi cấu trúc database                    | Migration tạo bảng, thêm cột, sửa constraint           |
| **Seed Data**                       | Dữ liệu mẫu ban đầu                                  | Tạo 5 danh mục món, 10 món mẫu khi cài đặt             |
| **Middleware**                      | Tầng xử lý trung gian                                | JWT middleware kiểm tra token trước khi vào route      |
| **Deployment**                      | Triển khai ứng dụng lên server                       | Deploy code lên production server                      |
| **CI/CD**                           | Continuous Integration / Continuous Deployment       | Tự động test và deploy khi có code mới                 |
| **Responsive Design**               | Giao diện tự điều chỉnh theo kích thước màn hình     | Web hiển thị tốt trên desktop, tablet, mobile          |
| **Progressive Web App (PWA)**       | Web app có khả năng như native app                   | Có thể cài đặt về màn hình chính, hoạt động offline    |

---

## 1.6. Tài liệu Tham khảo

### 1.6.1. Tiêu chuẩn và Hướng dẫn

1. **IEEE Std 830-1998** - IEEE Recommended Practice for Software Requirements Specifications

   - Chuẩn quốc tế về viết tài liệu SRS
   - URL: https://standards.ieee.org/standard/830-1998.html

2. **ISO/IEC 25010:2011** - Systems and software quality models

   - Tiêu chuẩn về chất lượng phần mềm
   - Bao gồm: Functional suitability, Performance, Security, Usability

3. **GDPR (General Data Protection Regulation)**
   - Quy định bảo vệ dữ liệu cá nhân
   - Áp dụng cho xử lý thông tin khách hàng

### 1.6.2. Tài liệu Kỹ thuật

4. **Node.js Documentation (v16+)**

   - URL: https://nodejs.org/docs/latest-v16.x/api/
   - Tài liệu về Node.js backend

5. **React Documentation (v18+)**

   - URL: https://react.dev/
   - Tài liệu về React frontend

6. **PostgreSQL Documentation (v13+)**

   - URL: https://www.postgresql.org/docs/13/
   - Tài liệu về PostgreSQL database

7. **Express.js Guide (v4.x)**

   - URL: https://expressjs.com/
   - Framework backend cho Node.js

8. **Sequelize ORM Documentation (v6+)**

   - URL: https://sequelize.org/docs/v6/
   - ORM để làm việc với database

9. **Redis Documentation (v6+)**

   - URL: https://redis.io/docs/
   - Cache layer và session storage

10. **WebSocket Protocol (RFC 6455)**
    - URL: https://datatracker.ietf.org/doc/html/rfc6455
    - Giao thức truyền tin real-time

### 1.6.3. Tài liệu Thiết kế

11. **Material Design Guidelines**

    - URL: https://material.io/design
    - Hướng dẫn thiết kế giao diện Google

12. **Ant Design System**

    - URL: https://ant.design/
    - Thư viện UI component cho React

13. **UML 2.5 Specification**
    - URL: https://www.omg.org/spec/UML/
    - Chuẩn biểu đồ UML (Use Case, Sequence, Class, ERD)

### 1.6.4. Tài liệu Nghiệp vụ

14. **Quy định về Hóa đơn Điện tử (Việt Nam)**

    - Nghị định 123/2020/NĐ-CP về hóa đơn, chứng từ
    - URL: https://thuvienphapluat.vn/van-ban/Thue-Phi-Le-Phi/Nghi-dinh-123-2020-ND-CP-hoa-don-chung-tu-457563.aspx

15. **Thông tư 219/2013/TT-BTC** - Hướng dẫn thuế GTGT ngành F&B

    - Thuế VAT 8% cho dịch vụ ăn uống
    - URL: https://thuvienphapluat.vn/van-ban/Thue-Phi-Le-Phi/Thong-tu-219-2013-TT-BTC-huong-dan-thue-gia-tri-gia-tang-215380.aspx

16. **Best Practices for Restaurant Management Software**
    - Các nghiên cứu về phần mềm quản lý nhà hàng
    - Tham khảo từ: Toast POS, Square, Clover

### 1.6.5. Tài liệu Dự án (Internal)

17. **Yêu cầu Nghiệp vụ Chi tiết** (Business Requirements Document)

    - File: `Yêu cầu nghiệp vụ.docx`
    - Người cung cấp: Quản lý Nhà hàng PTIT

18. **Danh sách Task và Tiêu chí Chấm điểm**

    - File: `Yêu cầu SRS.xlsx - Danh sách task.pdf`
    - Người cung cấp: Giảng viên hướng dẫn

19. **Phỏng vấn Stakeholders** (Meeting minutes)

    - Ngày 01/11/2025 - Phỏng vấn Quản lý
    - Ngày 05/11/2025 - Phỏng vấn Thu ngân và Phục vụ
    - Ngày 08/11/2025 - Phỏng vấn Bếp trưởng

20. **Khảo sát Quy trình Hiện tại** (Process Analysis)
    - Shadowing nhân viên phục vụ (3 ngày)
    - Quan sát quy trình trong giờ cao điểm
    - Thu thập dữ liệu: 150 đơn hàng mẫu

---

## 1.7. Tổng quan Tài liệu

### 1.7.1. Cấu trúc Tài liệu

Tài liệu SRS này được tổ chức thành **8 chương chính** và **Phụ lục**:

#### **PHẦN I: PHÂN TÍCH TỔNG QUAN VÀ KIẾN TRÚC**

**Chương 1: GIỚI THIỆU** _(Chương hiện tại)_

- Mục đích tài liệu và đối tượng sử dụng
- Bối cảnh dự án và tình hình hiện tại
- Vấn đề cần giải quyết và mục tiêu
- Phạm vi dự án (In-scope/Out-of-scope)
- Định nghĩa thuật ngữ và tài liệu tham khảo

**Chương 2: KIẾN TRÚC VÀ TỔNG QUAN HỆ THỐNG**

- Kiến trúc 3 tầng (Presentation - Business Logic - Data)
- Biểu đồ Use Case tổng thể
- Danh sách và mô tả các Use Case
- Mô tả chi tiết các Actors (5 vai trò)
- Ràng buộc nghiệp vụ (32 ràng buộc)
- Workflow chính của hệ thống
- Luồng dữ liệu giữa các thành phần

#### **PHẦN II: THIẾT KẾ CẤU TRÚC DỮ LIỆU & HƯỚNG ĐỐI TƯỢNG**

**Chương 3: YÊU CẦU CHỨC NĂNG CHI TIẾT**

- RF-01 đến RF-16: Đặc tả chi tiết 16 yêu cầu chức năng chính
- Mỗi RF bao gồm: Mô tả, Input, Output, Quy tắc nghiệp vụ, Xử lý lỗi
- Phân hệ Quản lý Bàn & Khách hàng (RF-01, RF-02, RF-03)
- Phân hệ Quản lý Thực đơn & Nguyên liệu (RF-04, RF-05, RF-06)
- Phân hệ Quản lý Đơn hàng & Thanh toán (RF-07, RF-08, RF-09)
- Phân hệ Báo cáo & Thống kê (RF-10, RF-11, RF-12)
- Tính năng Tự động (RF-13, RF-14, RF-15, RF-16)

**Chương 4: THIẾT KẾ CƠ SỞ DỮ LIỆU**

- Biểu đồ ERD (Entity-Relationship Diagram) đầy đủ
- SQL DDL Script (20+ tables)
- Mô tả chi tiết 20+ bảng dữ liệu:
  - Tables chính: employees, customers, tables, orders, menu_items, ingredients
  - Tables quan hệ: item_ingredients, order_details, reservations
  - Tables hỗ trợ: inventory_transactions, notifications, audit_logs
- Indexes, Constraints, Foreign Keys
- Views, Functions, Triggers, Stored Procedures
- Dữ liệu mẫu (Seed data)

**Chương 5 (Tùy chọn): BIỂU ĐỒ LỚP**

- Class Diagram chi tiết
- Các lớp chính và thuộc tính
- Mối quan hệ giữa các lớp (Aggregation, Composition, Inheritance)
- Multiplicity và Visibility

#### **PHẦN III: THIẾT KẾ HÀNH VI CHI TIẾT**

**Chương 5 (hoặc 6): ĐẶC TẢ USE CASE CHI TIẾT**

- Đặc tả chi tiết 4-5 Use Case phức tạp nhất:
  - **UC04**: Tạo Đơn hàng (Create Order)
  - **UC05**: Cập nhật Trạng thái Đơn hàng
  - **UC09**: Xử lý Thanh toán
  - **UC10**: Báo cáo Doanh thu
  - **UC13**: Tự động Trừ Nguyên liệu
- Mỗi UC bao gồm:
  - Thông tin tổng quan
  - Tiền điều kiện & Hậu điều kiện
  - Luồng sự kiện chính (Main Success Scenario)
  - Luồng sự kiện thay thế (Alternative Flows) - 5-8 luồng
  - Quy tắc nghiệp vụ chi tiết
  - Yêu cầu đặc biệt (Performance, Security, Usability)

**Chương 6 (hoặc 7): BIỂU ĐỒ TUẦN TỰ**

- 4-5 Sequence Diagrams cho các Use Case phức tạp
- Thể hiện đầy đủ:
  - Actors, Objects, Lifelines
  - Messages (Synchronous/Asynchronous)
  - Activation bars
  - Combined Fragments (alt, opt, loop, par)
  - Return values
- Mô tả luồng tương tác chi tiết giữa:
  - User → UI → Controller → Service → Repository → Database
  - Tích hợp với External Services (Payment Gateway, SMS/Email)

**Chương 7 (Tùy chọn): BIỂU ĐỒ TRẠNG THÁI**

- State Diagrams cho các đối tượng có nhiều trạng thái:
  - **Order State Machine**: Preparing → Ready → Serving → Completed
  - **Table State Machine**: Available → Reserved → Occupied
  - **Reservation State Machine**: Pending → Confirmed → Completed/Cancelled
- Các thành phần: States, Transitions, Events, Guards, Actions

#### **PHẦN IV: THIẾT KẾ GIAO DIỆN & YÊU CẦU PHI CHỨC NĂNG**

**Chương 7 (hoặc 8): THIẾT KẾ GIAO DIỆN NGƯỜI DÙNG**

- Wireframes cho 10-15 màn hình chính:
  - Dashboard tổng quan
  - Sơ đồ bàn và tạo đơn hàng
  - Quản lý thực đơn
  - Quản lý tồn kho
  - Thanh toán
  - Báo cáo doanh thu
  - Quản lý khách hàng
  - Cấu hình hệ thống
- Mô tả chi tiết từng màn hình:
  - Mục đích
  - Thành phần UI
  - Tương tác người dùng
  - Validation và Error handling
- UI/UX Guidelines:
  - Color scheme
  - Typography
  - Icons
  - Responsive breakpoints

**Chương 8 (hoặc 9): THIẾT KẾ FIGMA (Tùy chọn - 15 điểm thưởng)**

- Mockup giao diện chi tiết trên Figma
- High-fidelity design với màu sắc, font chữ, icon thực tế
- Interactive prototype
- Component library
- Design system documentation

**Chương 8 (hoặc 9 hoặc 10): YÊU CẦU PHI CHỨC NĂNG**

- **Hiệu năng (Performance)**:
  - Thời gian phản hồi: < 2 giây
  - Số người dùng đồng thời: 20-30
  - Throughput: 100 đơn hàng/giờ cao điểm
- **Bảo mật (Security)**:
  - Authentication: JWT
  - Authorization: RBAC
  - Mã hóa mật khẩu: bcrypt
  - HTTPS bắt buộc
  - SQL Injection prevention
  - XSS, CSRF protection
- **Độ tin cậy (Reliability)**:
  - Uptime: 99.5%
  - MTBF (Mean Time Between Failures): > 720 giờ
  - MTTR (Mean Time To Repair): < 2 giờ
  - Backup tự động hàng ngày
- **Khả năng sử dụng (Usability)**:
  - Dễ học: Nhân viên mới làm quen trong 2 giờ
  - Dễ sử dụng: 5 clicks để hoàn thành đơn hàng
  - Error prevention và Clear error messages
  - Help & Documentation
- **Khả năng mở rộng (Scalability)**:
  - Hỗ trợ 2-3 chi nhánh trong tương lai
  - Tăng từ 30 bàn → 50 bàn
  - Tăng từ 80 món → 150 món
- **Khả năng bảo trì (Maintainability)**:
  - Code structure rõ ràng
  - Documentation đầy đủ
  - Unit test coverage: > 70%
  - CI/CD pipeline
- **Khả năng tương thích (Compatibility)**:
  - Browsers: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
  - Devices: Desktop (1920x1080), Tablet (1024x768), Mobile (375x667)
  - OS: Windows 10+, macOS 11+, Ubuntu 20.04+
- **Tuân thủ pháp lý (Legal & Compliance)**:
  - GDPR: Quyền xóa dữ liệu cá nhân
  - Luật An toàn thông tin Việt Nam
  - Quy định về hóa đơn điện tử

#### **PHẦN V: PHỤ LỤC**

**Phụ lục A: Glossary (Bảng thuật ngữ mở rộng)**

- Bổ sung thêm 50+ thuật ngữ kỹ thuật và nghiệp vụ

**Phụ lục B: Phân tích Rủi ro**

- Rủi ro kỹ thuật (Mất dữ liệu, Lỗi bảo mật, Performance bottleneck)
- Rủi ro nghiệp vụ (Nhân viên kháng cự thay đổi, Ngân sách vượt)
- Kế hoạch giảm thiểu rủi ro

**Phụ lục C: Kế hoạch Triển khai**

- Timeline dự án (6 tháng)
- Phân chia Phase 1, 2, 3
- Milestone và Deliverables
- Resource allocation

**Phụ lục D: Test Cases Mẫu**

- 10-15 test cases cho các chức năng chính
- Format: ID, Description, Preconditions, Steps, Expected Result

**Phụ lục E: Mockup Data**

- Dữ liệu mẫu để demo và test:
  - 30 bàn ăn
  - 80 món ăn
  - 50 nguyên liệu
  - 100 đơn hàng lịch sử

**Phụ lục F: Tài liệu Tham khảo Bổ sung**

- Link đến GitHub repository (nếu có)
- Link đến Figma design
- Link đến demo video

---

### 1.7.2. Quy ước Ký hiệu trong Tài liệu

#### Biểu tượng Mức độ Ưu tiên

| Ký hiệu    | Ý nghĩa             | Giải thích                                   |
| ---------- | ------------------- | -------------------------------------------- |
| ⭐⭐⭐⭐⭐ | Rất cao (Critical)  | Phải có trong phiên bản 1.0, không thể thiếu |
| ⭐⭐⭐⭐   | Cao (High)          | Rất quan trọng, nên có trong v1.0            |
| ⭐⭐⭐     | Trung bình (Medium) | Quan trọng nhưng có thể delay sang v1.1      |
| ⭐⭐       | Thấp (Low)          | Nice to have, có thể để Phase 2              |
| ⭐         | Rất thấp (Very Low) | Tính năng bổ sung, không ảnh hưởng nghiệp vụ |

#### Biểu tượng Trạng thái

| Ký hiệu | Ý nghĩa                       |
| ------- | ----------------------------- |
| ✅      | Trong phạm vi (In-scope)      |
| ❌      | Ngoài phạm vi (Out-of-scope)  |
| 🚧      | Đang phát triển (In progress) |
| ⏸️      | Tạm hoãn (On hold)            |
| ✔️      | Hoàn thành (Completed)        |
| ⚠️      | Cảnh báo (Warning)            |
| 🔴      | Lỗi/Khẩn cấp (Error/Urgent)   |
| 🟡      | Chú ý (Attention)             |
| 🟢      | Bình thường (Normal)          |

#### Quy ước Đánh số

**Use Cases:**

- Format: `UC{XX}` (VD: UC01, UC15)
- XX là số thứ tự từ 01-99

**Yêu cầu Chức năng:**

- Format: `RF-{XX}` (VD: RF-01, RF-16)
- XX là số thứ tự từ 01-99

**Yêu cầu Phi chức năng:**

- Format: `NFR-{Category}-{XX}`
- VD: NFR-PERF-01 (Performance requirement #1)
- VD: NFR-SEC-05 (Security requirement #5)

**Business Rules:**

- Format: `BR-{XX}.{Y}`
- VD: BR-04.1 (Rule 1 thuộc nhóm 4)

**Test Cases:**

- Format: `TC-{UC}{XX}`
- VD: TC-UC04-01 (Test case 01 cho Use Case 04)

#### Quy ước Màu sắc

- **Xanh lá**: Tính năng đã hoàn thành / Trạng thái thành công
- **Đỏ**: Lỗi / Cảnh báo nghiêm trọng / Out-of-scope
- **Vàng**: Chờ xử lý / Cảnh báo / Reserved
- **Xám**: Disabled / Inactive / Deprecated
- **Xanh dương**: Thông tin / In progress / Link

---

### 1.7.3. Hướng dẫn Đọc Tài liệu

#### Đối với Người đọc lần đầu

**Bước 1: Đọc phần Tổng quan** (30 phút)

- Chương 1: Hiểu vấn đề và mục tiêu
- Chương 2: Hiểu kiến trúc tổng thể

**Bước 2: Chọn phần quan tâm** (tùy vai trò)

- **Business stakeholder**: Chương 3 (Yêu cầu chức năng)
- **Developer**: Chương 4, 5, 6 (Database, Use Case, Sequence)
- **Designer**: Chương 7 (Wireframe/Figma)
- **Tester**: Chương 5, Phụ lục D (Use Case chi tiết, Test cases)

**Bước 3: Đọc sâu phần cụ thể** (tùy nhu cầu)

#### Đối với Project Manager

Đọc theo thứ tự:

1. Chương 1 → Hiểu scope và timeline
2. Chương 2 → Hiểu kiến trúc và ước lượng effort
3. Phụ lục C → Kế hoạch triển khai chi tiết
4. Phụ lục B → Đánh giá rủi ro

#### Đối với Developer

Đọc theo thứ tự:

1. Chương 2 → Hiểu kiến trúc 3 tầng
2. Chương 4 → Hiểu database schema
3. Chương 5, 6 → Hiểu luồng nghiệp vụ chi tiết
4. Chương 3 → Implement từng yêu cầu chức năng

#### Đối với QA/Tester

Đọc theo thứ tự:

1. Chương 1, 2 → Hiểu tổng quan
2. Chương 5 → Đặc tả Use Case chi tiết
3. Phụ lục D → Test cases mẫu
4. Viết thêm test cases mới

---

### 1.7.4. Lịch sử Thay đổi (Change Log)

| Phiên bản | Ngày       | Người thực hiện | Nội dung thay đổi                           |
| --------- | ---------- | --------------- | ------------------------------------------- |
| **0.1**   | 01/11/2025 | [Tên SV]        | Khởi tạo tài liệu, viết Chương 1 draft      |
| **0.2**   | 05/11/2025 | [Tên SV]        | Bổ sung Chương 2 - Kiến trúc                |
| **0.3**   | 10/11/2025 | [Tên SV]        | Hoàn thiện Chương 3, 4 - Yêu cầu & Database |
| **0.4**   | 15/11/2025 | [Tên SV]        | Bổ sung Chương 5, 6 - Use Case & Sequence   |
| **0.5**   | 18/11/2025 | [Tên SV]        | Review và sửa lỗi Chương 1-6                |
| **0.6**   | 20/11/2025 | [Tên SV]        | Bổ sung Chương 7 - Wireframe                |
| **0.7**   | 22/11/2025 | [Tên SV]        | Hoàn thiện Chương 8 - NFR và Phụ lục        |
| **0.8**   | 24/11/2025 | [Giảng viên]    | Review và góp ý sửa chữa                    |
| **0.9**   | 26/11/2025 | [Tên SV]        | Sửa theo góp ý, chuẩn bị nộp                |
| **1.0**   | 28/11/2025 | [Tên SV]        | **Phiên bản chính thức nộp**                |

---

### 1.7.5. Phê duyệt Tài liệu

| Vai trò             | Họ tên             | Chữ ký           | Ngày               |
| ------------------- | ------------------ | ---------------- | ------------------ |
| **Người thực hiện** | [Tên sinh viên]    | \***\*\_\_\*\*** | \_**\_/\_\_**/2025 |
| **Người hướng dẫn** | [Tên giảng viên]   | \***\*\_\_\*\*** | \_**\_/\_\_**/2025 |
| **Product Owner**   | [Quản lý Nhà hàng] | \***\*\_\_\*\*** | \_**\_/\_\_**/2025 |
| **Người phê duyệt** | [Trưởng bộ môn]    | \***\*\_\_\*\*** | \_**\_/\_\_**/2025 |

---

## 1.8. Tóm tắt Chương 1

Chương 1 đã trình bày tổng quan về dự án Hệ thống Quản lý Nhà hàng PTIT (PTIT-RMS):

✅ **Mục đích**: Xây dựng hệ thống số hóa toàn bộ quy trình vận hành nhà hàng

✅ **Bối cảnh**:

- Nhà hàng vừa và nhỏ (80-100 khách, 25-30 bàn, 60-80 món)
- Hiện tại vận hành thủ công với nhiều vấn đề (35% sai sót đơn hàng, 40% thiếu đồng bộ, 50% khó kiểm soát kho)

✅ **Vấn đề cần giải quyết**:

- Sai sót đơn hàng → Số hóa quy trình đặt món
- Thiếu đồng bộ → Thông báo real-time
- Khó kiểm soát kho → Quản lý tự động
- Thanh toán chậm → Tính tiền tức thời
- Báo cáo chậm → Dashboard real-time

✅ **Phạm vi dự án**:

- **In-scope (v1.0)**: 6 module chính với 16 yêu cầu chức năng
- **Out-of-scope**: 13 tính năng để lại cho Phase 2/3

✅ **Định nghĩa**: Giải thích 20+ từ viết tắt và 40+ thuật ngữ nghiệp vụ/kỹ thuật

✅ **Tài liệu tham khảo**: 20 nguồn tham khảo (chuẩn, kỹ thuật, nghiệp vụ, nội bộ)

✅ **Cấu trúc tài liệu**: 8-10 chương + phụ lục, tổng ~150-200 trang

**Chương tiếp theo (Chương 2)** sẽ trình bày chi tiết về:

- Kiến trúc 3 tầng của hệ thống
- Biểu đồ Use Case tổng thể với 15-20 use cases
- Mô tả 5 Actors và 32 ràng buộc nghiệp vụ
- Workflow và luồng dữ liệu chính

---

**HẾT CHƯƠNG 1**

_Ngày hoàn thành: [Ngày/Tháng/Năm]_  
_Người thực hiện: [Họ tên sinh viên - MSSV]_  
_Lớp: [Mã lớp] - [Tên môn học]_

---

> **LƯU Ý**:
>
> - Tài liệu này là phiên bản 1.0 chính thức
> - Mọi thay đổi phải được phê duyệt bằng văn bản
> - Vui lòng tham khảo Change Log (mục 1.7.4) để biết lịch sử cập nhật
> - Liên hệ: [Email] - [Số điện thoại]# ĐẶC TẢ YÊU CẦU PHẦN MỀM (SRS)

## HỆ THỐNG QUẢN LÝ NHÀ HÀNG PTIT

### PTIT Restaurant Management System (PTIT-RMS)

**Phiên bản:** 1.0  
**Ngày:** Tháng 11/2025  
**Người thực hiện:** [Tên sinh viên]  
**Lớp:** [Mã lớp]

---
