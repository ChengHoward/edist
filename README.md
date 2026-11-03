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

Example dependency for version 44.0.0-cloudbypass.6:

```json
{
  "devDependencies": {
    "electron": "https://github.com/ChengHoward/edist/releases/download/v44.0.0-cloudbypass.6/electron-44.0.0-cloudbypass.6.tgz"
  }
}
```

Available platforms vary by release. Refer to the assets attached to the selected release.
## 各版本定制功能

请查看 [二进制版本功能支持](./FEATURES.zh-CN.md)，按版本和平台列出定制 API、已核验行为、待验证项及 ZIP 校验值。

## 最新 Agent 自动化版本

[v45.0.0-cloudbypass.3](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.3)（Windows x64）：无需 CDP，支持跨域 iframe、封闭 Shadow DOM 和隐藏窗口可信输入。详见 [接口说明](./AGENT.zh-CN.md)。

安装包：[electron-45.0.0-cloudbypass.3.tgz](https://github.com/ChengHoward/edist/releases/download/v45.0.0-cloudbypass.3/electron-45.0.0-cloudbypass.3.tgz)。沿用上方镜像环境变量配置即可安装。


## v45.0.0-cloudbypass.4

[Windows x64 发布包](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.4)：原生虚拟光标、鼠标/键盘统一时间线、真实输入阻断、有限焦点等待，46 项运行验收通过。详见 [接口、性能及验收范围](./AGENT.zh-CN.md)。

ZIP SHA-256：`d61e6293bfd80f008c9ae03503b9aa90033b6d3d6b73434867a0b0651c4ddb39`。
