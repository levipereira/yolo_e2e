# YOLOv7 Export - Support Detection and Segmentation Models

You can use official Yolov7 Repo : https://github.com/WongKinYiu/yolov7

## Export TensorRT

See https://github.com/WongKinYiu/yolov7#export for more info.

```bash
#install onnx-simplifier not listed in general yolov7 requirements.txt
pip3 install onnx-simplifier 

# Pytorch Yolov7 -> ONNX with grid, EfficientNMS plugin and dynamic batch size
python export.py --weights ./yolov7.pt --grid --end2end --dynamic-batch --simplify --topk-all 100 --iou-thres 0.65 --conf-thres 0.35 --img-size 640 640
```
