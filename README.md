Bot Giao Dịch Chiến Lược Hedge (Đối Ứng)
Một bot giao dịch dựa trên Binance Futures API, sử dụng chiến lược đối ứng mở đồng thời cả vị thế Mua (Long) và Bán (Short).

Ý tưởng chiến lược
Chiến lược này áp dụng tư duy giao dịch mở song song Long/Short đối ứng, với logic cốt lõi như sau:

Mở vị thế thị trường trung tính (Market Neutral): Mở đồng thời lệnh Long và Short cho cùng một cặp giao dịch để hình thành vị thế đối ứng. Như vậy, dù thị trường tăng hay giảm, lãi lỗ của hai bên sẽ triệt tiêu lẫn nhau, về lý thuyết đạt được trạng thái trung tính với thị trường.

Cắt lỗ bên thua cuộc: Khi thị trường xuất hiện xu hướng đơn biến mạnh, một bên sẽ có lãi và bên còn lại sẽ lỗ. Khi mức lỗ của bên thua vượt quá ngưỡng thiết lập (mặc định 1%), hệ thống sẽ ngay lập tức cắt lỗ để kiểm soát rủi ro.

Chốt lời động (Trailing Stop) cho bên thắng: Sau khi bên lỗ đã cắt vị thế, bên có lợi nhuận sẽ chuyển sang chế độ chốt lời động. Hệ thống thiết lập 11 mức chốt lời động để điều chỉnh tỷ lệ dựa trên biên độ lợi nhuận:

Khi lợi nhuận nhỏ (0.2% - 1.5%): Sử dụng mức hồi quy cố định hoặc tỷ lệ hồi quy nhỏ.

Khi lợi nhuận lớn (2.0% - 10.0%): Sử dụng tỷ lệ hồi quy lớn hơn để tối ưu hóa không gian tăng trưởng và bắt trọn con sóng lớn.

Kiểm soát rủi ro: Thiết lập ngưỡng lỗ cho tổng lợi nhuận tích lũy. Nếu tổng lỗ vượt quá giá trị cài đặt, bot sẽ tự động dừng giao dịch để tránh thua lỗ kéo dài.

Ưu điểm của chiến lược:

Trung tính với thị trường, không phụ thuộc vào việc dự đoán hướng đi của giá.

Tối đa hóa lợi nhuận của bên thắng thông qua cơ chế chốt lời động.

Cơ chế cắt lỗ nghiêm ngặt để kiểm soát rủi ro trên từng lệnh.

Rủi ro tổng thể có thể kiểm soát với các điều kiện dừng rõ ràng.

Sử dụng trong trường hợp:

Thị trường đi ngang (sideway) hoặc thị trường có biến động mạnh (high volatility).

Phù hợp để bắt các biến động giá ngắn hạn.

Môi trường giao dịch yêu cầu kiểm soát rủi ro chặt chẽ.

Tính năng đặc sắc
✅ Mở kép Long/Short: Mở đồng thời cả hai vị thế để duy trì chiến lược trung tính.
✅ Cắt lỗ thông minh: Tự động đóng vị thế khi một bên lỗ vượt quá 1%.
✅ Chốt lời động (Trailing Stop): 11 mức chốt lời động giúp tối đa hóa lợi nhuận cho bên thắng.
✅ Giám sát đơn biên: Hỗ trợ theo dõi chốt lời động ngay cả khi chỉ còn giữ vị thế một phía.

✅ Thông báo DingTalk: Hỗ trợ đẩy thông báo giao dịch và trạng thái vị thế thời gian thực.

✅ Ghi nhật ký (Logs): Hệ thống log hoàn chỉnh, hỗ trợ xoay vòng (rotation) dữ liệu.

Cấu trúc dự án
hedge_strategy/
├── main_hedge.py                    # Điểm vào chương trình chính
├── config.json                      # Tệp cấu hình
├── requirements.txt                 # Danh sách thư viện phụ thuộc
├── strategies/                      # Các mô-đun chiến lược
│   ├── base_strategy.py            # Lớp cơ sở (Base class)
│   └── hedge_strategy.py           # Thực thi chiến lược đối ứng
├── position_manager/                # Mô-đun quản lý vị thế
│   ├── hedge_stop_loss_manager.py   # Quản lý chốt lời/cắt lỗ đối ứng
│   ├── stop_loss_manager.py        # Quản lý chốt lời/cắt lỗ chung
│   └── position_monitor.py        # Điểm vào giám sát độc lập
└── utils/                           # Các mô-đun công cụ
    ├── config_loader.py            # Tải cấu hình
    ├── exchange_utils.py           # Công cụ hỗ trợ sàn giao dịch
    ├── logger_setup.py             # Cấu hình nhật ký
    ├── math_utils.py               # Công cụ tính toán toán học
    └── notification.py             # Mô-đun thông báo
Cài đặt
Clone hoặc tải dự án về máy địa phương.

Cài đặt các thư viện phụ thuộc:

Bash

pip install -r requirements.txt
Cấu hình config.json:

Bash

# Sao chép tệp cấu hình mẫu
cp config.example.json config.json

# Chỉnh sửa config.json, điền API Key và các thông tin cần thiết
Cách sử dụng
Chạy chương trình chính
Bash

python main_hedge.py
Chạy độc lập trình giám sát Chốt lời/Cắt lỗ
Bash

python position_manager/position_monitor.py
Giải thích cấu hình chính
binance: Cấu hình Binance API.

leverage: Đòn bẩy toàn cục (Mặc định 6x).

monitor_interval: Khoảng thời gian kiểm tra lệnh (giây, mặc định 60s).

max_total_profit_loss_usdt: Ngưỡng lỗ tối đa cho phép của tổng lợi nhuận tích lũy (số âm). Nếu tổng PnL <= giá trị này, bot dừng lại.

stop_loss: Cấu hình cắt lỗ (mặc định 1%) và 11 mức chốt lời động.

tradingPairs: Cấu hình cặp giao dịch, số tiền vào lệnh Long/Short (USDT).

Lưu ý quan trọng ⚠️
Bảo mật API Key: Tuyệt đối không tiết lộ hoặc đẩy API Key lên kho lưu trữ mã nguồn.

Môi trường thử nghiệm: Khuyến khích chạy trên Testnet trước khi đánh thật.

Chế độ vị thế: Đảm bảo tài khoản Binance đã bật Chế độ vị thế hai chiều (Hedge Mode).

Quản lý vốn: Thiết lập thông số max_total_profit_loss_usdt hợp lý để bảo vệ tài khoản.

Rủi ro đòn bẩy: Đòn bẩy cao đi kèm rủi ro lớn, hãy thận trọng.

Giấy phép (License)
Dự án này chỉ phục vụ mục đích học tập và nghiên cứu. Tác giả không chịu trách nhiệm cho bất kỳ tổn thất tài chính nào phát sinh từ việc sử dụng mã nguồn này để giao dịch thực tế.
