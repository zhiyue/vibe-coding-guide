# 附录：Claude Code + 智谱 GLM 安装配置指南

> **本指南你将学到：** 如何在国内环境下安装 Claude Code，并配置智谱 GLM 模型，让你在终端里用 AI 写代码。

## 这个指南适合谁

在第三章里我们介绍过，Claude Code 是一个强大的命令行 AI 编程工具，但国内用户无法直接使用 Anthropic 的官方服务。好消息是，**智谱提供了完全兼容的 GLM Coding 方案**——你用的是 Claude Code 的界面和操作方式，但背后跑的是智谱的 GLM 模型。

如果你已经用过 Lovable、Trae 或 Cursor 等工具，想尝试更强大的命令行端工具，这个指南就是为你准备的。

> **提醒：** Claude Code 是命令行工具，入门门槛比网页端和编辑器端工具高一些。如果你是完全的新手，建议先从 Lovable 或 Trae 开始体验 vibe coding，等熟悉了基本流程再回来尝试。

---

## 第一步：确认前置条件

在安装 Claude Code 之前，你需要确保电脑上已经装好了以下工具：

### Node.js（18 或更新版本）

打开终端，输入：

```bash
node --version
```

- 看到 `v18.x.x` 或更高版本号 → 可以继续
- 提示"command not found"或版本低于 18 → 需要先安装或更新 Node.js

**没装 Node.js？** 回到[第二章的"按路线准备环境"](02-what-you-need.md#按路线准备环境)按步骤安装。

**Mac 用户推荐用 nvm 或 Homebrew 安装 Node.js**，可以避免权限问题：

```bash
# 用 Homebrew
brew install node

# 或用 nvm（Node 版本管理器）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install --lts
```

### Git

```bash
git --version
```

- 看到版本号 → 可以继续
- 提示找不到 → Mac 用户运行 `xcode-select --install`，Windows 用户从 https://git-scm.com 下载安装

---

## 第二步：安装 Claude Code

打开终端，输入一行命令：

```bash
npm install -g @anthropic-ai/claude-code
```

等待安装完成（通常需要 1-2 分钟），然后验证：

```bash
claude --version
```

看到版本号（建议 2.0.14 或更新）就说明安装成功了。

> **安装失败？** 常见原因：
> - **权限不够：** Mac/Linux 用户试试在命令前加 `sudo`：`sudo npm install -g @anthropic-ai/claude-code`
> - **npm 版本太旧：** 先运行 `npm install -g npm` 更新 npm
> - **网络问题：** 可以配置 npm 镜像：`npm config set registry https://registry.npmmirror.com`

---

## 第三步：注册智谱账号并订阅 GLM Coding

### 1. 注册智谱开放平台

打开 https://open.bigmodel.cn ，注册一个账号（支持手机号注册）。

### 2. 订阅 GLM Coding 套餐

打开 https://www.bigmodel.cn/glm-coding ，选择适合你的套餐：

| 套餐 | 价格 | 每 5 小时配额 | 每周配额 | 适合谁 |
|------|------|-------------|---------|--------|
| **Lite** | ¥49/月 | ~80 次对话 | ~400 次 | 轻度使用、体验尝鲜 |
| **Pro** | ¥149/月 | ~400 次对话 | ~2,000 次 | 日常开发（最受欢迎） |
| **Max** | ¥469/月 | ~1,600 次对话 | ~8,000 次 | 重度使用 |

> **建议：** 如果不确定用量，先从 Lite 开始体验。不够用了再升级，随时可以在控制台调整。

### 3. 获取 API Key

1. 登录智谱开放平台（https://open.bigmodel.cn）
2. 进入控制台，找到 **API Keys** 页面
3. 点击 **创建 API Key**
4. 复制生成的 Key，**妥善保存**——这是你的"钥匙"，后面配置要用

<!-- screenshot: 智谱开放平台 API Key 创建页面 -->

> **重要：** API Key 只在创建时显示一次。如果忘记了，就删掉重新创建一个。

---

## 第四步：配置 Claude Code 连接智谱

这一步是把你的 Claude Code "指向"智谱的服务器。有三种方式，选一种就行：

### 方式一：一键配置工具（推荐，最简单）

在终端运行：

```bash
npx @z_ai/coding-helper
```

按照提示操作，输入你的 API Key，工具会自动帮你配置好一切。**Mac、Linux、Windows 都支持。**

### 方式二：自动脚本（仅 Mac / Linux）

```bash
curl -O "https://cdn.bigmodel.cn/install/claude_code_env.sh" && bash ./claude_code_env.sh
```

脚本会自动创建配置文件并设置好环境变量。

### 方式三：手动配置（所有平台）

如果上面两种方式不好使，手动配置也很简单：

**第 1 步：创建 Claude Code 配置文件**

用任意文本编辑器创建（或编辑）文件 `~/.claude/settings.json`：

- **Mac / Linux：** 文件路径是 `~/.claude/settings.json`（`~` 代表你的用户主目录）
- **Windows：** 文件路径是 `C:\Users\你的用户名\.claude\settings.json`

> **找不到 `.claude` 文件夹？** 那就新建一个。Mac/Linux 在终端运行 `mkdir -p ~/.claude`，Windows 在文件资源管理器的地址栏输入 `%USERPROFILE%` 回车，然后新建名为 `.claude` 的文件夹。

写入以下内容（把 `你的API Key` 替换成第三步获取的真实 Key）：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你的API Key",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1
  }
}
```

**每个字段的含义：**
- `ANTHROPIC_AUTH_TOKEN`：你的智谱 API Key（用于身份验证）
- `ANTHROPIC_BASE_URL`：告诉 Claude Code 把请求发到智谱而不是 Anthropic
- `API_TIMEOUT_MS`：请求超时时间，设为 3000 秒（避免复杂任务超时）
- `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`：关闭不必要的网络请求，避免连接 Anthropic 服务器失败

**第 2 步：跳过登录引导**

创建（或编辑）文件 `~/.claude.json`（注意：这个文件在用户主目录下，不在 `.claude` 文件夹里）：

- **Mac / Linux：** `~/.claude.json`
- **Windows：** `C:\Users\你的用户名\.claude.json`

写入：

```json
{
  "hasCompletedOnboarding": true
}
```

这是告诉 Claude Code "我已经配置好了，不需要走官方的登录流程"。

---

## 第五步：启动验证

**关掉所有终端窗口**，重新打开一个新的终端（这很重要，确保配置生效），然后：

```bash
claude
```

如果看到 Claude Code 的交互界面，恭喜你，配置成功了！

进入 Claude Code 后，输入 `/status` 可以查看当前连接状态和模型信息。

<!-- screenshot: Claude Code 启动成功后的界面 -->

> **启动后报错？** 常见问题排查：
> - **"Invalid API Key"**：检查 `settings.json` 里的 API Key 是否正确复制，有没有多余的空格
> - **网络连接失败**：确认能访问 `open.bigmodel.cn`
> - **JSON 格式错误**：检查配置文件的 JSON 格式——注意大括号、逗号、引号是否正确。可以用 https://jsonlint.com 在线验证
> - **配置没生效**：删除 `~/.claude/settings.json`，重新创建，然后关掉终端重新打开

---

## 模型说明

配置完成后，Claude Code 内部的模型会自动映射到智谱的 GLM 模型：

| Claude Code 内部模型 | 实际使用的 GLM 模型 | 说明 |
|---------------------|-------------------|------|
| Haiku（快速模型） | GLM-4.5-Air | 速度快，适合简单任务 |
| Sonnet（默认模型） | GLM-4.7 | 日常使用的主力模型 |
| Opus（最强模型） | GLM-4.7 | 默认也是 GLM-4.7 |

你不需要额外做什么——Claude Code 会自动根据任务选择合适的模型。

### 想用更强的 GLM-5？

GLM-5 是智谱目前最强的模型，适合特别复杂的编程任务。如果你想用它，需要在 `~/.claude/settings.json` 的 `env` 里加一行：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你的API Key",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1,
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-5"
  }
}
```

> **注意：** GLM-5 消耗配额更多——非高峰时段按 2 倍计算，高峰时段（每天 14:00-18:00）按 3 倍计算。Lite 套餐暂不支持 GLM-5，需要 Pro 或 Max 套餐。

---

## 日常使用小贴士

### 基本用法

在你的项目文件夹里打开终端，运行 `claude`，然后就可以用自然语言和它对话了：

```
> 帮我看看这个项目的结构，告诉我它是做什么的

> 在 src/utils 下新建一个日期格式化的工具函数

> 这个报错是什么意思？（粘贴错误信息）
```

### 常用命令

在 Claude Code 的对话中，你可以输入以下命令：

| 命令 | 作用 |
|------|------|
| `/status` | 查看连接状态和模型信息 |
| `/compact` | 压缩对话历史，释放上下文空间 |
| `/clear` | 清空当前对话 |
| `/help` | 查看所有可用命令 |

### 配额管理

- 在智谱控制台的 **财务 → 订阅管理** 中可以查看剩余配额
- 配额按 5 小时滚动窗口 + 每周总量双重限制
- 如果提示配额不足，等 5 小时后会自动恢复一部分
- 避免在高峰时段（14:00-18:00）执行大量任务，这段时间配额消耗更快

---

## 常见问题

### Claude Code 和 Cursor/Trae 有什么区别？

最大的区别是交互方式。Cursor 和 Trae 有图形界面——你能看到代码编辑器、文件树、预览窗口。而 Claude Code 完全在终端里运行，通过纯文字对话来操作。

Claude Code 的优势是**能理解整个项目**并做跨文件的复杂修改，适合有一定经验后处理更复杂的任务。

### 可以和 Cursor/Trae 同时使用吗？

可以。很多开发者的工作流是：用 Cursor 或 Trae 做日常开发，遇到需要大规模重构或复杂跨文件修改时切换到 Claude Code。

### 智谱的 GLM 模型和 Anthropic 原版 Claude 差距大吗？

GLM-4.7 在编程任务上表现很好，日常 vibe coding 使用完全够用。和原版 Claude 相比各有优劣，但对于本教程覆盖的场景来说差异不大。GLM-5 则更接近顶级模型的水平。

### 订阅后可以退款吗？

根据智谱的政策，订阅服务一经购买不支持退款。建议先从 Lite 套餐（¥49/月）开始体验。
