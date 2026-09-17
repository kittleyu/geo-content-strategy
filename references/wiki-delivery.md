# 交付经验：把 GEO 文章归入飞书知识库（wiki）的操作细节

GEO 项目产出的文章经常需要归入客户的飞书知识库目录。以下经验来自 2026-09 实测（目标空间 CLI 无写权限、前端创建按钮失灵的环境）。

## 1. 权限判断顺序

- **优先试 CLI**：`lark-cli wiki +node-get --node-token '<wiki_url>' --as user --format json` 解析目标节点（拿 space_id / parent_node_token / obj_token）。
- 写操作（+node-create / +move / +member-list）报 131006 permission_denied = CLI 身份对该空间只有读权限 → **换浏览器身份**（浏览器登录的账号可能对目标空间有编辑权限）。
- 不要反复重试相同参数，报错本身是事实。

## 2. 网页版创建子页面（关键：按钮归属）

- **树顶部「添加子页面」按钮属于空间根目录**——点它创建的是空间根节点，不是选中节点的子页面（实测踩坑：3 篇全建到空间根）。
- **正确入口：目标节点行 hover → 行尾出现 `file-create-btn`（+）→ 点击 → 菜单「文档」→ 弹窗「新建空白文档」**。只有节点行的 + 才创建到该节点下。

## 3. 虚拟滚动树的坑

- 飞书知识库左侧树是**虚拟滚动**：只有视口附近的节点在 DOM 里，其余查不到坐标、点不到。
- 相距较远的两个节点（如目标父节点与待移动文章）**无法同时可见** → 拖拽移动不可行。
- 移动不了就**在目标节点行 + 新建、再删旧的**（或让用户在客户端拖拽）。

## 4. 删除误建节点

- 右键节点 → 菜单「删除」→ 弹窗（「是否删除：X？」）→ 若有「删除范围」单选（仅删除此页面 / 删除此页面和它包含的所有子页面），**必须先点单选，删除按钮才从禁用变可点**。
- 用 CDP 时坐标一律 `getBoundingClientRect()` 取真实 DOM 坐标；**截图标注坐标（千分比）与 DOM 坐标不一致**，直接按截图换算会点错。

## 5. 内容填充（CDP 自动化流程）

1. 点击标题区（`[contenteditable]` / `.zone-container.text-editor` 真实坐标）
2. `Input.dispatchKeyEvent` 逐字输入标题 → Enter
3. `navigator.clipboard.writeText(正文)`（需 awaitPromise:true）→ Ctrl+V 粘贴
4. 点页面空白处失焦保存
5. 回读 `document.body.innerText.length` 与本地草稿长度核对

## 6. 前端按钮失灵的绕过（QuotaExceededError）

- 现象：普通 click 点击「添加子页面>文档」无响应，控制台刷 QuotaExceededError（docx_app_spa 的存储异常），localStorage 写入正常、磁盘有空间。
- 绕过：CDP `Input.dispatchMouseEvent`（mouseMoved→mousePressed→mouseReleased）按真实坐标点击可正常弹出菜单/创建。
- CDP WebSocket 连接必须 `suppress_origin=True`，否则 403。

## 7. 交付校验

- 创建后：左侧树确认节点挂在正确父节点下（截图核对层级与缩进）。
- 内容完整性：打开文档看标题、正文尾部（如「常见问题与局限声明…逐篇改写」）是否完整。
- 误建节点清理后再交付，交付说明里交代清理结果。
