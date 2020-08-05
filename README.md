# Dữ liệu ví dụ tham khảo

Trong thư mục `sample_data/videos` là dữ liệu ví dụ để các đội dự thi tham khảo, bao gồm:
* Mỗi video ghi nhận tình hình giao thông từ một camera cố định. 
* Đi kèm với mỗi video, bạn có một file JSON tương ứng, mô tả thông tin về vùng quan sát (ROI) và các hướng di chuyển (MOI). 
* Ngoài ra, ban tổ chức còn cung cấp một ảnh (PNG) minh họa vùng quan sát (ROI) và các hướng di chuyển (MOI) cần quan tâm trong video.

Trong thư mục `sample_data/submission` là ví dụ về file kết quả mà các đội sẽ nộp. Lưu ý là kết quả trong file kết quả ví dụ chỉ nhằm minh họa về cấu trúc trình bày, không nhất thiết là kết quả chính xác và đầy đủ tương ứng với các video ví dụ mẫu.

# Lời giải cơ bản (để tham khảo)

Nhằm giúp các bạn mới làm quen với Trí tuệ Nhân tạo tham gia cuộc thi, ban tổ chức giới thiệu một phương pháp giải đơn giản để bạn có thể thử nghiệm và tạo ra được lời giải cơ bản.

Phương pháp giải được hướng dẫn cho phép phát hiện hướng di chuyển (MOI) của phương tiện giao thông trong vùng quan sát (ROI) cùng thời điểm phương tiện rời khỏi ROI.

Cần lưu ý đây không phải là phương pháp giải hoàn chỉnh hay tối ưu, mà chỉ nhằm để giúp bạn làm quen với bài toán và tạo ra kết quả cho cuộc thi. 

Bạn có thể tiếp tục phát triển phương pháp giải của đội mình dựa trên phương pháp cơ bản này, hoặc chủ động phát triển phương pháp riêng. Các đội dự thi không bắt buộc sử dụng phương pháp được hướng dẫn này.

Các bạn có thể chạy lời giải cơ bản này trên Colab
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/hcmcaic/ai-challenge-baseline/blob/master/solution_baseline/dl_detect_tf2.ipynb)

Hướng dẫn này sử dụng các mã nguồn mở từ các nguồn sau:

* [TensorFlow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection)

* [Simple online and realtime tracking](https://github.com/abewley/sort)


---


Tổng quan phương pháp cơ bản được trình bày như sau:
* **Phát hiện phương tiện giao thông trong từng frame của video**: Sử dụng TensorFlow Object Detection API. Kết quả trả về là một danh sách các bounding box ứng với tất cả các phương tiện giao thông trong ảnh.
* **Theo vết (multiple objects tracking)**: Dựa vào IOU (chỉ số đo đạc mức độ trùng lắp của hai bounding box), các bounding box ở các frame liên tiếp sẽ được gom nhóm và từ đó sẽ hình thành quỹ đạo di chuyển của chính phương tiện đó.
* **Xác định hướng di chuyển (MOI) dựa trên quỹ đạo**: MOI của phương tiện sẽ được lựa chọn dựa trên độ tương đồng (ở đây sử dụng Cosine Similarity Score) giữa quỹ đạo của mỗi phương tiện và các MOI sẽ được tính toán.   

**Lưu ý**: Trong lời giải cơ bản này, chỉ minh họa việc phát hiện và đếm phương tiện xe máy. Bạn cần chỉnh sửa và cải tiến để có thể đếm được tất cả phương tiện giao thông mà đề bài yêu cầu.

