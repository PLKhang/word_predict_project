
# 🤖 Context-Aware Word Prediction with Transformer

Dự án huấn luyện mô hình Transformer nhỏ (DistilGPT2) để **gợi ý từ tiếp theo phù hợp với ngữ cảnh khi người dùng đang gõ**, tương tự như IntelliSense hoặc autocomplete trong VSCode. Mô hình được huấn luyện từ dữ liệu nội bộ (3 triệu dòng văn bản), tối ưu để **deploy real-time trên web/mobile**.

---

## 📌 Mục tiêu

- Gợi ý từ tiếp theo theo ngữ cảnh (context-aware word prediction)
- Mô hình nhẹ, deploy được trên frontend/mobile
- Tự huấn luyện từ dữ liệu nội bộ (đặc thù ngôn ngữ công ty)
- Có thể resume training nếu bị gián đoạn trên Kaggle (do giới hạn thời gian)

---

## 🗂️ Cấu trúc thư mục

```bash
word_predict_project/
│
├── data/
│   └── train_data.txt             # Dữ liệu văn bản (1 dòng = 1 task hoặc câu)
│
├── src/
│   └── predictor.py               # Class dự đoán từ tiếp theo
│
├── notebooks/
│   └── word_predict_train.ipynb  # Notebook huấn luyện mô hình trên Kaggle
│
├── models/
│   └── gpt2-word-predict/         # Mô hình đã huấn luyện (sẽ tạo sau training)
│
├── requirements.txt              # Thư viện cần thiết
└── README.md                     # Hướng dẫn chi tiết
```

---

## ⚙️ Cài đặt môi trường (nếu chạy local)

```bash
pip install -r requirements.txt
```

---

## 📚 Dữ liệu huấn luyện

- Tệp: `data/train_data.txt`
- Dạng: plain text, mỗi dòng là một đơn vị huấn luyện (task, câu, báo cáo)
- Ví dụ:

```text
Hôm nay tôi đã hoàn thành task tạo API cho hệ thống quản lý thú cưng.
Yêu cầu đặt ra là hệ thống có thể gợi ý dịch vụ phù hợp cho khách hàng.
...
```

---

## 🧠 Huấn luyện mô hình (Kaggle hoặc local)

### 👉 Trên Kaggle

1. Tạo notebook mới tại: https://www.kaggle.com/code
2. Upload hoặc `Add Data` thư mục `data/`
3. Chạy `notebooks/word_predict_train.ipynb`

> Mô hình sẽ được lưu tại `/kaggle/working/model/`, bạn có thể zip lại và tải xuống

### 👉 Nếu train chưa xong trong 30h?

- HuggingFace `Trainer` sẽ tự lưu checkpoint (ví dụ: `checkpoint-1000`)
- Zip lại thư mục `model/` và tải về
- Trong lần sau, upload lại checkpoint và `resume_from_checkpoint=True` để train tiếp

---

## 🧪 Sử dụng mô hình dự đoán

### Class `WordPredictor`:

```python
from src.predictor import WordPredictor

predictor = WordPredictor("models/gpt2-word-predict")
print(predictor.suggest_next("next m", top_k=5, prefix_filter="m"))
```

### Kết quả ví dụ:

```text
['monday', 'morning', 'month', 'meeting', 'march']
```

---

## 🚀 Triển khai real-time (tùy chọn)

- Convert mô hình sang:
  - `TFJS`: để dùng với web client
  - `TFLite`: để dùng với Android/iOS
  - `ONNX`: để dùng trong backend, Rust, C++, etc.

> Gợi ý sử dụng `optimum` của HuggingFace để export ONNX

---

