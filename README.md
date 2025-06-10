# Human_Detection

## Tổng quan
Notebook này hướng dẫn toàn bộ quy trình huấn luyện mô hình YOLOv8 để phát hiện người, bao gồm các bước chuẩn bị dữ liệu, cài đặt mô hình, huấn luyện, đánh giá, dự đoán và xuất mô hình.

## Mục lục
1. [Tải tập dữ liệu](#1-tải-tập-dữ-liệu)
2. [Cài đặt YOLOv8](#2-cài-đặt-yolov8)
3. [Tải mô hình pretrained](#3-tải-mô-hình-pretrained)
4. [Huấn luyện](#4-huấn-luyện)
5. [Đánh giá](#5-đánh-giá)
6. [Dự đoán](#6-dự-đoán)
7. [Xuất mô hình (Tùy chọn)](#7-xuất-mô-hình-tùy-chọn)

---

## 1. Tải tập dữ liệu
Bắt đầu bằng cách kết nối Google Drive và tải tập dữ liệu phát hiện người từ liên kết Google Drive, sau đó giải nén để sử dụng.

```python
from google.colab import drive
drive.mount('/content/gdrive')

!gdown https://drive.google.com/u/0/uc?id=1--0QuKMwj31K-CSvD8oq5fceFweiFPuN&export=download
!unzip /content/ultralytics/human_detection_dataset.zip
```

---

## 2. Cài đặt YOLOv8
Clone repository YOLOv8 từ GitHub và cài đặt các gói cần thiết.

```python
!git clone https://github.com/ultralytics/ultralytics
%cd ultralytics
%pip install ultralytics
import ultralytics
ultralytics.checks()
```

---

## 3. Tải mô hình pretrained
Tải mô hình YOLOv8 đã được huấn luyện sẵn để fine-tuning.

```python
%wget https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8s.pt
```

---

## 4. Huấn luyện
Tiến hành huấn luyện mô hình với cấu hình:
- Mô hình: `yolov8s.pt`
- Tập dữ liệu: `./human_detection_dataset/data.yaml`
- Số epoch: 20
- Kích thước ảnh: 640

```python
!yolo train model=yolov8s.pt data=./human_detection_dataset/data.yaml epochs=20 imgsz=640
```

---

## 5. Đánh giá
Đánh giá mô hình sau huấn luyện trên tập validation.

```python
!yolo val model=./runs/detect/train2/weights/best.pt data=../human_detection_dataset/data.yaml
```

---

## 6. Dự đoán
Sử dụng mô hình để dự đoán trên:
- Ảnh tải lên
- Ảnh online
- Video YouTube

```python
# Với ảnh tải lên
!yolo predict model=./runs/detect/train/weights/best.pt source='/content/ultralytics/frame007.25.00-07.30.00.jpg'

# Với ảnh online
!yolo predict model=./runs/detect/train/weights/best.pt source='https://assets.weforum.org/article/image/XaHpf_z51huQS_JPHs-jkPhBp0dLlxFJwt-sPLpGJB0.jpg'

# Với video YouTube
!yolo predict model=./runs/detect/train/weights/best.pt source='https://youtu.be/MsXdUtlDVhk'
```

---

## 7. Xuất mô hình (Tùy chọn)
Xuất mô hình sang định dạng ONNX và lưu lên Google Drive để sử dụng sau này.

```python
# Chuyển đổi sang định dạng khác
!yolo export model=./runs/detect/train/weights/best.pt format=onnx

# Lưu lên Google Drive
!cp '/content/ultralytics/runs/detect/train/weights/best.onnx' '/content/gdrive/MyDrive/Coordinate/aio_2023_ta/module1/yolov8_project/solution/weights'
!cp '/content/ultralytics/runs/detect/train/weights/best.pt' '/content/gdrive/MyDrive/Coordinate/aio_2023_ta/module1/yolov8_project/solution/weights'
```

---

## Yêu cầu hệ thống
- Môi trường Google Colab với GPU
- Truy cập Google Drive để lưu trữ dữ liệu và mô hình
- Kết nối internet để tải dữ liệu và mô hình

## Lưu ý
- Kiểm tra kỹ đường dẫn tập dữ liệu trong file `data.yaml`
- Điều chỉnh số epoch và kích thước ảnh phù hợp với tài nguyên tính toán
