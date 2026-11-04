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

Example dependency for Windows x64 version 45.0.0-cloudbypass.7:

```json
{
  "devDependencies": {
    "electron": "https://github.com/ChengHoward/edist/releases/download/v45.0.0-cloudbypass.7/electron-45.0.0-cloudbypass.7.tgz"
  }
}
```

Available platforms vary by release. Refer to the assets attached to the selected release.
## 各版本定制功能

请查看 [二进制版本功能支持](./FEATURES.zh-CN.md)，按版本和平台列出定制 API、已核验行为、待验证项及 ZIP 校验值。

## 最新 Windows x64 定制版本

[v45.0.0-cloudbypass.7](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.7)：Textarea、系统亚克力、细滚动条与 `overlay`／`gutter`，并包含同版本 PageInfo／图标按钮焦点框修订。运行 138/138、参数 126/126、镜像安装焦点框 UI 3/3 通过。接口与前后截图见 [NativePanel 定制说明](./NATIVE-PANEL-CUSTOMIZATION.zh-CN.md)。

安装包：[electron-45.0.0-cloudbypass.7.tgz](https://github.com/ChengHoward/edist/releases/download/v45.0.0-cloudbypass.7/electron-45.0.0-cloudbypass.7.tgz)。版本号仍为 `.7`；已安装项目需显式重新下载，并按新摘要刷新依赖锁定。修订 ZIP SHA-256：`3f2f0595baaab729f0a0cf7fe04cf356753078fab95a173663fd2242aa94f651`。

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

## v45.0.0-cloudbypass.7

2026-11-04：[Windows x64 发布包](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.7)，同版本焦点框修订。修复 PageInfo 整行操作和 NativePanel 图标按钮的焦点轮廓裁剪；继续使用原生 FocusRing，向内收 1 DIP、线宽 2 DIP、圆角跟随 `style.radius`，保留鼠标、Tab、方向键及 Enter／Space 激活。

本版包含 NativePanel 多行 Textarea、系统亚克力 `backgroundMaterial: 'acrylic'`、列表／外层溢出区／编辑区统一细圆头滚动条，以及 `scrollbarPlacement: 'overlay' | 'gutter'`。默认预留槽位，编辑器滚动条独立于正文内边距。Menu 本轮不增加系统材质。

修订构建的运行验收与回归 **138/138**、参数校验 **126/126** 通过；从本地镜像实际安装后，运行、公开类型及 **3/3** 项焦点框界面检查通过。安装 UI 复测采用 `focus` 范围；材质和滚动条界面结论来自完整构建验收。同一夹具已复现首发焦点检查失败并验证修订通过。接口、前后截图和系统边界见 [NativePanel 多行编辑、系统材质与焦点修订](./NATIVE-PANEL-CUSTOMIZATION.zh-CN.md)。

版本号仍为 `45.0.0-cloudbypass.7`；内部上游版本为 `45.0.0-beta.5`。已安装相同版本的项目需要显式重新下载，并在保留旧锁定摘要时刷新对应依赖锁文件。启用远程校验和且安装流程运行时，`@electron/get` 会按新的远程摘要校验缓存，不符则重新下载。请用下面的 SHA-256 区分构建。本次仅提供 Windows x64，Linux 仍为独立 `.2`。

- 修订可执行文件 SHA-256：`c737bce9ba686cd08a63511a8c9b8c80edc001209895867b2c6cc0847c70518a`。
- 修订 ZIP SHA-256：`3f2f0595baaab729f0a0cf7fe04cf356753078fab95a173663fd2242aa94f651`。
- 修订 npm 包 SHA-256：`6a943837891218f2e40fca0d14f0f7e84fd584d968b5053ecad3b44458fa5d57`。
- 修订源码：[对应提交](https://github.com/ChengHoward/electron/commit/cf7d6be672a56ebffe5bf33c03a61302085c22c9)。

首发 `.7` 的 135/135 运行结果、可执行文件 `bbd02f3fed48537a5a1b7f66d313a7a73a64cfb20e80f74aea35898e4809b468` 和 ZIP `89e2159eafe721cc14e99b3392ae2fa6a417e1002cdb19791f62a1f9cbae6402` 属于历史构建，不作为本次修订验收。原始文档与证据保留在 [首发源码提交](https://github.com/ChengHoward/electron/commit/782f7d28c0e92aa670287a1592f51bb4868f9518)；旧版本记录保持原样。

