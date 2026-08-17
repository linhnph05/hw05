# Video Narration and Recording Steps

No video was recorded. This guide is for a later six-minute Vietnamese demo.
Keep the JMeter CLI terminal or its HTML dashboard on one side of the screen
and `htop` on the other side.

## 1. Before recording

1. Start the EShop backend.
2. Open the three JMeter plans.
3. Open the `results/` folder and the four HTML dashboards.
4. Open a terminal and run `htop -p "$(pgrep -f 'node server.js' | tail -n 1)"`.
5. Arrange the windows so the test tool and backend resources are visible in
   the same frame.

## 2. Narration script

### 0:00–0:40 — Introduction

Show the report title and computer information.

> Em là Nguyễn Phan Hùng Linh, sinh viên 23127081. Đây là bài HW05 về kiểm
> thử hiệu năng cho hệ thống EShop. Em dùng Apache JMeter 5.6.3 trên MacBook
> Pro M1, 16 GB RAM. Em dùng screenfetch để ghi lại thông tin máy. Backend dùng
> Node.js, Express và SQLite.

### 0:40–1:25 — Scope and test design

Show the endpoint table and the three CSV files.

> Em kiểm thử ba nhóm API. Load test dùng ba API đọc về giỏ hàng và đơn hàng.
> Stress test dùng đăng ký, quên mật khẩu và đặt lại mật khẩu. Spike test dùng
> ba API coupon. Mỗi test có một file CSV riêng và một loại report riêng.

### 1:25–2:20 — Load test

Show `23127081_Load_20260816.jmx`, `read_input.csv`, the Load dashboard, and
the backend resource terminal.

> Load test chạy 20 users, ramp-up 60 giây, thời gian 300 giây và think time
> một giây. Kết quả có 5.348 samples, throughput 17,8 RPS, p95 30 ms và không
> có lỗi. Peak CPU của backend là 17,3 phần trăm.

### 2:20–3:15 — Stress test

Show `23127081_Stress_20260816.jmx`, `auth_input.csv`, the Stress dashboard,
and backend resources.

> Stress test chạy 30 users và tạo nhiều thao tác ghi vào SQLite. Kết quả có
> 8.522 samples, throughput 47,3 RPS, p95 139 ms và không có lỗi. Em không dùng
> API login nên không kích hoạt cơ chế khoá sau ba lần đăng nhập sai.

### 3:15–4:20 — Spike test

Show `23127081_Spike_20260816.jmx`, `transaction_input.csv`, and the three
stages in `results/spike_execution.md`.

> Spike test có ba giai đoạn: 2 users baseline, tăng nhanh lên 30 users, sau đó
> quay lại 2 users. Ở giai đoạn tải cao, throughput là 54,25 RPS và p95 là 103
> ms. Ở giai đoạn recovery, p95 giảm còn 40 ms. Cả ba giai đoạn đều không có
> lỗi.

### 4:20–5:15 — Endurance test

Show the Endurance dashboard and resource data.

> Endurance test chạy 100 users trong 10 phút. Test hoàn thành 55.622 samples,
> đạt 92,70 RPS, p95 83 ms và không có lỗi. Peak CPU của backend là 26,9 phần
> trăm và peak RSS là 76.000 KB. Đây là mức ổn định đã kiểm thử, không phải giới
> hạn tuyệt đối của máy.

### 5:15–6:05 — AI review and conclusion

Show the AI audit, the corrected Spike values, and the continuous-testing flow
chart.

> AI hỗ trợ thiết kế test và phân tích kết quả. Tuy nhiên, AI đọc sai p95 của
> Spike. Em kiểm tra raw JTL và sửa lại thành 93 ms cho file gộp và 103 ms cho
> giai đoạn tải cao. Em đề xuất chạy smoke test và load test khi backend thay
> đổi, còn stress và endurance chạy hằng đêm. Kết luận: tất cả test đều không
> có lỗi và mức endurance ổn định đã đo được là 92,70 RPS. Cảm ơn thầy cô.

Stop the recording after the conclusion. The total target time is about six
minutes and five seconds.
