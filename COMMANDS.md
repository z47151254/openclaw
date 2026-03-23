# OpenClaw 指令速查手册

> 本文档列出所有常用 pnpm 指令，分为 **Windows 兼容指令（`:win` 后缀）** 和 **通用/Linux 指令**。
> Windows 用户优先使用 `:win` 后缀的指令，或使用全局安装的 `openclaw` 命令。

---

## 全局安装方式（推荐）

```powershell
npm install -g openclaw@latest     # 安装
npm uninstall -g openclaw          # 卸载
openclaw --version                 # 验证安装
```

安装后可直接使用 `openclaw xxx` 命令，无需 `pnpm` 或 `node scripts/...`。

---

## 一、初始化引导（Onboard）

首次使用必须运行，会引导你选择 AI provider、输入 API key、选模型。

| 指令 | 平台 | 说明 |
|------|------|------|
| `openclaw onboard --install-daemon` | 全局安装 | 官方推荐方式，含守护进程安装 |
| `openclaw onboard` | 全局安装 | 仅引导配置，不装守护进程 |
| `pnpm onboard:win` | Windows 源码 | 正式模式引导，配置写入 `~/.openclaw/` |
| `pnpm onboard:dev:win` | Windows 源码 | dev 模式引导，配置写入 `~/.openclaw-dev/` |

---

## 二、Gateway 网关

Gateway 是 OpenClaw 的核心服务，负责 AI 对话和消息路由。

### 启动

| 指令 | 平台 | 说明 |
|------|------|------|
| `openclaw gateway --port 18789` | 全局安装 | 前台启动，默认端口 18789 |
| `pnpm gateway:dev:win` | Windows 源码 | dev 模式启动，端口 19001，跳过频道 |
| `pnpm gateway:run:win` | Windows 源码 | 正式模式启动，端口 18789 |
| `pnpm gateway:dev` | Linux/macOS 源码 | dev 模式启动 |

### 重置

| 指令 | 平台 | 说明 |
|------|------|------|
| `pnpm gateway:dev:reset:win` | Windows 源码 | 重置 dev 配置并启动 |
| `pnpm gateway:dev:reset` | Linux/macOS 源码 | 重置 dev 配置并启动 |

### 状态与停止

| 指令 | 平台 | 说明 |
|------|------|------|
| `openclaw gateway status` | 全局安装 | 查看 Gateway 运行状态 |
| `openclaw gateway stop` | 全局安装 | 停止 Gateway |
| `pnpm gateway:status:win` | Windows 源码 | 查看状态 |
| `pnpm gateway:stop:win` | Windows 源码 | 停止 Gateway |

### 监听模式

| 指令 | 平台 | 说明 |
|------|------|------|
| `pnpm gateway:watch` | 通用 | 文件变更自动重启 Gateway |

---

## 三、Dashboard 控制面板

浏览器 Web UI，用于和 AI 对话、管理配置。

| 指令 | 平台 | 说明 |
|------|------|------|
| `openclaw dashboard` | 全局安装 | 自动打开浏览器并复制链接 |
| `pnpm dashboard:win` | Windows 源码 | 正式模式，端口 18789 |
| `pnpm dashboard:dev:win` | Windows 源码 | dev 模式，端口 19003 |

手动打开：
- 正式模式：`http://127.0.0.1:18789/`
- dev 模式：`http://127.0.0.1:19003/`

连接时需要在「网关令牌」输入框填入 `.env` 中 `OPENCLAW_GATEWAY_TOKEN` 的值。

---

## 四、构建相关

| 指令 | 平台 | 说明 |
|------|------|------|
| `pnpm install` | 通用 | 安装所有依赖 |
| `pnpm build` | 通用 | 完整构建（TypeScript 编译 + UI） |
| `pnpm ui:build` | 通用 | 仅构建 Web UI |
| `pnpm ui:dev` | 通用 | Web UI 开发模式（热更新） |

---

## 五、测试与代码质量

| 指令 | 平台 | 说明 |
|------|------|------|
| `pnpm test` | 通用 | 运行所有单元测试 |
| `pnpm test:fast` | 通用 | 快速运行单元测试 |
| `pnpm test:gateway` | 通用 | 仅测试 Gateway |
| `pnpm test:watch` | 通用 | 测试监听模式（文件变更自动重跑） |
| `pnpm lint` | 通用 | 代码检查 |
| `pnpm lint:fix` | 通用 | 自动修复代码问题 |
| `pnpm format` | 通用 | 代码格式化 |
| `pnpm check` | 通用 | 完整检查（格式 + 类型 + lint） |

---

## 六、通用启动

| 指令 | 平台 | 说明 |
|------|------|------|
| `pnpm dev` | 通用 | 源码启动（需传子命令，如 `pnpm dev -- gateway`） |
| `pnpm start` | 通用 | 等同于 `pnpm dev` |
| `pnpm openclaw` | 通用 | 等同于 `node scripts/run-node.mjs`，后面跟子命令 |
| `pnpm tui` | 通用 | 启动终端 UI 界面 |

---

## 七、配置文件位置

| 文件 | 路径 | 说明 |
|------|------|------|
| 环境变量 | 项目根目录 `.env` | API key、Gateway token 等 |
| 正式配置 | `~/.openclaw/openclaw.json` | onboard 自动生成 |
| dev 配置 | `~/.openclaw-dev/openclaw.json` | `--dev` 模式自动生成 |
| 环境变量模板 | 项目根目录 `.env.example` | 所有可用变量参考 |

Windows 下 `~` 即 `C:\Users\你的用户名\`。

---

## 八、常用 `.env` 配置

```env
# Gateway 认证令牌（本地随便写，暴露外网需用强随机值）
OPENCLAW_GATEWAY_TOKEN=你的token

# OpenRouter API Key
OPENROUTER_API_KEY=sk-or-v1-你的key

# 跳过频道启动（dev 模式下 .env 里加上就不用在命令里设了）
OPENCLAW_SKIP_CHANNELS=1
CLAWDBOT_SKIP_CHANNELS=1
```

---

## 九、常见问题

**Q: `pnpm gateway:dev` 在 Windows 报错？**
A: 用 `pnpm gateway:dev:win` 代替，或直接用全局安装的 `openclaw` 命令。

**Q: 浏览器打开 Dashboard 显示 unauthorized？**
A: 在「网关令牌」输入框里填入 `.env` 中 `OPENCLAW_GATEWAY_TOKEN` 的值。

**Q: 怎么换模型？**
A: 运行 `openclaw onboard`（或 `pnpm onboard:win`）重新选择 provider 和模型。

**Q: Gateway 占用了终端怎么办？**
A: 在 Cursor 里按 `Ctrl+Shift+`` ` 新开一个终端。
