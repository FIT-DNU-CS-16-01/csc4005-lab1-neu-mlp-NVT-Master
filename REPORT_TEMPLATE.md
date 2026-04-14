# CSC4005 – Lab 1 Report

## 1. Mục tiêu
Mục tiêu của Lab 1 là huấn luyện mô hình MLP để phân loại lỗi bề mặt thép trên bộ dữ liệu NEU-CLS (6 lớp: Crazing, Inclusion, Patches, Pitted_Surface, Rolled-in_Scale, Scratches), sau đó so sánh các cấu hình huấn luyện và chọn cấu hình tốt nhất theo tập validation.

## 2. Cấu hình thí nghiệm
Các cấu hình đã chuẩn bị và/hoặc chạy:

| Config | run_name | Optimizer | LR | Weight Decay | Dropout | Epochs | Batch size | Augment | Trạng thái |
|---|---|---|---:|---:|---:|---:|---:|---|---|
| C1 | baseline_adamw | AdamW | 0.001 | 0.0001 | 0.3 | 20 | 32 | Có | Đã có kết quả |
| C2 | quick_test | AdamW | 0.001 | 0.0001 | 0.3 | 20 | 32 | Có | Đã có kết quả |
| C3 | sgd_exp | SGD | 0.01 | 0.0001 | 0.3 | 12 | 32 | Có | Chưa có kết quả (thiếu dữ liệu cục bộ) |

## 3. Kết quả

### 3.1 Bảng metrics
| Config | Best Val Loss | Best Val Acc | Test Loss | Test Acc |
|---|---:|---:|---:|---:|
| C1 (baseline_adamw) | 1.5036 | 0.4037 | 1.5018 | 0.3593 |
| C2 (quick_test) | 1.4467 | 0.4593 | 1.4281 | 0.4556 |
| C3 (sgd_exp) | N/A | N/A | N/A | N/A |

### 3.2 Nhận xét learning curves
- C1: train acc tăng từ 0.1635 lên 0.3651; val acc tăng từ 0.1407 lên 0.4037. Đường học tăng chậm nhưng ổn định.
- C2: train acc tăng từ 0.1817 lên 0.3492; val acc tăng từ 0.1593 lên 0.4593. Validation tốt hơn C1 ở giai đoạn cuối.
- So sánh C1 và C2 cho thấy C2 hội tụ tốt hơn trên validation và cho test accuracy cao hơn.

## 4. Phân tích

### 4.1 Cấu hình tốt nhất
Cấu hình tốt nhất trong các run đã có kết quả là C2 (quick_test), vì có cả best validation accuracy (0.4593) và test accuracy (0.4556) cao hơn C1.

### 4.2 Dấu hiệu overfitting / underfitting
- Cả C1 và C2 đều có train accuracy còn thấp (khoảng 0.35) và val accuracy cũng chưa cao, cho thấy mô hình còn xu hướng underfitting nhẹ.
- C2 có đoạn dao động ở các epoch giữa (val acc giảm rồi tăng lại), nhưng không thấy khoảng cách train-val quá lớn, nên chưa có dấu hiệu overfitting rõ rệt.

### 4.3 AdamW và SGD trong thí nghiệm này
- Hiện tại mới có kết quả thực nghiệm với AdamW (C1, C2), chưa có số liệu SGD hoàn chỉnh để đối chiếu định lượng công bằng.
- Về kỳ vọng: AdamW thường hội tụ nhanh và ổn định hơn với MLP trong giai đoạn đầu; SGD có thể cần tinh chỉnh learning rate/scheduler kỹ hơn để đạt kết quả tương đương.
- Để kết luận chính xác, cần chạy xong C3 (SGD) trên cùng điều kiện dữ liệu và so sánh trực tiếp metrics.

## 5. Kết luận
Tạm thời chọn C2 (quick_test) là best config vì cho hiệu năng validation/test tốt nhất trong các kết quả hiện có.

Đề xuất bước tiếp theo:
1. Khôi phục đường dẫn dữ liệu và chạy xong C3 (SGD).
2. Bổ sung bảng so sánh đầy đủ giữa AdamW và SGD.
3. Chốt cấu hình cuối cùng sau khi có kết quả C3.
