# 各二进制版本的定制功能

核验日期：2026-11-02。范围为 edist 已发布、且本地保留的 11 个版本、16 个平台 ZIP。本文以发布包为依据，不把当前源码的全部功能直接归入历史版本。

## 如何理解核验结果

- **Windows 已确认**：实际启动该 ZIP 中的 Electron，创建隐藏窗口，检查 WebContents 上的方法是否可调用。UA / platform / hideChrome 和 Chrome 页面对象另做页面行为检查。API 存在不代表全部参数、边界条件均完成回归。
- **Linux 静态检出**：检查 ZIP 内可执行文件的功能特征，未在 Linux 上运行。字符串存在不能独立证明 API 已注册或行为正确，因此只列为待运行核验的候选能力。
- **未暴露**：Windows 实例没有对应公开方法；不以安装包的类型声明或二进制中的 CDP 同名字符串代替运行结果。
- 平台逐项记录；没有附件的平台不视为支持。自定义版本名与 ZIP 内上游 version 可能不同，不能用名称推断补丁集合。

## 功能的含义与边界

- UA 覆盖：Windows 各版本均实测 UA 字符串、`navigator.platform` 覆盖与 `hideChrome: true` 隐藏 `window.chrome`。Client Hints 字段、请求头及跨域继承未逐版本进行网络回归。
- 可信输入：`dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`cancelDragging`；源码设计采用 CDP 风格参数，无需 debugger。本次仅核验公开方法。
- DOM：`querySelectorDeep`、`getNodeBoxModel`、`clickSelector`；`querySelectorAcrossDocuments` 是另一个独立的跨文档 API，不能由前者推断支持。
- 主文档响应：`loadURLWithResponse`；请求头保序是另一项能力，通过 `session.fetch` / `net.fetch` 的 `headerOrder` 控制。本次仅对 `headerOrder` 做二进制特征核对，未逐版本抓包验证。
- 对话框：`setJsDialogHandler` 处理 alert / confirm / prompt；源码同时提供 `js-dialog` 事件和 `setJsDialogTimeout`，后两项未逐版本调用验证。
- 文件上传：以实例上的 `setFileInputFiles` 是否存在为准。旧版 Chromium 自带同名 CDP 字符串会产生误判。
- 代理认证：当前源码实现 `setProxy({ username, password })` 的 HTTP/HTTPS 与 SOCKS5 认证；这属于既有 API 的行为增强，不能用方法是否存在判断。本次未对历史包逐个进行代理握手，暂不指定首次支持版本。
- Chrome 页面对象：仅对页面主世界的 `chrome.app`、`chrome.csi`、`chrome.loadTimes` 做存在性检查，不表示所有 Chrome 扩展 API 可用。

## v41.0.0-cloudbypass.1

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v41.0.0-cloudbypass.1)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- 未暴露的公开方法：`querySelectorAcrossDocuments`、`setFileInputFiles`、`loadURLWithResponse`、`setJsDialogHandler`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：未检出该定制字段。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`150.0.7871.129`；Node.js：`24.18.0`。

### Linux x64：仅静态检查

- 检出候选特征：`dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`cancelDragging`、`setFileInputFiles`。
- 注意：`dispatch*`、`setFileInputFiles` 等也可能来自 Chromium/CDP，自身不足以证明公开 API 可用。
- 未检出特征：`querySelectorDeep`、`querySelectorAcrossDocuments`、`getNodeBoxModel`、`clickSelector`、`loadURLWithResponse`、`setJsDialogHandler`。
- UA/Client Hints、hideChrome、Chrome 页面对象、代理认证和头序：运行行为待 Linux 核验。

### 文件校验依据

- `electron-v41.0.0-cloudbypass.1-linux-x64.zip`
  - SHA-256：`ca132e327cb13dcf677130e28f5a1b18a2580f790d3ede6c4b555a325eac4b81`
- `electron-v41.0.0-cloudbypass.1-win32-x64.zip`
  - SHA-256：`9ba4750d254c4ac3fd3b5af5623919cfb6310abc70e5852472a9d7ddb0011315`

## v41.0.0-cloudbypass.2

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v41.0.0-cloudbypass.2)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- 未暴露的公开方法：`querySelectorAcrossDocuments`、`setFileInputFiles`、`loadURLWithResponse`、`setJsDialogHandler`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：未检出该定制字段。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`150.0.7871.129`；Node.js：`24.18.0`。

### Linux / macOS

本版本未发布这些平台的二进制附件。

### 文件校验依据

- `electron-v41.0.0-cloudbypass.2-win32-x64.zip`
  - SHA-256：`9ba4750d254c4ac3fd3b5af5623919cfb6310abc70e5852472a9d7ddb0011315`

## v41.0.0-cloudbypass.3

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v41.0.0-cloudbypass.3)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- 未暴露的公开方法：`querySelectorAcrossDocuments`、`setFileInputFiles`、`setJsDialogHandler`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`150.0.7871.129`；Node.js：`24.18.0`。

### Linux x64：仅静态检查

- 检出候选特征：`dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`cancelDragging`、`querySelectorDeep`、`getNodeBoxModel`、`clickSelector`、`setFileInputFiles`、`loadURLWithResponse`。
- 注意：`dispatch*`、`setFileInputFiles` 等也可能来自 Chromium/CDP，自身不足以证明公开 API 可用。
- 未检出特征：`querySelectorAcrossDocuments`、`setJsDialogHandler`。
- UA/Client Hints、hideChrome、Chrome 页面对象、代理认证和头序：运行行为待 Linux 核验。

### 文件校验依据

- `electron-v41.0.0-cloudbypass.3-linux-x64.zip`
  - SHA-256：`482c28ffa853e5f4184b4c96cf3d41bf8fa6c400e276c39608ea7c4f15ce4a95`
- `electron-v41.0.0-cloudbypass.3-win32-x64.zip`
  - SHA-256：`9a101361d70ed0a8763f74be71c5a89038cceabcbe0f0b10335fdf50b3b27dc6`

## v41.0.0-cloudbypass.4

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v41.0.0-cloudbypass.4)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- 未暴露的公开方法：`querySelectorAcrossDocuments`、`setFileInputFiles`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`150.0.7871.129`；Node.js：`24.18.0`。

### Linux x64：仅静态检查

- 检出候选特征：`dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`cancelDragging`、`querySelectorDeep`、`getNodeBoxModel`、`clickSelector`、`setFileInputFiles`、`loadURLWithResponse`、`setJsDialogHandler`。
- 注意：`dispatch*`、`setFileInputFiles` 等也可能来自 Chromium/CDP，自身不足以证明公开 API 可用。
- 未检出特征：`querySelectorAcrossDocuments`。
- UA/Client Hints、hideChrome、Chrome 页面对象、代理认证和头序：运行行为待 Linux 核验。

### 文件校验依据

- `electron-v41.0.0-cloudbypass.4-linux-x64.zip`
  - SHA-256：`3988caba91bbe17f7f73a60a2d3a805ae2c85084070bb8685f065345260063e7`
- `electron-v41.0.0-cloudbypass.4-win32-x64.zip`
  - SHA-256：`8f585ca6444641c465216154dd53a6c4d63278275e85ff728efba95372526bbe`

## v44.0.0-cloudbypass.5

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v44.0.0-cloudbypass.5)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- 未暴露的公开方法：`querySelectorAcrossDocuments`、`setFileInputFiles`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`152.0.7977.130`；Node.js：`24.21.0`。

### Linux / macOS

本版本未发布这些平台的二进制附件。

### 文件校验依据

- `electron-v44.0.0-cloudbypass.5-win32-x64.zip`
  - SHA-256：`dd84a260532f86c73de6994d6a125d272268a1984c8c8d0402b4d640d83506d4`

## v44.0.0-cloudbypass.6

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v44.0.0-cloudbypass.6)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `setFileInputFiles`：设置文件输入框的本地文件。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- 未暴露的公开方法：`querySelectorAcrossDocuments`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`152.0.7977.130`；Node.js：`24.21.0`。

### Linux x64：仅静态检查

- 检出候选特征：`dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`cancelDragging`、`querySelectorDeep`、`getNodeBoxModel`、`clickSelector`、`setFileInputFiles`、`loadURLWithResponse`、`setJsDialogHandler`。
- 注意：`dispatch*`、`setFileInputFiles` 等也可能来自 Chromium/CDP，自身不足以证明公开 API 可用。
- 未检出特征：`querySelectorAcrossDocuments`。
- UA/Client Hints、hideChrome、Chrome 页面对象、代理认证和头序：运行行为待 Linux 核验。

### 文件校验依据

- `electron-v44.0.0-cloudbypass.6-linux-x64.zip`
  - SHA-256：`25e6dc0b1d082eb1fcaeb3398a8754d914ab2a850233f3d1b9b20e9632e61e33`
- `electron-v44.0.0-cloudbypass.6-win32-x64.zip`
  - SHA-256：`fe78a8735bb33b52b6ac00b75b648576882700cc78f3c919b8fdf81cd5a98faa`

## v44.0.0-cloudbypass.7

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v44.0.0-cloudbypass.7)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `setFileInputFiles`：设置文件输入框的本地文件。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- 未暴露的公开方法：`querySelectorAcrossDocuments`。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`152.0.7977.130`；Node.js：`24.21.0`。

### Linux / macOS

本版本未发布这些平台的二进制附件。

### 文件校验依据

- `electron-v44.0.0-cloudbypass.7-win32-x64.zip`
  - SHA-256：`f3e60d7c1e66db3a77c4d309b67db6aefa4b68a0611e5285983fd851c5a33901`

## v44.0.0-cloudbypass.8

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v44.0.0-cloudbypass.8)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `querySelectorAcrossDocuments`：跨 iframe / 跨域文档查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `setFileInputFiles`：设置文件输入框的本地文件。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`25.0.0-nightly.20230223`；Chromium：`152.0.7977.130`；Node.js：`24.21.0`。

### Linux / macOS

本版本未发布这些平台的二进制附件。

### 文件校验依据

- `electron-v44.0.0-cloudbypass.8-win32-x64.zip`
  - SHA-256：`2bfa16956efd9c7eff69480d8ffb77b8962ee52223d97f4072c836ef6f00b1d5`

## v45.0.0-cloudbypass.1

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.1)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `querySelectorAcrossDocuments`：跨 iframe / 跨域文档查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `setFileInputFiles`：设置文件输入框的本地文件。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- Chrome 页面对象：未暴露 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`45.0.0-beta.5`；Chromium：`156.0.8078.36`；Node.js：`24.22.0`。

### Linux / macOS

本版本未发布这些平台的二进制附件。

### 文件校验依据

- `electron-v45.0.0-cloudbypass.1-win32-x64.zip`
  - SHA-256：`f84d6956067fe80d87d05d3300f42ceceb2e694ce22229a214523ae49ae57ba5`

## v45.0.0-cloudbypass.2

[下载此版本](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.2)

### Windows x64：已运行核验

- UA、navigator.platform 覆盖及隐藏 window.chrome：页面行为检查通过。
- `dispatchMouseEvent`：鼠标输入（CDP 风格）。
- `dispatchKeyEvent`：键盘输入（CDP 风格）。
- `dispatchTouchEvent`：触摸输入（CDP 风格）。
- `cancelDragging`：取消拖拽。
- `querySelectorDeep`：穿 Shadow DOM 查找。
- `querySelectorAcrossDocuments`：跨 iframe / 跨域文档查找。
- `getNodeBoxModel`：节点盒模型。
- `clickSelector`：按选择器点击。
- `setFileInputFiles`：设置文件输入框的本地文件。
- `loadURLWithResponse`：指定 URL 的自定义主文档响应。
- `setJsDialogHandler`：处理 alert / confirm / prompt。
- Chrome 页面对象：存在 `chrome.app`、`chrome.csi`、`chrome.loadTimes`。
- `headerOrder`：静态检出；请求头实际顺序待网络核验。
- Client Hints 完整行为、HTTP/SOCKS5 代理认证：待逐版本网络核验。
- 包内上游版本：`45.0.0-beta.5`；Chromium：`156.0.8078.36`；Node.js：`24.22.0`。

### Linux x64：仅静态检查

- 检出候选特征：`dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`cancelDragging`、`querySelectorDeep`、`querySelectorAcrossDocuments`、`getNodeBoxModel`、`clickSelector`、`setFileInputFiles`、`loadURLWithResponse`、`setJsDialogHandler`。
- 注意：`dispatch*`、`setFileInputFiles` 等也可能来自 Chromium/CDP，自身不足以证明公开 API 可用。
- UA/Client Hints、hideChrome、Chrome 页面对象、代理认证和头序：运行行为待 Linux 核验。

### 文件校验依据

- `electron-v45.0.0-cloudbypass.2-linux-x64.zip`
  - SHA-256：`cecd8d69608aebe59633de9c36615456af17e8eb7cecff59b207fd7385d1a722`
- `electron-v45.0.0-cloudbypass.2-win32-x64.zip`
  - SHA-256：`ac9bb5ba16b1f04fad8b165faa401458926aeb5c5423075afe0009ce37957cb5`

## 源码与发布包的区别

当前源码还有未提交的 `setFileInputFiles` 绝对路径校验和文件读取授权修复，不能据此认定以上任何发布包已包含该修复。

旧说明中“仅隐藏 window.chrome、不提供 chrome.app/csi/loadTimes”的描述，对 45.0.0-cloudbypass.2 的 Windows 包已经过时；此前版本本次仍未观察到这些页面对象。Linux 需另行确认。

## 后续发布要求

每个平台发布时保存构建提交、工作区补丁摘要、平台架构、上游版本与 ZIP SHA-256，并用实际产物回归 UA/CH、输入、DOM、文件上传、对话框、头序和代理认证。不能仅从提交日期或版本递增推断功能继承。

## v45.0.0-cloudbypass.3

Windows x64 定制 Electron：无需 CDP 的 AI Agent 自动化。

- 新增 getAgentSnapshot、findAgentElements；增强 dispatchMouseEvent、dispatchKeyEvent、dispatchTouchEvent、clickSelector、querySelectorAcrossDocuments，共七个公开方法。
- 原生读取主文档、同源 iframe、跨域 OOPIF、开放及封闭 Shadow DOM 的名称、文本、当前值、控件状态和错误信息；快照明确报告部分覆盖及截断原因。
- 隐藏或未聚焦窗口支持可信鼠标、键盘及触摸输入，不激活系统窗口。派发结果区分 dispatched / failed；效果须重新读取验证。
- Windows 增量编译，11 项验收全部通过，系统前台激活 0 次。Linux 未编译或验证，此版本没有 Linux 包。
- 原生弹出控件、离屏渲染等限制及完整接口说明见 [Agent 自动化](https://github.com/ChengHoward/edist/blob/main/AGENT.zh-CN.md)。
- 自定义分发版本为 45.0.0-cloudbypass.3；二进制内部上游版本仍为 45.0.0-beta.5。

附件包括平台 ZIP、SHA-256、npm 安装包、验收结果及构建记录。

Windows x64 ZIP SHA-256：`d5168ab311800e0feb739c1bd7bea50cea2ddb601975152de9f95ff1dd9ec82f`。


## v45.0.0-cloudbypass.4

[Windows x64 发布包](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.4)：原生虚拟光标、鼠标/键盘统一时间线、真实输入阻断、有限焦点等待，46 项运行验收通过。详见 [接口、性能及验收范围](./AGENT.zh-CN.md)。

ZIP SHA-256：`d61e6293bfd80f008c9ae03503b9aa90033b6d3d6b73434867a0b0651c4ddb39`。
