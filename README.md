# CT Pneumonia Segmentation with Novel U-Net

## Giới thiệu dự án

Dự án này xây dựng mô hình học sâu để phân vùng vùng tổn thương viêm phổi trên ảnh chụp cắt lớp vi tính ngực. Bài toán phân vùng ảnh y tế có mục tiêu xác định chính xác vùng bất thường trên ảnh, từ đó hỗ trợ quá trình quan sát, đánh giá và phân tích tổn thương trong phổi.

Trong dự án, mô hình **Novel U-Net** được sử dụng cho bài toán phân vùng nhị phân, trong đó đầu vào là ảnh CT ngực và đầu ra là mặt nạ phân vùng biểu diễn vùng viêm phổi. Kiến trúc U-Net phù hợp với bài toán này vì có cấu trúc mã hóa - giải mã, giúp trích xuất đặc trưng ở nhiều mức khác nhau và khôi phục lại bản đồ phân vùng ở kích thước ảnh ban đầu.

## Mục tiêu

Mục tiêu chính của dự án gồm:

* Xây dựng mô hình Novel U-Net cho bài toán phân vùng vùng viêm phổi trên ảnh CT.
* Tiền xử lý dữ liệu ảnh và mặt nạ phân vùng trước khi đưa vào mô hình.
* Huấn luyện mô hình trên các tập dữ liệu khác nhau.
* Kiểm thử chéo giữa các tập dữ liệu để đánh giá khả năng tổng quát hóa của mô hình.
* So sánh kết quả giữa trường hợp ảnh đã crop và chưa crop.

## Cấu trúc thư mục

```text
CT_pneumonia_segmentation_with_novel_unet/
│
├── Chua_crop_anh/
│   ├── pneumonia-with-novel-unet-test-d1-train-d2-ver3.ipynb
│   ├── pneumonia-with-novel-unet-test-d1-train-d3-ver3.ipynb
│   ├── pneumonia-with-novel-unet-test-d2-train-d3-ver3.ipynb
│   └── pneumonia-with-novel-unet-test-d3-train-d2-ver1.ipynb
│
├── Crop_anh/
│   ├── pneumonia-with-novel-unet-test-d1-train-d2-ver2.ipynb
│   ├── pneumonia-with-novel-unet-test-d1-train-d3-ver2.ipynb
│   ├── pneumonia-with-novel-unet-test-d2-train-d3-ver2.ipynb
│   └── pneumonia-with-novel-unet-train-d2-test-d3 (2).ipynb
```

Trong đó:

* `Chua_crop_anh/`: chứa các notebook thực nghiệm với dữ liệu ảnh chưa crop.
* `Crop_anh/`: chứa các notebook thực nghiệm với dữ liệu ảnh đã crop.
* Các file notebook được đặt tên theo dạng `train-dX-test-dY`, thể hiện tập dữ liệu dùng để huấn luyện và tập dữ liệu dùng để kiểm thử.

## Mô hình sử dụng

Dự án sử dụng mô hình **Novel U-Net**, một biến thể của U-Net cho bài toán phân vùng ảnh y tế. Mô hình gồm hai phần chính:

### 1. Encoder

Encoder có nhiệm vụ trích xuất đặc trưng từ ảnh đầu vào. Qua từng tầng, kích thước không gian của ảnh giảm dần, trong khi số lượng đặc trưng tăng lên. Nhờ đó, mô hình học được các thông tin quan trọng ở nhiều mức khác nhau, từ đặc trưng biên, vùng sáng tối cho đến đặc trưng ngữ nghĩa phức tạp hơn.

### 2. Decoder

Decoder có nhiệm vụ khôi phục lại kích thước không gian của ảnh để tạo ra mặt nạ phân vùng. Các đặc trưng từ encoder được kết nối sang decoder thông qua các skip connection dày đặc, giúp mô hình giữ lại thông tin chi tiết của ảnh gốc và cải thiện độ chính xác ở biên vùng tổn thương.

## Quy trình thực hiện

Quy trình tổng quát của dự án gồm các bước:

1. Đọc dữ liệu ảnh CT và mặt nạ tương ứng.
2. Tiền xử lý dữ liệu, bao gồm chuẩn hóa ảnh, resize ảnh và xử lý mặt nạ. Với trường hợp đề xuất cải thiện độ chính xác của kết quả dự đoán thì bước tiền xử lý còn có thêm phần cắt ảnh để loại bỏ bớt vùng nền.
3. Chia dữ liệu thành các tập huấn luyện và kiểm thử.
4. Xây dựng mô hình Novel U-Net.
5. Huấn luyện mô hình trên tập dữ liệu đã chọn.
6. Dự đoán mặt nạ phân vùng trên tập kiểm thử.
7. Đánh giá kết quả bằng các độ đo phân vùng.
8. Trực quan hóa ảnh đầu vào, mặt nạ thật và mặt nạ dự đoán.

## Thực nghiệm kiểm thử chéo

Dự án thực hiện kiểm thử chéo giữa các tập dữ liệu D1, D2 và D3. Cách đánh giá này giúp kiểm tra khả năng tổng quát hóa của mô hình khi được huấn luyện trên một tập dữ liệu nhưng kiểm thử trên một tập dữ liệu khác.

Ví dụ:

* Huấn luyện trên D2, kiểm thử trên D1.
* Huấn luyện trên D3, kiểm thử trên D1.
* Huấn luyện trên D3, kiểm thử trên D2.
* Huấn luyện trên D2, kiểm thử trên D3.

Việc kiểm thử chéo có ý nghĩa quan trọng trong bài toán ảnh y tế, vì dữ liệu có thể thay đổi theo nguồn thu thập, thiết bị chụp, chất lượng ảnh và đặc điểm bệnh lý.

## Bộ dữ liệu

Dữ liệu sử dụng trong dự án là ảnh CT ngực và mặt nạ phân vùng vùng viêm phổi. Các tập dữ liệu được ký hiệu là D1, D2 và D3. Mỗi tập dữ liệu được sử dụng trong các thí nghiệm huấn luyện và kiểm thử khác nhau nhằm đánh giá độ ổn định của mô hình.

## Kết quả đầu ra

Sau khi huấn luyện và kiểm thử, mô hình tạo ra mặt nạ phân vùng dự đoán cho từng ảnh CT đầu vào. Kết quả có thể được trực quan hóa dưới dạng:

* Ảnh CT gốc.
* Mặt nạ phân vùng thật.
* Mặt nạ phân vùng do mô hình dự đoán.
* So sánh trực quan giữa mặt nạ thật và mặt nạ dự đoán.

## Công nghệ sử dụng

Dự án được triển khai chủ yếu bằng Jupyter Notebook, phù hợp cho quá trình thử nghiệm, huấn luyện mô hình và trực quan hóa kết quả. Một số thư viện thường dùng trong bài toán này gồm:

* Python
* NumPy
* OpenCV
* Matplotlib
* TensorFlow/Keras hoặc PyTorch
* Scikit-learn

## Ý nghĩa của dự án

Dự án giúp áp dụng học sâu vào bài toán phân vùng ảnh y tế, cụ thể là phân vùng vùng viêm phổi trên ảnh CT ngực. Kết quả của mô hình có thể hỗ trợ quá trình phân tích ảnh y tế bằng cách làm nổi bật vùng tổn thương, giúp việc quan sát và đánh giá trở nên trực quan hơn.

Ngoài ra, việc thực nghiệm trên cả dữ liệu đã crop và chưa crop giúp đánh giá ảnh hưởng của bước tiền xử lý đến chất lượng phân vùng. Kiểm thử chéo giữa các tập dữ liệu cũng giúp xem xét khả năng tổng quát hóa của mô hình trong các điều kiện dữ liệu khác nhau.

