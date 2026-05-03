# 🏦 Chương trình mô phỏng lập lịch FCFS cho giao dịch Ngân hàng

Mô phỏng thuật toán **First Come First Served (FCFS)** áp dụng trong hệ thống xử lý giao dịch ngân hàng online. Chương trình tính toán và hiển thị thời gian chờ, thời gian hoàn tất cho từng giao dịch.

---

## 📋 Mục lục

- [Giới thiệu](#giới-thiệu)
- [Cấu trúc chương trình](#cấu-trúc-chương-trình)
- [Cách chạy](#cách-chạy)
- [Ví dụ đầu ra](#ví-dụ-đầu-ra)
- [Giải thích thuật toán](#giải-thích-thuật-toán)

---

## 📖 Giới thiệu

**FCFS (First Come First Served)** là thuật toán lập lịch đơn giản nhất: giao dịch nào đến trước sẽ được xử lý trước. Trong ngữ cảnh ngân hàng, đây là cơ chế đảm bảo tính công bằng và thứ tự xử lý theo hàng đợi FIFO.

| Thuộc tính | Giá trị |
|---|---|
| Thuật toán | FCFS (Non-preemptive) |
| Ngôn ngữ | Python 3 |
| Ứng dụng | Mô phỏng hệ thống giao dịch ngân hàng |

---

## 🗂 Cấu trúc chương trình

```
fcfs_ngan_hang/
├── fcfs_simulation.py   # File chương trình chính
└── README.md
```

### Class `Transaction`

Đại diện cho một giao dịch ngân hàng với các thuộc tính:

| Thuộc tính | Kiểu | Mô tả |
|---|---|---|
| `id` | `str` | Mã định danh giao dịch |
| `arrival` | `int` | Thời điểm giao dịch đến (ms) |
| `burst` | `int` | Thời gian xử lý cần thiết (ms) |
| `start_time` | `int` | Thời điểm bắt đầu xử lý |
| `finish_time` | `int` | Thời điểm hoàn tất xử lý |
| `waiting_time` | `int` | Thời gian chờ trong hàng đợi |
| `turnaround_time` | `int` | Tổng thời gian từ lúc đến đến lúc xong |

### Hàm `simulate_fcfs(transactions)`

- Sắp xếp giao dịch theo `arrival` (đảm bảo FIFO)
- Tính `start_time`, `finish_time`, `waiting_time`, `turnaround_time` cho từng giao dịch
- In bảng kết quả và các chỉ số trung bình

---

## ▶️ Cách chạy

**Yêu cầu:** Python 3.6 trở lên

```bash
python fcfs_simulation.py
```

Để thêm giao dịch tuỳ chỉnh, chỉnh sửa danh sách `data` trong khối `__main__`:

```python
data = [
    Transaction("T1-SaoKe",   arrival=0, burst=6),
    Transaction("T2-ChuyenK", arrival=1, burst=2),
    Transaction("T3-XemSD",   arrival=2, burst=1),
    Transaction("T4-ThanhT",  arrival=3, burst=3),
]
```

---

## 📊 Ví dụ đầu ra

Với dữ liệu mô phỏng phần **4.2**:

```
--- Tiến trình xử lý FCFS ---
GiaoDịch  |Arrival |Burst |Start |Finish  |WT   |TAT  
-----------------------------------------------------------------
T1-SaoKe  |0       |6     |0     |6       |0    |6    
T2-ChuyenK|1       |2     |6     |8       |5    |7    
T3-XemSD  |2       |1     |8     |9       |6    |7    
T4-ThanhT |3       |3     |9     |12      |6    |9    
-----------------------------------------------------------------
Thời gian chờ trung bình   (AVG WT):  4.25 ms
Thời gian hoàn tất trung bình (AVG TAT): 7.25 ms
```

### Biểu đồ Gantt

| Giao dịch  | 0-1 | 1-2 | 2-3 | 3-4 | 4-5 | 5-6 | 6-7 | 7-8 | 8-9 | 9-10 | 10-11 | 11-12 |
|------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|------|-------|-------|
| T1-SaoKe   | ███ | ███ | ███ | ███ | ███ | ███ |     |     |     |      |       |       |
| T2-ChuyenK |     |     |     |     |     |     | ███ | ███ |     |      |       |       |
| T3-XemSD   |     |     |     |     |     |     |     |     | ███ |      |       |       |
| T4-ThanhT  |     |     |     |     |     |     |     |     |     | ███  | ███   | ███   |

## 🧮 Giải thích thuật toán

### Các công thức tính
Finish Time      = Start Time + Burst Time
Turnaround Time  = Finish Time - Arrival Time
Waiting Time     = Turnaround Time - Burst Time
### Xử lý server rỗi

Nếu server hoàn tất giao dịch trước khi giao dịch tiếp theo đến, `current_time` sẽ nhảy tới `arrival` của giao dịch kế:

```python
if current_time < t.arrival:
    current_time = t.arrival
```

### Nhận xét

> **Convoy Effect:** Giao dịch `T1-SaoKe` có burst time lớn (6ms) khiến `T2`, `T3`, `T4` phải chờ lần lượt 5ms, 6ms, 6ms — đây là hạn chế đặc trưng của FCFS khi có giao dịch nặng xuất hiện đầu hàng đợi.

---

## 📄 Giấy phép

Dự án được thực hiện cho mục đích học thuật — Tiểu luận Hệ điều hành.
