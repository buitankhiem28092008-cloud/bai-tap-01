# Lesson 03 - PowerShell Pipeline: Đếm sự cố TIMEOUT

## 1. Mục tiêu
- Làm chủ kỹ thuật đường ống Pipeline (`|`) trong PowerShell để nối các lệnh đơn lẻ thành chuỗi xử lý tự động.
- Hiểu sự khác biệt giữa Pipeline của PowerShell (truyền object) và Pipeline của CMD/Bash (truyền text).

## 2. Bối cảnh & Vấn đề
Phòng DevOps cần thống kê nhanh: "Có bao nhiêu sự cố TIMEOUT xảy ra trong tệp log hôm nay?".
Kết quả trả về phải là MỘT con số tổng duy nhất, không in ra hàng nghìn dòng văn bản.

## 3. Yêu cầu nghiệp vụ
- Sử dụng mô hình Pipeline: Đọc tệp -> Lọc từ khóa -> Đếm kết quả.
- Thực hiện bằng duy nhất 1 dòng lệnh trên PowerShell.

## 4. Ràng buộc & Bẫy dữ liệu
- Bẫy: Tệp log có chứa các dòng `NOT TIMEOUT`. Lệnh lọc không được đếm nhầm các dòng này.
  Gợi ý: lọc 2 lần liên tiếp, hoặc dùng `Where-Object` sau dấu `|`.
- Kiến thức mở rộng: PowerShell truyền object qua `|`, không phải text thuần.
  Có thể dùng thuộc tính `.Count` thay vì `wc -l` như trên Bash.

## 5. Yêu cầu đầu ra
- Phần 1 - Phân tích: vẽ sơ đồ luồng dữ liệu qua Pipeline -> làm trong `bai_lam\phan1_phan_tich.md`
- Phần 2 - Triển khai: viết lệnh 1 dòng hoàn chỉnh -> làm trong `bai_lam\phan2_trien_khai.ps1`

## Cấu trúc thư mục
```
Lesson_Pipeline_Timeout\
├── README.md                     (đề bài)
├── config\
│   └── app.config.json           (cấu hình dịch vụ)
├── logs\
│   ├── app_2026-10-13.log        (log ứng dụng - HÔM NAY)
│   ├── worker_2026-10-13.log     (log worker - hôm nay)
│   ├── access_2026-10-13.log     (log truy cập web - hôm nay)
│   ├── monitor_2026-10-13.csv    (số liệu giám sát dạng bảng)
│   ├── api_events_2026-10-13.json (sự kiện API dạng JSON)
│   ├── healthcheck_2026-10-13.xml (báo cáo health check dạng XML)
│   ├── incident_notes_2026-10-13.txt (ghi chú ca trực, viết tay)
│   ├── app_2026-10-13.log.bak    (bản sao lưu cũ)
│   ├── app_2026-10-12.log        (log ứng dụng - hôm qua)
│   └── archive\
│       ├── app_2026-10-10.log
│       └── app_2026-10-11.log
└── bai_lam\
    ├── phan1_phan_tich.md
    └── phan2_trien_khai.ps1
```

## Thử thách thêm (tùy chọn)
1. Đếm TIMEOUT trong log ứng dụng của hôm nay.
2. Đếm TIMEOUT trong TẤT CẢ log của hôm nay (app + worker).
3. Đếm TIMEOUT trong toàn bộ thư mục `logs`, kể cả `archive`.
4. So sánh kết quả khi KHÔNG xử lý bẫy `NOT TIMEOUT` và khi có xử lý.
5. Đếm TIMEOUT trong tệp `.csv` theo 2 cách: lọc text bằng `Select-String`,
   và đọc bằng `Import-Csv` rồi lọc theo cột `Status`. Cách nào chính xác hơn? Vì sao?
6. Đếm TIMEOUT trong tệp `.json` bằng `ConvertFrom-Json` và lọc theo trường `event`.
7. Đếm TIMEOUT trong tệp `.xml` bằng `[xml]` và lọc theo thuộc tính `result`.
8. Khi quét cả thư mục `logs`, làm sao để KHÔNG đếm nhầm tệp `.bak`?
9. Tệp `.txt` là ghi chú viết tay. Có nên đưa nó vào thống kê không? Giải thích.
