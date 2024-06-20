# YOLOv10 ONNX End2End for TensorRT


![image](https://github.com/levipereira/yolo_e2e/assets/22964932/d31fe5f2-5a1c-4065-9b23-88f6631bdf09)

This repo converts a YOLOv10 model in PyTorch (.pt) format to ONNX format using TensorRT for optimized inference.


## How to use
1. Clone the repository and install its dependencies :
```
git clone https://github.com/THU-MIG/yolov10.git
cd /yolov10
```

2. Copy `yolov10_onnxtrt.py` to /yolov10/ and export:
```
cd /yolov10
python3 yolov10_onnxtrt.py  -w yolov10n.onnx 
```
### Arguments
- `-w, --weights`: Path to the weights (.pt) file of the YOLOv10 model (required).
- `-s, --size`: Inference size [H, W]. Default is [640].
- `--opset`: ONNX opset version. Default is 16.
- `--max_det`: Maximum detections per image. Default is 300.

 
### Layers Outputs

The following four output tensors are generated:

- **num_detections:**
  This is a `[batch_size, 1]` tensor of data type `int32`. The last dimension is a scalar indicating the number of valid detections per batch image.  

- **detection_boxes:**
  This is a `[batch_size, max_output_boxes, 4]` tensor of data type `float32` or `float16`, containing the coordinates of non-max suppressed boxes. The output coordinates will always be in BoxCorner format, regardless of the input code type.

- **detection_scores:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `float32` or `float16`, containing the scores for the boxes.

- **detection_classes:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `int32`, containing the classes for the boxes.
