# edist

Custom Electron binaries distributed through GitHub Releases.

Each release contains platform ZIP archives, SHASUMS256.txt, and the corresponding Electron npm package tarball.

Install Node.js first: [Download Node.js v24.22.0](https://nodejs.org/zh-cn/download/archive/v24.22.0).

To use a custom release, install its electron-VERSION.tgz asset and configure the following environment variables before npm install:

```powershell
$env:ELECTRON_MIRROR = "https://github.com/ChengHoward/edist/releases/download/"
$env:ELECTRON_CUSTOM_DIR = "v{{ version }}"
$env:ELECTRON_USE_REMOTE_CHECKSUMS = "1"
```

Example dependency for Windows x64 version 45.0.0-cloudbypass.6:

```json
{
  "devDependencies": {
    "electron": "https://github.com/ChengHoward/edist/releases/download/v45.0.0-cloudbypass.6/electron-45.0.0-cloudbypass.6.tgz"
  }
}
```

Available platforms vary by release. Refer to the assets attached to the selected release.
## 各版本定制功能

请查看 [二进制版本功能支持](./FEATURES.zh-CN.md)，按版本和平台列出定制 API、已核验行为、待验证项及 ZIP 校验值。

## 最新 Windows x64 定制版本

[v45.0.0-cloudbypass.6](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.6)：原生 Menu 与 NativePanel 样式、行内控件、原版证书查看器及多语言网站信息。保留 Agent、可信输入和虚拟光标。详见 [原生界面定制说明](./NATIVE-UI.zh-CN.md)。

安装包：[electron-45.0.0-cloudbypass.6.tgz](https://github.com/ChengHoward/edist/releases/download/v45.0.0-cloudbypass.6/electron-45.0.0-cloudbypass.6.tgz)。沿用上方镜像环境变量配置即可安装。


## v45.0.0-cloudbypass.4

[Windows x64 发布包](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.4)：原生虚拟光标、鼠标/键盘统一时间线、真实输入阻断、有限焦点等待，46 项运行验收通过。详见 [接口、性能及验收范围](./AGENT.zh-CN.md)。

ZIP SHA-256：`d61e6293bfd80f008c9ae03503b9aa90033b6d3d6b73434867a0b0651c4ddb39`。

## v45.0.0-cloudbypass.5

2026-11-04：[Windows x64 发布包](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.5)：原生网站信息与 NativePanel，42 组定向验收及 46 组既有功能回归通过，镜像安装和公开类型验证通过。接口与实测边界见 [原生浮层说明](./NATIVE-PANELS.zh-CN.md)。

ZIP SHA-256：`bf86a89be1a2578ea30fbf48d2f34082dd31938acda0cbc98b67f688c90c9ece`。

## v45.0.0-cloudbypass.6

2026-11-04：[Windows x64 发布包](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.6)：NativePanel 列表、图标按钮、文本与滚动条样式；原生 Menu 外观、行内控件、图标与勾选/单选；原版 Chromium 证书查看器和多语言网站信息。菜单与 NativePanel 不新增 WebContents；证书查看器使用一个隔离的专用 WebContents。

115 项定向验收及回归通过；从本地镜像实际安装后的运行、公开类型和 3 项菜单/证书界面检查通过。完整接口、23 张截图及实测边界见 [原生界面定制说明](./NATIVE-UI.zh-CN.md)。本版内部 Electron 版本仍为 `45.0.0-beta.5`，Linux 仍使用独立的 `.2` 版本。

ZIP SHA-256：`9b1e6a80968dac730ff60f7a1d5a0ee67dd43643357f4ff04422648705f967b8`。
