# YOLOv8 Export - Support Detection and Segmentation Models

## ⚠️ Important Notice: This project is currently under construction. ⚠️

This repo converts a YOLOv8 model in PyTorch (.pt) format to ONNX format using TensorRT for optimized inference.


## How to use
1. Clone the repository and install its dependencies :
```
git clone https://github.com/levipereira/ultralytics
cd ultralytics
```

2. Copy `yolov8_onnxtrt.py` to /ultralytics and export:
```
cd /ultralytics
python3 yolov8_onnxtrt.py  -w yolov8n.onnx 
```
### Arguments
- `-w, --weights`: Path to the weights (.pt) file of the YOLOv8 model (required).

### Layers Outputs

The following four output tensors are generated:


### Detection Models 
- **num_detections:**
  This is a `[batch_size, 1]` tensor of data type `int32`. The last dimension is a scalar indicating the number of valid detections per batch image.  

- **detection_boxes:**
  This is a `[batch_size, max_output_boxes, 4]` tensor of data type `float32` or `float16`, containing the coordinates of non-max suppressed boxes. The output coordinates will always be in BoxCorner format, regardless of the input code type.

- **detection_scores:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `float32` or `float16`, containing the scores for the boxes.

- **detection_classes:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `int32`, containing the classes for the boxes.


### Instance Segmentation Models 
- **num_detections:**
  This is a `[batch_size, 1]` tensor of data type `int32`. The last dimension is a scalar indicating the number of valid detections per batch image.  

- **detection_boxes:**
  This is a `[batch_size, max_output_boxes, 4]` tensor of data type `float32` or `float16`, containing the coordinates of non-max suppressed boxes. The output coordinates will always be in BoxCorner format, regardless of the input code type.

- **detection_scores:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `float32` or `float16`, containing the scores for the boxes.

- **detection_classes:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `int32`, containing the classes for the boxes.

- **detection_masks:**
  This is a `[batch_size, max_output_boxes]` tensor of data type `int32`, containing the masks for the boxes.


 
