# MEASY AIManager Demo Assets

AIManager 的静态模型目录与最新 Demo 资源。开发板不调用动态目录服务，而是根据运行时检测到的 SoC 和 Kernel 前三段直接读取：

```text
catalogs/<soc>/<kernel-x.y.z>/demos.json
```

当前支持的 RK3576 Kernel 索引：

```text
catalogs/rk3576/6.1.75/demos.json
catalogs/rk3576/6.1.118/demos.json
```

## 目录结构

```text
catalogs/
└── rk3576/
    ├── 6.1.75/demos.json
    └── 6.1.118/demos.json

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
- 每个已验证支持的 Kernel 前三段都必须提供精确 Catalog；兼容内核可复用同一份资源清单和 artifact。
- 更新共享资源时必须同步更新所有引用该资源的 Kernel Catalog，避免支持目录之间产生版本差异。
- Catalog 中的 `source` 是相对本仓库根目录的路径。
- 每个文件必须填写准确的 `size`、`sha256` 和 `executable`。
- 替换资源后同步更新 Catalog 的文件信息与 `catalogVersion`。
- 板端通过文件清单摘要判断是否需要更新；不会定时轮询或自动下载 Demo。
- 没有真实可执行文件和模型时，不发布占位 variant。

默认发布地址：

```text
https://raw.githubusercontent.com/huzippm/MEASY-AIManager-assets/master/
```
