# -HCM-KS26-CNTT1-_-Thi-k-t-th-c-m-n-PROTING-V-I-AI-_
# BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 06
- **Họ tên:LÊ TUẤN KIỆT**
- **Lớp:CNTT1**
- **Link repository Pha1: **

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Tên Khách hàng [TEN_KHACH_HANG], số điện thoại [SO_DIEN_THOAI],số CCCD [SO_CCCD], email [EMAIL], đặt bàn buffet hải sản (mã HAISAN) tối ngày 27/12/2026 cho 4 người lớn và 2 trẻ em. Khách đã chuyển khoản đặt cọc 1.000.000 đồng. Khách yêu cầu bàn gần cửa sổ.
```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Câu 1: Chỉ trả lời dựa trên file đính kèm và không được thêm các thông tin khác vào, sử dụng từ ngữ chính xác
Câu 2: Trả 
```

## 4.2. Trích xuất tin nhắn đặt bàn ra JSON

**PROMPT**

```text

C - context 
Tôi là nhân viên mới của nhà hàng Bếp Việt Buffet, phục vụ ba loại buffet: lẩu, nướng và hải sản. Quản lý nhà hàng giao cho tôi xây dựng prompt để xử lý thông tin khách, tin nhắn đặt bàn, báo giá và khiếu nại một cách an toàn, chính xác.

R - request
Hãy trích xuất từ 3 tin nhắn đặt bàn 

E - Explanation
5 QUY TẮC CHUẨN HÓA
1. ma_buffet lấy theo bảng mã loại buffet.
2. ngay_dat dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. khu_vuc chỉ nhận "trong_nha", "san_vuon" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về ăn uống, dị ứng, đi lại; không có thì null.

A - Action
Tin nhắn mẫu 1:
Đặt giúp em buffet hải sản tối 14/11/2026, 2 người lớn 2 bé, ngồi sân vườn. 1 bé dị ứng tôm.
Kết quả đúng:
{"ma_buffet": "HAISAN", "ngay_dat": "2026-11-14", "nguoi_lon": 2, "tre_em": 2, "khu_vuc": "san_vuon", "ghi_chu": "1 bé dị ứng tôm"}

Tin nhắn mẫu 2:
Buffet lẩu còn bàn trong nhà không shop, mình đi 1 người, hôm nào cũng được.
Kết quả đúng:
{"ma_buffet": "LAU", "ngay_dat": null, "nguoi_lon": 1, "tre_em": 0, "khu_vuc": "trong_nha", "ghi_chu": null}

T - Tone
Chính xác, Khách quan, chuẩn định dạng kỹ thuật.

E - Extras
Chỉ trả về định dạng JASON hợp lệ, không chào hỏi, không giải thích thêm.
TN1: Cho mình đặt buffet nướng ngày 19/12 nha, 2 người lớn với 1 bé 9 tuổi, ngồi trong nhà.

TN2: Đặt bàn buffet hải sản cho 3 người ngày 03/01/2027 ạ. Một người dị ứng sữa.

TN3: Buffet lẩu tối Giáng sinh còn bàn không ạ? Nhà em 4 người lớn, ông ngoại đi xe lăn, ngồi sân vườn.

```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
quy tắc tính giá.
<QUY_TAC_TINH_GIA> 
Bước 1. Trẻ em cao từ 1 m đến 1,4 m tính 50% giá người lớn. Trẻ dưới 1 m miễn phí.
Bước 2. Đoàn từ 10 người lớn trở lên được giảm 5% trên tiền buffet (tiền người lớn + tiền trẻ em).
Bước 3. Phụ thu phòng riêng: 500.000 đồng/phòng. Phụ thu không được giảm giá.
Bước 4. Tổng tiền = tiền buffet sau giảm + phụ thu.
yêu cầu báo giá.
<YEU_CAU_BAO_GIA>
Đoàn công ty ăn buffet nướng (NUONG) gồm 10 người lớn, 3 trẻ em (cao 1,1 m, 1,2 m và 1,35 m) và 1 bé cao 0,8 m. Đoàn thuê 1 phòng riêng.

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
EMAIL KHIẾU NẠI CỦA KHÁCH

Tối 08/11/2026 gia đình tôi 5 người ăn buffet hải sản. Nhà hàng quảng cáo có tôm hùm và cua nhưng cả buổi tối hai món này đều hết, chúng tôi chờ 2 lần bổ sung đều không có. Nhân viên phục vụ rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị nhà hàng giải quyết thỏa đáng.
<Thieu_mon>

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. Trạm 1 – đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "thieu_mon" | "cho_lau" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_mon_bi_thieu": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}

2.Dưới đây là kết quả phân loại khiếu nại (JSON của Trạm 1) và chính sách đền bù. Chỉ dùng chính sách trong thẻ <chinh_sach> để tính tổng mức đền bù. Trả về đúng một đối tượng JSON, không giải thích:
{"loai_su_co": ..., "muc_den_bu": tổng số tiền (đồng) hoặc null, "can_duyet": true nếu muc_den_bu trên 800.000, ngược lại false}
<ket_qua_tram_1>(JSON của Trạm 1)</ket_qua_tram_1>
<chinh_sach>(chính sách đền bù ở trên)</chinh_sach>


### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Bạn là nhân viên chăm sóc khách hàng của Bếp Việt Buffet. Dựa trên JSON trong thẻ <ket_qua_tram_2>, viết email phản hồi khách: xin lỗi, nêu đúng mức đền bù trong JSON, không tự đặt mức khác, giọng điệu chân thành, chuyên nghiệp, dưới 150 từ.
<ket_qua_tram_2>(JSON của Trạm 2)</ket_qua_tram_2>

```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text

```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Bếp Việt", nhân viên tư vấn trên website của nhà hàng Bếp Việt Buffet, giúp khách chọn và đặt bàn một trong 3 loại buffet: LAU (Buffet lẩu), NUONG (Buffet nướng), HAISAN (Buffet hải sản).

[Quy tắc ràng buộc]
1. Chỉ tư vấn đúng bảng giá buffet (LAU - buffet lẩu, NUONG - Buffet nướng, HAISAN - Buffet hải sản) không tư vấn gì thêm.
2.Tuân thủ đúng bảng giá niêm yết, tuyệt đối không hứa hẹn thêm giá nào ngoài bảng giá.
3. Không bao giờ hỏi số cccd hoặc số thẻ ngân hàng của khách hàng. 

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá buffet trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
