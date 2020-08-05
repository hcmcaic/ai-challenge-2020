# Lời giải cơ bản cho cuộc thi

Đây là hướng dẫn một phương pháp cơ bản dùng để phát hiện hướng di chuyển (MOI) của phương tiện giao thông trong vùng trong vùng quan sát (ROI) cùng thời điểm phương tiện rời khỏi ROI.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/hcmcaic/ai-challenge-baseline/blob/master/dl_detect_tf2.ipynb)

Hướng dẫn này sử dụng các mã nguồn mở từ các nguồn sau:

* [TensorFlow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection)

* [Simple online and realtime tracking](https://github.com/abewley/sort)


---


Tổng quan phương pháp cơ bản được trình bày như sau:
* **Phát hiện phương tiện giao thông trong từng frame của video**: Sử dụng TensorFlow Object Detection API. Kết quả trả về là một danh sách các bounding box ứng với tất cả các phương tiện giao thông trong ảnh.
* **Theo vết (multiple objects tracking)**: Dựa vào IOU (chỉ số đo đạc mức độ trùng lắp của hai bounding box), các bounding box ở các frame liên tiếp sẽ được gom nhóm và từ đó sẽ hình thành quỹ đạo di chuyển của chính phương tiện đó.
* **Xác định hướng di chuyển (MOI) dựa trên quỹ đạo**: MOI của phương tiện sẽ được lựa chọn dựa trên độ tương đồng (ở đây sử dụng Cosine Similarity Score) giữa quỹ đạo của mỗi phương tiện và các MOI sẽ được tính toán.   

**Lưu ý**: Trong lời giải cơ bản này, chỉ minh họa việc phát hiện và đếm phương tiện xe máy. Bạn cần chỉnh sửa và cải tiến để có thể đếm được tất cả phương tiện giao thông mà đề bài yêu cầu.
