# CLAUDE.md

## 项目概述

MyCollection — 个人知识收藏工具。用于收集和检索浏览中遇到的 AI 工具、技巧、链接、指令等碎片信息。

单文件 HTML 应用，无构建工具、无依赖。

## 技术栈

- 纯 HTML + CSS + JavaScript（内嵌在 index.html 中）
- File System Access API（`showOpenFilePicker`）持久化数据到本地 JSON 文件（可选）
- IndexedDB 保存文件句柄 + GitHub Gist 配置（跨会话复用）
- localStorage 存储主题偏好和上次同步时间（`mycollection_last_sync`）
- Google Fonts：Source Serif 4（标题）+ DM Sans（正文）+ JetBrains Mono（代码/快捷键）

## 数据存储

- 用户通过 `showOpenFilePicker` 选择一个 JSON 文件作为数据源（可选）
- 文件句柄存入 IndexedDB（`MyCollectionDB`），重启后自动恢复
- JSON 结构：`{ entries: [...], presets: [...] }`
- 每条 entry：`{ id, title, content, notes, tags[], image, pinned, createdAt, updatedAt }`
- 支持从旧版 localStorage 自动迁移（`migrateFromLocal()`）

### 无本地文件的云端模式

当 `fileHandle` 为 null（未选择本地文件或权限未授权）：
- `saveData()` 跳过文件写入，但仍设置 `_dirty=true` 并触发脏状态 UI
- 头部文件标签显示「云端模式」（灰色，非报错），不再显示红色「选择数据文件」
- 数据完全依赖 Gist 同步持久化：启动拉取 → 内存编辑 → 「↑ 点击同步」推送
- 适合多设备场景：无需绑定本地文件，Gist 即主存储

## 云同步（GitHub Gist）

- 同步后端：GitHub Gist（私有），通过 GitHub REST API 读写
- 配置项：`gistConfig { token, gistId }`，存入 IndexedDB 的 `gistConfig` key
- 首次推送时自动创建 Gist，Gist ID 写回 IndexedDB 并刷新文件标签
- 合并策略：以 `updatedAt` 为准取较新版本，两端独立新增的条目都保留

### 脏状态追踪

- `_dirty` 布尔标志：`saveData()` 写入后设为 `true`，`markSynced()` 清除
- `saveData(skipDirty=true)`：内部调用（syncFromCloud 拉取后写文件）不触发脏标记
- 有未同步更改时头部显示橙色胶囊「↑ 点击同步」，可点击直接触发上传
- 已同步时显示灰色文字「↑↓ X分前」，每分钟自动刷新（`setInterval`）
- 关闭/刷新页面时若有脏状态 + Gist 已配置，触发 `beforeunload` 警告

### 自动同步

- 每次 `saveData()` 后设定 90 秒防抖定时器，到期静默推送（`syncToCloud(quiet=true)`）
- 静默推送失败后 60 秒自动重试一次
- 「从云端同步 ↓」手动触发时：若有脏状态先弹确认框提示合并规则

## 设计规范

- 对齐 Claude 官方设计语言：暖灰配色 + clay 陶土色品牌色 (#d97757)
- 亮色背景 `#faf9f5`，暗色背景 `#141413`
- CSS 自定义属性控制主题，`.dark` class 切换暗色模式
- 所有视觉细节（阴影、圆角、间距）通过 CSS 变量统一管理
- 容器最大宽度 1120px；布局为 `.app-body` flex 行：左侧 `.sidebar`（188px sticky）+ `.main-area`（flex:1）
- 响应式断点 860px：边栏折叠为顶部横排，网格视图降为单列
- **无原生浏览器弹窗**：所有 `confirm/alert/prompt` 均替换为自定义 Promise 弹窗（`showConfirm`）或 toast（`showToast`）

## 文件说明

- `index.html` — 唯一的应用文件
- `mycollection.json` — 示例数据文件（用户自己的数据在别处）

## 功能列表

- 快速录入（Quick Capture）：顶部 textarea，Enter 直接保存，Shift+Enter 换行
- 搜索：全文匹配标题、内容、标签
- 左侧边栏：快速筛选（全部/置顶/含图片/最近7天）+ 标签竖排列表（带计数）+ 底部统计
- 标签筛选：点击 tag chip 过滤，再次点击取消；与快速筛选可叠加为 AND 条件
- 网格/列表视图：stats bar 右侧 ⊞/☰ 切换；网格为 2 列，隐藏备注，图片限高 120px
- 置顶：`★` 按钮，置顶条目始终排在最前
- 图片：支持点击选择、拖拽、Ctrl+V 粘贴；存储前自动压缩到最长边 900px（JPEG 0.72）
- URL 标题抓取：粘贴链接后可点击「抓取标题」，通过 allorigins.win 代理获取页面 title
- Markdown 渲染：内容区支持 ` ``` `代码块、`` ` ``行内代码、`**粗体**`、`*斜体*`、`- 列表`
- 批量操作：点击「选择」进入批量模式，多选后可批量打标签（3-state 编辑器）或删除；未选时按钮 disabled
- 批量标签编辑器（`showTagEditor`）：绿色✓=将添加，红色删除线=将移除，虚线=部分记录有，实线accent=全部已有
- 导入/导出：菜单中导出为 JSON，支持合并导入（去重逻辑基于 id）
- 暗色模式：自动跟随系统，也可手动切换，偏好存 localStorage
- 云同步脏状态：有未同步更改时头部出现「↑ 点击同步」胶囊，点击立即上传

## 快捷键

- `Ctrl+N` 新增收藏
- `/` 聚焦搜索框
- `Esc` 关闭弹窗或取消聚焦
