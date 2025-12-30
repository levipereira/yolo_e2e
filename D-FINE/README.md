# D-FINE ONNX Exporter

Export D-FINE object detection models to ONNX format for deployment with TensorRT and DeepStream.

## Output Format

The exported ONNX model has the following outputs:

| Output | Shape | Type | Description |
|--------|-------|------|-------------|
| `num_dets` | `[batch]` | int32 | Number of detections per image |
| `det_boxes` | `[batch, num_queries, 4]` | float32 | Bounding boxes (x1, y1, x2, y2) |
| `det_scores` | `[batch, num_queries]` | float32 | Confidence scores |
| `det_classes` | `[batch, num_queries]` | int32 | Class labels |

## Installation

### 1. Clone D-FINE repository

```bash
git clone https://github.com/Peterande/D-FINE.git
cd D-FINE
pip3 install -r requirements.txt
pip3 install onnx onnxslim onnxruntime onnxscript
```

### 2. Copy exporter

Copy the `export_dfine.py` file to the D-FINE folder.

## Usage

### 3. Download the model

Download the `.pth` file from [D-FINE releases](https://github.com/Peterande/D-FINE/releases).

Example for D-FINE-S:

```bash
wget https://github.com/Peterande/storage/releases/download/dfinev1.0/dfine_s_coco.pth
```

> **Note:** You can use your custom trained model.

### 4. Convert model

Generate the ONNX model file.

Example for D-FINE-S:

```bash
python3 export_dfine.py -w dfine_s_coco.pth -c configs/dfine/dfine_hgnetv2_s_coco.yml --simplify --dynamic
```

## Arguments

| Argument | Description | Default |
|----------|-------------|---------|
| `-w`, `--weights` | Input weights (.pth) file path | Required |
| `-c`, `--config` | Input YAML (.yml) config file path | Required |
| `-s`, `--size` | Inference size [H, W] | 640 |
| `--opset` | ONNX opset version | 18 |
| `--simplify` | Simplify ONNX model | False |
| `--dynamic` | Enable dynamic batch-size | False |
| `--batch` | Static batch-size | 1 |
| `--license` | License metadata for the ONNX model | "" |

## Examples

### Change inference size

Default size is 640x640. To change:

```bash
# Square size
python3 export_dfine.py -w model.pth -c config.yml -s 1280

# Custom height and width
python3 export_dfine.py -w model.pth -c config.yml -s 1280 1280
```

### Simplify ONNX model

Recommended for DeepStream >= 6.0:

```bash
python3 export_dfine.py -w model.pth -c config.yml --simplify
```

### Dynamic batch-size

Required for DeepStream >= 6.1 with variable batch sizes:

```bash
python3 export_dfine.py -w model.pth -c config.yml --dynamic
```

### Static batch-size

For fixed batch-size (e.g., batch = 4):

```bash
python3 export_dfine.py -w model.pth -c config.yml --batch 4
```

### Add license metadata

```bash
python3 export_dfine.py -w model.pth -c config.yml --license "Your License"
```

### Full example

```bash
python3 export_dfine.py \
    -w dfine_s_coco.pth \
    -c configs/dfine/dfine_hgnetv2_s_coco.yml \
    -s 640 \
    --simplify \
    --dynamic \
    --license "MIT"
```

## TensorRT Conversion

After exporting to ONNX, convert to TensorRT engine:

### Static batch-size

```bash
trtexec --onnx=model.onnx --saveEngine=model.engine
```

### Dynamic batch-size

```bash
trtexec --onnx=model.onnx --saveEngine=model.engine \
    --minShapes=images:1x3x640x640 \
    --optShapes=images:4x3x640x640 \
    --maxShapes=images:8x3x640x640
```

## Available Models

| Model | Config |
|-------|--------|
| D-FINE-S (COCO) | `configs/dfine/dfine_hgnetv2_s_coco.yml` |
| D-FINE-M (COCO) | `configs/dfine/dfine_hgnetv2_m_coco.yml` |
| D-FINE-L (COCO) | `configs/dfine/dfine_hgnetv2_l_coco.yml` |
| D-FINE-X (COCO) | `configs/dfine/dfine_hgnetv2_x_coco.yml` |
| D-FINE-L (Objects365 → COCO) | `configs/dfine/objects365/dfine_hgnetv2_l_obj2coco.yml` |
| D-FINE-X (Objects365 → COCO) | `configs/dfine/objects365/dfine_hgnetv2_x_obj2coco.yml` |

## Citation

```bibtex
@misc{peng2024dfine,
      title={D-FINE: Redefine Regression Task in DETRs as Fine-grained Distribution Refinement},
      author={Yansong Peng and Hebei Li and Peixi Wu and Yueyi Zhang and Xiaoyan Sun and Feng Wu},
      year={2024},
      eprint={2410.13842},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

## Acknowledgments

This exporter script is a modified version based on the original work from [DeepStream-Yolo](https://github.com/marcoslucianops/DeepStream-Yolo/blob/master/docs/DFINE.md) by [@marcoslucianops](https://github.com/marcoslucianops).

