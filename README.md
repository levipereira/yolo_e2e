# yolo_e2e
Implementation of End-to-End YOLO Models for TensorRT

## ⚠️ Important Notice: This project is currently under construction. ⚠️

This repository supports segmentation and detection models from the YOLO series - YOLOv10, YOLOv9, YOLOv8, and YOLOv7. 

All YOLO series models are implemented with End2End Deep Learning, incorporating three key features:

1. **Dynamic Shapes** - TensorRT enables the creation of network resolutions different from the original exported ONNX.
2. **Dynamic Batch Size** - Dynamically adjusts the batch size to maximize model performance according to the GPU's capabilities.
3. **NMS-Free** - Achieved in two different ways:
   1. **Native NMS-Free** - Models natively support NMS-Free, available for some YOLOv9 models and all YOLOv10 detection models.
   2. **TensorRT Plugins** - Utilizes TensorRT EfficientNMS plugin for detection models, and EfficientNMSX and ROIAlign for segmentation models.


All detection models across the YOLO series adhere to standardized output layers:

- `num_det`: Represents the number of detections.
- `det_boxes`: Provides the bounding boxes coordinates for each detected object.
- `det_scores`: Indicates the confidence score associated with each detected object.
- `det_classes`: Specifies the class label or category assigned to each detected object.

Similarly, instance segmentation models from the YOLO series also maintain standardized output layers:

- `num_det`: Represents the count of detected instances.
- `det_boxes`: Provides bounding box coordinates for each detected instance.
- `det_scores`: Indicates the confidence score associated with each detected instance.
- `det_classes`: Specifies the class label assigned to each detected instance.
- `det_masks`: Provides the segmentation masks corresponding to each detected instance.
