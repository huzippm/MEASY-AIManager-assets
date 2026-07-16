# MEASY AIManager Demo Assets

AIManager 的静态模型目录与最新 Demo 资源。开发板不调用动态目录服务，而是根据运行时检测到的 SoC 和 Kernel 前三段直接读取：

```text
catalogs/<soc>/<kernel-x.y.z>/demos.json
```

RK3576 Linux 6.1.75 对应：

```text
catalogs/rk3576/6.1.75/demos.json
```

## 目录结构

```text
catalogs/
└── rk3576/6.1.75/demos.json

artifacts/
└── rk3576/
    └── <demo-id>/
        └── <variant-id>/
            ├── executable
            ├── weights/
            └── labels/config
```

一个逻辑模型只有一个 Demo ID；不同运行后端使用同一 Demo 下的独立 variant。当前已有资源均为 `rk3576-rknpu2`。

## 发布规则

- 远端当前工作树只保留每个 variant 的最新资源，不维护历史资源目录。
- Catalog 中的 `source` 是相对本仓库根目录的路径。
- 每个文件必须填写准确的 `size`、`sha256` 和 `executable`。
- 替换资源后同步更新 Catalog 的文件信息与 `catalogVersion`。
- 板端通过文件清单摘要判断是否需要更新；不会定时轮询或自动下载 Demo。
- 没有真实可执行文件和模型时，不发布占位 variant。

默认发布地址：

```text
https://raw.githubusercontent.com/huzippm/MEASY-AIManager-assets/master/
```
