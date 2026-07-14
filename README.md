# MEASY AIManager Demo Assets

RK3576 边缘 AI 演示平台 — Demo 可执行文件、模型、配置资源。

## 结构

每个 Demo 独立目录，包含可执行程序、NPU 模型文件和标签/配置文件。

| Demo | 可执行文件 | 模型 |
|------|----------|------|
| YOLOv8 目标检测 | yolov8_detect/ | yolov8s.int.rknn |
| YOLOv8 姿态估计 | yolov8_pose/ | yolov8s-pose.int.rknn |
| YOLOv8 实例分割 | yolov8_seg/ | yolov8s-seg.int.rknn |
| YOLO11 目标检测 | yolo11_detect/ | yolo11.rknn |
| RetinaFace 人脸检测 | retinaface_detect/ | RetinaFace.rknn |

## 使用

文件通过 raw.githubusercontent.com 直链下载，SHA256 校验。

部署路径: /opt/aiplatform/demos/<demo-id>/

