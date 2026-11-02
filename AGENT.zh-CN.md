# 无需 CDP 的 Agent 自动化

本说明对应 v45.0.0-cloudbypass.3 的 Windows x64 发布包。Windows 11 项验收通过，系统前台激活 0 次；此前历史版本不据此认定支持，Linux 未验证。

## API 范围

新增 `webContents.getAgentSnapshot(options)` 和 `findAgentElements(query, options)`；增强 `dispatchMouseEvent`、`dispatchKeyEvent`、`dispatchTouchEvent`、`clickSelector`、`querySelectorAcrossDocuments`，共涉及七个公开方法。内部通信方法不属于公开 API。

读取通过浏览器进程枚举当前页面的框架，再经原生 Mojo 接口进入每个框架的 Blink DOM 和可访问性对象。支持主文档、同源 iframe、跨域 OOPIF，以及开放和封闭的作者 Shadow DOM。不连接 debugger、不执行页面 JavaScript 来实现自动化，不要求远程调试端口。

## 页面快照

```js
const snapshot = await win.webContents.getAgentSnapshot({
  maxNodes: 1000,
  maxFrames: 100,
  timeoutMs: 5000,
  pierce: true
});
```

返回 `elements`、`frames`、`complete`、`truncated` 和 `truncationReasons`。元素是扁平列表，携带框架与文档身份及 parentBackendNodeId（含 Shadow host 边界）；不能按列表位置推断 DOM 父子关系。

- 元素信息：`tagName`、`type`、Chromium AX `role`、可访问名称 `name`、直接文本 `text`、`labelText`、`description`、`descriptionText`、`errorText`、`placeholder`。
- 当前表单状态（含可编辑文本区域的 value）：`value`、`disabled`、`readOnly`、`checked`、`selected`、`expanded`、`focused`、`invalid`。AX 状态采用 Chromium 的枚举字符串，例如 `checked: 'true'`；属性缺失不等于 false。
- 下拉框直接返回 options 列表（包含 label、text、value、selected、disabled），option 也分别出现在快照中；单个下拉框超过 1000 项时明确报告 MAX_OPTIONS 截断。错误提示通过 `aria-errormessage`、说明通过 `aria-describedby` 关联；alert/status 等状态文本也作为页面元素读取。
- 密码和文件输入框的 value 默认置空，并标记 `valueRedacted`。不把脚本、样式源码混入祖先元素文本。
- `bounds` 为 Blink 返回的局部布局坐标，可能包含设备缩放；带 target 的输入会在原生设备缩放信息的基础上转换为 RenderWidget 输入坐标；`visible` 是布局/样式可见性，不承诺元素没有被其他元素遮挡。真正执行操作时会重新解析和检查目标。

`target` 包含 webContentsId、processId、routingId、documentId、backendNodeId。它是当前文档内的定位引用，不是可跨导航持久保存的业务 ID。导航、移除节点或框架销毁后，应重新获取快照。

## 查找与覆盖状态

```js
const found = await win.webContents.findAgentElements({
  name: 'Customer name',
  disabled: false
});
```

支持 role、name、text、label、disabled、checked、selected 和 selector；字符串 name/text/label 为区分大小写的包含匹配。selector 是每个文档/Shadow tree 范围内的 CSS 选择器。返回完整快照相同的覆盖字段，elements 仅保留匹配项；没有匹配项且 `complete: false` 时，不能推断整个页面不存在目标。

现有跨文档选择器路径仍可复用：

```js
const result = await win.webContents.querySelectorAcrossDocuments(
  'iframe >>> input',
  { agentInfo: true }
);
```

该模式保留旧接口的首个匹配语义，再从快照获取对应元素的语义信息。未指定 agentInfo 时保留原有返回值。

每个框架有 covered、partial、failed 或 skipped 状态及原因。达到节点、框架、时间、遍历深度、访问节点数或输出字节上限时显式报告截断。单框架目前有 100ms DOM 遍历预算、128 层深度、20000 个访问节点、2MiB 元素输出上限。全局默认最多 1000 个输出元素、100 个框架、5000ms；可配置的硬上限分别为 5000、1000、30000ms。限制不代表页面的真实大小；AX 初始化等原生工作不能被 JavaScript 超时抢占。

跨进程读取不是整个页面的原子事务。页面在读取过程中仍可能变化，输入时必须重新校验 target。

## 后台可信输入

```js
const field = found.elements[0];
if (!field) throw new Error('没有找到输入框，请检查覆盖状态');

const dispatched = await win.webContents.dispatchKeyEvent({
  target: field.target,
  type: 'char',
  text: 'Alice'
});

// 必须重新读取，确认值是否实际改变。
const after = await win.webContents.findAgentElements({ name: 'Customer name' });
```

`target` 模式将事件路由到目标框架的 RenderWidget，不依赖系统前台窗口或 compositor 的跨进程命中路由。键盘操作先设置目标的页面内部焦点；不会调用 BrowserWindow.show/focus，也不会通过临时激活窗口完成输入。

鼠标的 x/y、触摸点的 x/y 在 target 模式下表示相对目标元素的 CSS 像素偏移；鼠标省略坐标时使用元素中心。多点触摸应属于同一目标框架，touchStart/touchMove 必须提供触摸点；touchEnd/touchCancel 可使用空数组。触摸序列必须持续使用同一 target。

```js
const buttons = await win.webContents.findAgentElements({ name: 'Submit form' });
await win.webContents.clickSelector('', { target: buttons.elements[0].target });
```

输入期间暂时关闭后台节流，并获取保持隐藏的渲染保活作用域。它不生成截图文件、不显示或激活窗口；输入队列屏障不等待屏幕实际呈现，完成后释放作用域并恢复设置。鼠标按下/释放、键盘按下/释放、触摸开始/结束之间保持序列状态；不完整序列有恢复节流的超时保护。隐藏和未聚焦能力以带 target 的调用为准，旧坐标接口保持兼容。

返回：

- `status: 'dispatched'`：事件已交给原生输入通道，**不代表页面操作成功**。
- `status: 'failed'`：解析或派发前失败，包含 target-resolution、frame-routing 或 event-dispatch 阶段、原因及阶段记录。
- 已派发但处理屏障超时/失败时，仍返回 dispatched，并附 `deliveryWarning`，不误报为“未发送”。

页面调用 preventDefault、收到事件后不改变状态等情形，由 Agent 重新读取快照判断。没有“页面未消费”这个公开结果状态。

失效文档/节点、失效框架、禁用元素、不可见或被遮挡的目标、无法聚焦的键盘目标、触摸目标不一致等应提供明确原因。显式启用 offscreen 渲染的 WebContents 暂不支持带 target 的输入，会返回 OFFSCREEN_AGENT_INPUT_UNSUPPORTED；它与普通 BrowserWindow 的隐藏状态不同。系统文件选择器、系统权限窗口不属于此输入 API；可信事件也不保证解除网站或浏览器的所有用户激活限制。

## 验收与构建

验收入口：`test/agent-automation.cjs`。测试使用两个站点的 iframe 和封闭 Shadow DOM 表单，分别检查快照、输入值/控件状态、可信事件、后台输入、前台窗口保持、截断及失效引用。测试夹具允许用页面脚本记录/断言结果；功能实现本身不依赖页面脚本或 CDP。

在现有 `out/Release` 上增量编译，保留缓存：

```powershell
cd D:\electron\main\src
ninja -C out/Release electron:electron_dist_zip
```

验证通过后再为新定制版本命名、发布。此前版本功能文档不应被改写成已经包含本次功能。

### 原生控件限制

为避免系统弹窗抢占焦点，带 target 的鼠标/触摸操作不打开原生 select 菜单；select 可用不带 Alt 的 ArrowUp/ArrowDown/Home/End/Tab 键操作，并重新读取 value/options 确认结果。文件、颜色、日期和时间选择器返回 TARGET_NATIVE_POPUP_UNSUPPORTED；文件输入可使用现有 setFileInputFiles。网页脚本自行创建的窗口、对话框及权限 UI 仍需由宿主的窗口/对话框策略接管。
