# AI Gateway model assets

该目录只发布 `aigw-manager` 可信 Catalog 可以下载的数据资源，不包含可执行文件、
共享库或脚本。

## 目录

```text
aigw/
├── catalogs/v1/<soc>/models.json
├── manifests/<model_id>/<version>/manifest.yaml
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

## 发布约束

- Catalog 只允许 `https://raw.githubusercontent.com/huzippm/MEASY-AIManager-assets/`
  前缀下的固定 URL。
- 不接受客户端提交的任意 URL。
- 包和包内文件都必须通过 SHA-256、大小、路径、role 和 magic 校验。
- 更新包后必须更新 Catalog 的 `catalog_version`、`generated_at`、包大小和 SHA-256。
- 生产发布仍需通过固定版本或 commit URL冻结资产；`master` URL只用于阶段验证。
