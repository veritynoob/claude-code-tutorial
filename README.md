# VS Code + Claude Code + DeepSeek-V4-Pro 配置教程

## 1. 安装 VS Code

官网下载安装：https://code.visualstudio.com/

## 2. 安装 Claude Code 扩展

1. 打开 VS Code
2. 按 `Ctrl+Shift+X` 打开扩展面板
3. 搜索 **Claude Code**
4. 安装 Anthropic 官方发布的 Claude Code 扩展

## 3. 配置 DeepSeek-V4-Pro 作为后端模型

### 3.1 获取 DeepSeek API Key

1. 访问 [DeepSeek 开放平台](https://platform.deepseek.com/)
2. 注册并登录
3. 在 API Keys 页面创建新的 API Key 并复制

### 3.2 配置 settings.json

按以下路径打开配置：

**File** → **Preferences** → **Settings** → **Extensions** → **Claude Code** → **Environment Variables** → **Edit in settings.json**

（或 `Ctrl+,` 打开设置，搜索 `claude code environment`，点击 "Edit in settings.json"）

配置格式如下（**注意：值是数组，不是对象**）：

```json
{
    "claudeCode.environmentVariables": [
        {
            "name": "ANTHROPIC_BASE_URL",
            "value": "https://api.deepseek.com/anthropic"
        },
        {
            "name": "ANTHROPIC_AUTH_TOKEN",
            "value": "你的DeepSeek-API-Key"
        },
        {
            "name": "ANTHROPIC_MODEL",
            "value": "deepseek-v4-pro[1m]"
        },
        {
            "name": "ANTHROPIC_DEFAULT_SONNET_MODEL",
            "value": "deepseek-v4-pro[1m]"
        },
        {
            "name": "ANTHROPIC_DEFAULT_OPUS_MODEL",
            "value": "deepseek-v4-pro[1m]"
        },
        {
            "name": "ANTHROPIC_DEFAULT_HAIKU_MODEL",
            "value": "deepseek-v4-flash"
        }
    ],
    "claudeCode.preferredLocation": "panel"
}
```

### 3.3 各字段说明

| 环境变量 | 作用 | 值 |
|----------|------|-----|
| `ANTHROPIC_BASE_URL` | 自定义 API 端点 | `https://api.deepseek.com/anthropic` |
| `ANTHROPIC_AUTH_TOKEN` | 认证 Token | DeepSeek 平台的 API Key |
| `ANTHROPIC_MODEL` | 使用的模型 | `deepseek-v4-pro[1m]`（Max 版本，1M 上下文），或 `deepseek-v4-pro`（128K）。`[1m]` 适用于复杂任务，但 Token 消耗会翻倍 |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 覆盖 Sonnet 模型 | `deepseek-v4-pro[1m]` |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 覆盖 Opus 模型 | `deepseek-v4-pro[1m]` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 覆盖 Haiku 模型（轻量任务） | `deepseek-v4-flash` |

> **注意**：`claudeCode.environmentVariables` 的值是一个**数组**，每个元素是包含 `name` 和 `value` 的对象。配置键名是 `claudeCode`（驼峰），不是 `claude-code`。
>
> **配置推理深度**：打开 Claude Code 对话框后，点击输入框右侧 **+ 号旁边的斜杠按钮**，在其中设置 Effort 为 **MAX**，以获得最高推理能力。


### 3.4 面板位置（可选）

```json
"claudeCode.preferredLocation": "panel"
```

- `panel` — 底部面板显示
- `editor` — 编辑器区域显示

## 4. 在 VS Code 中使用 Claude Code

### 4.1 开始使用

1. **创建/打开项目文件夹**：在电脑上创建一个文件夹（或使用已有项目文件夹）
2. **打开文件夹**：VS Code 中 **File** → **Open Folder**，选择你的文件夹
3. **启动 Claude Code**：点击 VS Code 右侧工具栏的**橙色雪花状按钮**，打开 Claude Code 对话面板
4. **开始对话编程**：在输入框中输入任务指令，Claude Code 会直接在你的文件夹中读写文件、执行命令

> 也可用快捷键 `Ctrl+Shift+L` 打开，或 `Ctrl+Shift+P` → 输入 "Claude Code"。

### 4.2 常用操作

| 操作 | 方式 |
|------|------|
| 提问/任务 | 在输入框输入指令 |
| 引用代码 | 选中代码后右键 → Add to Context |
| 引用文件 | 拖拽文件到输入框 |
| 批准操作 | 工具调用时点 Allow / Always allow |
| 拒绝操作 | 点 Deny / 按 Esc |

### 4.3 常用斜杠命令

| 命令 | 功能 |
|------|------|
| `/help` | 查看帮助 |
| `/clear` | 清空对话历史 |
| `/init` | 为项目生成 CLAUDE.md |
| `/cost` | 查看 Token 用量 |

### 4.4 使用示例

在 Claude Code 输入框中输入以下指令：

```
使用 HTML/CSS/JS 写一个前端 2048 小游戏，所有代码放到 game/ 文件夹下
```

Claude Code 会自动：
- 在 `game/` 目录下创建 `index.html`（页面结构 + 内联样式 + 游戏逻辑）
- 如果需要，按 `Allow` 批准文件写入操作
- 完成后在浏览器中打开 `game/index.html`，使用键盘**上下左右方向键**控制方块移动，相同数字合并得分

## 5. 常见问题

### Q: 连接失败

检查 `ANTHROPIC_BASE_URL` 是否正确（DeepSeek 的端点带 `/anthropic` 后缀），以及 API Key 是否有效。

### Q: 模型不可用

确认 `ANTHROPIC_MODEL` 值为 `DeepSeek-V4-Pro`（注意大小写和连字符）。

### Q: 如何切回 Anthropic 官方模型

删除 `claudeCode.environmentVariables` 中的 `ANTHROPIC_BASE_URL` 相关配置（或将其设置为空），使用 Anthropic API Key 即可。

## 6. 参考资料

- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code)
- [DeepSeek API 文档](https://platform.deepseek.com/api-docs)
