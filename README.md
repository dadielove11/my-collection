<div align="center">

# MyCollection

**个人知识收藏夹 — 收集一切值得留下的东西**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-在线体验-d97757?style=for-the-badge)](https://dadielove11.github.io/my-collection/)
[![Single File](https://img.shields.io/badge/Single%20File-零依赖-5e5d59?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/License-MIT-87867f?style=for-the-badge)](#)

<br>

> 粘贴链接、指令、技巧、截图……一个 `index.html`，无需安装，打开即用。  
> 通过 GitHub Gist 同步，手机电脑随时随地访问。

</div>

---

## 快速开始

### 第一步：打开应用

直接访问 **[https://dadielove11.github.io/my-collection/](https://dadielove11.github.io/my-collection/)**，或将 `index.html` 下载到本地用浏览器打开。

### 第二步：配置云同步（强烈建议）

没有同步，数据只存在当前浏览器。**手机使用必须配置。**

1. 前往 [GitHub Token 生成页](https://github.com/settings/tokens/new)，勾选 **`gist`** 权限，生成 token
2. 点击右上角 **`⋯`** 菜单 → **同步设置**，填入 token
3. 点击菜单 → **同步到云端 ↑**，首次会自动创建私有 Gist
4. 在其他设备：填入同一 token + Gist ID，点击**从云端同步 ↓**

> Gist ID 推送后在「同步设置」里查看，也是 `gist.github.com` 页面 URL 末尾的字符串。

### 第三步：开始收藏

- 顶部输入框快速粘贴，`Enter` 保存
- 或点击右上角 **`+ 新增`** 填写完整信息（标题、内容、备注、标签、图片、附件）

---

## 功能一览

| | 功能 | 说明 |
|---|---|---|
| ⚡ | **快速录入** | 顶部输入框，`Enter` 即保存，`Shift+Enter` 换行 |
| 🔍 | **全文搜索** | 实时匹配标题、内容、标签 |
| 🏷️ | **标签系统** | 预设 + 自定义标签，侧边栏一键筛选 |
| 📌 | **置顶** | 重要条目固定在最顶部 |
| 🖼️ | **图片** | 拖拽 / 粘贴上传，自动压缩，支持 `Ctrl+V` |
| 📎 | **文件附件** | 附加任意文件（≤ 500 KB），点击直接下载 |
| 🔗 | **标题抓取** | 粘贴链接后一键获取页面标题 |
| ✍️ | **Markdown** | 支持代码块、粗体、斜体、列表、行内代码 |
| ☁️ | **云同步** | GitHub Gist 私有存储，多端实时同步 |
| 📦 | **批量操作** | 多选后批量打标签或删除 |
| 🌙 | **暗色模式** | 自动跟随系统，支持手动切换 |
| 💾 | **导入/导出** | JSON 格式备份，支持合并导入 |

---

## 快捷键

| 按键 | 操作 |
|------|------|
| `Ctrl+N` | 新增收藏 |
| `/` | 聚焦搜索框 |
| `Enter` | 快速保存（在顶部输入框中） |
| `Esc` | 关闭弹窗 |

---

## 多设备使用说明

| 场景 | 方法 |
|------|------|
| 电脑 A → 电脑 B | 两台电脑填同一 Token + Gist ID |
| 手机使用 | 手机浏览器打开网址，配置同一 Gist，不需要本地文件 |
| 分享给他人 | 对方打开网址后配置自己的 Gist，数据互相独立 |

**同步行为：**
- 启动时自动静默拉取
- 有改动时头部显示橙色 **↑ 点击同步** 胶囊，点击立即推送
- 90 秒内无新改动自动静默推送
- 合并规则：以修改时间较新的版本为准，两端独立新增的条目都保留

---

## 数据存储

两种模式可混用：

- **本地文件模式**：点击头部文件标签，选择本地 `.json` 文件，关闭浏览器后自动恢复
- **云端模式**：配置 Gist 后无需本地文件，数据完全存在云端，适合手机或多设备场景

---

<div align="center">

纯 HTML + CSS + JavaScript · 单文件零依赖 · [Source Serif 4](https://fonts.google.com/specimen/Source+Serif+4) / [DM Sans](https://fonts.google.com/specimen/DM+Sans) / [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono)

</div>
