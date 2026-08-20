# AI Gateway model assets

该目录只发布 `aigw-manager` 可信 Catalog 可以下载的数据资源，不包含可执行文件、
共享库或脚本。

## 目录

```text
aigw/
├── catalogs/v1/<soc>/models.json
├── manifests/<soc>/<model_id>/<version>/manifest.yaml
└── packages/<soc>/<model_id>/<version>/<model_id>-<version>.tar.gz
```

每个 tar.gz 的根目录只包含模型包契约允许的 `manifest.yaml`、`model.rknn`、
`labels.txt` 等数据文件。Catalog 固定包的 HTTPS URL、大小和 SHA-256；
包内 Manifest 再固定每个文件的 role、大小和 SHA-256。

## 阶段 3 模型

| model_id | 资源 | 下载/安装 | 阶段 3 Pipeline |
|---|---|---|---|
| `yolov8s_dfl` | YOLOv8s RKNN2 DFL16 | 支持 | 支持 |
| `yolo11s_detect` | YOLO11s RKNN2 DFL16 | 支持 | 支持 |
| `retinaface_detect` | RetinaFace RKNN2 | 支持 | 暂不支持，等待 prior-box/landmark 后处理迁移 |
| `yolov26n` | YOLOv26n RKNN2 nms-free | 支持 | 支持 |
| `yolov26s` | YOLOv26s RKNN2 nms-free | 支持 | 支持 |

Pose 和 Seg 模型不进入本 Catalog，直到 AIResult 与 Runtime 后处理实现完成。

## RK3588 适配模型

| model_id | 版本 | 资源 | 下载/安装 | Runtime Pipeline |
|---|---|---|---|---|
| `yolov8n` | `1.0.0` | YOLOv8n RKNN2 INT8，9 输出 DFL16，COCO 80 类 | 支持 | 支持 |
| `yolov8s` | `1.0.0` | YOLOv8s RKNN2 INT8，9 输出 DFL16，COCO 80 类 | 支持 | 支持 |
| `construction_yolo11s` | `1.0.1` | YOLO11s RKNN2 INT8，9 输出 DFL16，施工与 PPE 11 类 | 支持 | 支持 |
| `construction_yolo26s` | `1.0.1` | YOLOv26s RKNN2 INT8，6 输出 NMS-free，施工与 PPE 11 类 | 支持 | 支持 |
| `retinaface_detect` | `1.0.1` | RetinaFace RKNN2，人脸框与 5 点关键点 | 支持 | 支持 |
| `yolov26s` | `1.0.1` | YOLOv26s RKNN2 INT8，6 输出 NMS-free，COCO 80 类 | 支持 | 支持 |

RK3588 Catalog 位于 `aigw/catalogs/v1/rk3588/models.json`。六份模型内嵌 target 均为 `rk3588`，
输入均为 RGB letterbox；YOLOv8/YOLO11 使用 `dfl16_regression_probabilities`，YOLOv26 使用
`yolov26_nmsfree`，RetinaFace 使用人脸框与 5 点关键点后处理。条目以 `runtime_ready: true` 发布，
表示模型输入/输出契约已有当前 Runtime 适配器，安装后可由 Web 创建 Pipeline。

本批四个新增 RK3588 模型是该平台的首次正式发布，版本统一为 `1.0.1`。`model_id` 是跨版本的永久
模型身份；后续只更新同一模型的二进制、Manifest 或标签时必须保持 `model_id` 并提升版本，不能通过
改名制造重复模型。相同 `model_id` 和版本的不同 SoC 工件分别存放在对应 `<soc>` 目录，避免审计
Manifest 互相覆盖。

## 输入颜色契约

`input.format` 表示送入 RKNN 张量的最终通道顺序，而不是 OpenCV/摄像头源格式。Catalog 维护方已
确认当前发布的 YOLOv8s、YOLO11s、YOLO26n/s 和 RetinaFace 工件都使用 RGB。首次发布的
`1.0.0` 保持原始 BGR 契约不变；修正版 `1.0.1` manifest 声明 `input.format: rgb`，Runtime
会将 BGR 源帧转换为 RGB。Catalog 指向 `1.0.1`，已安装 `1.0.0` 的设备会显示可更新。

2026-08-06 第一轮 RK3576 `bus.jpg` A/B：YOLOv8s 的 bus 置信度 BGR/RGB 为 0.8435/0.8855，
YOLO11s 为 0.9382/0.9459，两种颜色均检测到 5 个目标。2026-08-06 根据已确认输入契约修正
manifest 后，以 `1.0.1` 重新打包全部五个工件，并更新 Catalog 包 SHA、大小和版本；板端 RGB 固定图回归
覆盖 YOLOv8、YOLO26n、YOLO26s 与 YOLO26s letterbox。多人、车辆和复杂颜色场景仍需继续积累
效果 golden，但不再使用 BGR 作为当前发布包输入。

## 发布约束

- Catalog 只允许 `https://raw.githubusercontent.com/huzippm/MEASY-AIManager-assets/`
  前缀下的固定 URL。
- 不接受客户端提交的任意 URL。
- 包和包内文件都必须通过 SHA-256、大小、路径、role 和 magic 校验。
- Catalog、审计 Manifest 和模型包必须按同一 SoC 分目录；Catalog 的 `compatible.soc`、包内
  Manifest 的 `compatible.soc` 与 RKNN 内嵌 target 必须一致。
- 更新包后必须更新 Catalog 的 `catalog_version`、`generated_at`、包大小和 SHA-256。
- 已发布版本不可原地覆盖；任何 manifest、模型或标签变化都必须增加模型版本并使用新的包路径。
- 生产发布仍需通过固定版本或 commit URL冻结资产；`master` URL只用于阶段验证。
