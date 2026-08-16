# Video narration script and recording steps

No video was recorded. Use this script to make a six-minute Vietnamese demo
later. Keep JMeter and Activity Monitor in the same frame.

1. Open the backend terminal, JMeter, and Activity Monitor. Resize JMeter to
   the left and Activity Monitor to the right. Start recording.
2. Say: “Em là sinh viên 23127081. Đây là bài HW05 về kiểm thử hiệu năng cho
   EShop. Máy của em là MacBook Pro M1, 16 GB RAM. Em dùng Apache JMeter.”
3. Show `23127081_Load_20260816.jmx` and `read_input.csv`. Say: “Load test
   dùng ba API đọc: giỏ hàng, lịch sử đơn, và chi tiết đơn. Test có 20 users,
   ramp-up 60 giây, think time một giây.” Run it and point to CPU/RAM.
4. Show `23127081_Stress_20260816.jmx` and `auth_input.csv`. Say: “Stress test
   dùng đăng ký, quên mật khẩu, và đặt lại mật khẩu. Em không dùng login nên
   không kích hoạt cơ chế khoá sau ba lần sai.” Run it and show Aggregate Report.
5. Show `23127081_Spike_20260816.jmx` and `transaction_input.csv`. Say: “Spike
   test chạy baseline 2 users, tăng nhanh lên 30 users, rồi quay lại 2 users.
   Workflow áp mã, tạo coupon tạm, và xoá coupon tạm.” Run each stage and show
   View Results Tree plus Activity Monitor.
6. Show `results/` and the HTML reports. Say: “Raw JTL và HTML report được lưu
   đầy đủ. Endurance 10 phút đạt 92.70 RPS, p95 83 ms, và không có lỗi.”
7. Show GitHub Issue #1. Say: “Em cũng tìm thấy lỗi tính phần trăm coupon và
   đã báo cáo trên GitHub. Cảm ơn thầy/cô.” Stop recording.
