# Agent 代码地图（Code Map）

> 用途：对比 claude-code / openclaw / hermes-agent 三大 Agent 实现时，快速定位架构模块对应的代码位置。
> 维护方式：每次深入阅读某个模块后，更新对应行的"设计特点"和"关键文件"。

---

## claude-code（Anthropic Claude Code CLI 泄露源码）

**基本信息**：TypeScript / Bun / ~584 文件 / ~51 万行  
**架构风格**：命令驱动 + 工具链 + Bridge 桥接层  
**终端 UI**：React + Ink

### 1. Loop / Workflow（执行循环）

| 模块 | 路径 | 说明 |
|------|------|------|
| 命令注册 | `src/commands/` | advisor, brief, commit, init, review, security-review 等 |
| 任务生命周期 | `src/tasks/` | LocalMainSessionTask, stopTask, types |
| 协调器 | `src/coordinator/coordinatorMode.ts` | 协调模式（仅一个文件，可能较薄） |
| 会话运行 | `src/bridge/sessionRunner.ts` | Bridge 层驱动的会话执行 |

**设计特点**：命令和任务分离。命令 = 用户触发的顶层操作（如 `/commit`），任务 = 内部执行单元。

### 2. Tools（工具链）

claude-code 的工具按类型分目录，每个工具目录通常包含：
- `<ToolName>Tool.ts` — 工具主实现
- `prompt.ts` — 工具的 System Prompt 片段
- `constants.ts` — 常量配置

| 工具类别 | 路径 | 说明 |
|---------|------|------|
| Bash 执行 | `src/tools/BashTool/` | 含权限、安全验证、沙盒检测 |
| PowerShell | `src/tools/PowerShellTool/` | Windows 专用，同样有权限体系 |
| 文件操作 | `src/tools/FileEditTool/`, `FileReadTool/`, `FileWriteTool/` | 核心文件操作 |
| 代码搜索 | `src/tools/GlobTool/`, `GrepTool/` | 文件搜索 |
| LSP 语言服务 | `src/tools/LSPTool/` | 语言服务器协议客户端 |
| MCP 协议 | `src/tools/MCPTool/`, `McpAuthTool/` | Model Context Protocol |
| Web | `src/tools/WebFetchTool/`, `WebSearchTool/` | 网络搜索和抓取 |
| Agent/子Agent | `src/tools/AgentTool/` | 含 forkSubagent, runAgent, builtInAgents |
| Skill | `src/tools/SkillTool/` | Skill 调用入口 |
| 任务管理 | `src/tools/Task*Tool/` | TaskCreate, TaskGet, TaskList, TaskStop, TaskUpdate |
| 团队管理 | `src/tools/Team*Tool/` | TeamCreate, TeamDelete |
| Todo | `src/tools/TodoWriteTool/` | Todo 列表管理 |
| 定时任务 | `src/tools/ScheduleCronTool/` | CronCreate, CronDelete, CronList |
| 其他 | `src/tools/REPLTool/`, `BriefTool/`, `ConfigTool/` | REPL、摘要、配置 |

**设计特点**：工具数量极多（30+ 种），每个工具有独立的 prompt/权限/安全验证，高度模块化。

### 3. Permissions（权限系统）

| 模块 | 路径 | 说明 |
|------|------|------|
| 权限回调 | `src/bridge/bridgePermissionCallbacks.ts` | Bridge 层统一处理权限请求 |
| Bash 权限 | `src/tools/BashTool/bashPermissions.ts` | Bash 命令的权限规则 |
| Bash 安全 | `src/tools/BashTool/bashSecurity.ts` | 破坏性命令警告 |
| PowerShell 权限 | `src/tools/PowerShellTool/powershellPermissions.ts` | PowerShell 权限规则 |
| 只读验证 | `src/tools/BashTool/readOnlyValidation.ts` | 只读模式检查 |

**设计特点**：权限不是集中式，而是**分散在每个工具内部** + Bridge 层统一回调。每个工具自己定义什么操作需要权限确认。

### 4. State / Memory（状态与记忆）

| 模块 | 路径 | 说明 |
|------|------|------|
| 会话历史 | `src/assistant/sessionHistory.ts` | 对话历史管理 |
| 启动状态 | `src/bootstrap/state.ts` | 应用启动时的状态恢复 |
| 状态管理 | `src/state/` | （需进一步扫描具体文件） |
| 记忆目录 | `src/memdir/` | （需进一步扫描） |
| 服务层 | `src/services/` | （需进一步扫描） |

**设计特点**：记忆分散在多个模块，没有看到集中的向量数据库或 RAG 实现。主要是**对话历史的线性存储**。

### 5. UI / Output（终端 UI）

| 模块 | 路径 | 说明 |
|------|------|------|
| React 组件 | `src/components/` | Ink/React 终端组件 |
| 屏幕/页面 | `src/screens/` | 不同场景的屏幕布局 |
| Ink 封装 | `src/ink/` | Ink 库相关封装 |
| 输出样式 | `src/outputStyles/` | 文本样式和格式化 |
| 输入钩子 | `src/hooks/` | React hooks |

**设计特点**：完整的终端 UI 框架，用 React 的组件化思维做 CLI 界面。

### 6. Skills（技能系统）

| 模块 | 路径 | 说明 |
|------|------|------|
| Skill 加载 | `src/skills/loadSkillsDir.ts` | 从目录加载 skill |
| 内置 Skill | `src/skills/bundledSkills.ts` | 打包的默认 skills |
| MCP Skill 构建 | `src/skills/mcpSkillBuilders.ts` | 从 MCP 构建 skill |

**设计特点**：Skill 是工具的上一层抽象。工具是原子能力，Skill 是组合工作流。

### 7. Bridge（桥接层）

| 模块 | 路径 | 说明 |
|------|------|------|
| Bridge 主入口 | `src/bridge/bridgeMain.ts` | Bridge 核心 |
| 权限回调 | `src/bridge/bridgePermissionCallbacks.ts` | 权限请求处理 |
| 会话 API | `src/bridge/codeSessionApi.ts` | 会话管理 API |
| 远程桥接 | `src/bridge/remoteBridgeCore.ts` | 远程连接 |
| REPL Bridge | `src/bridge/replBridge.ts` | REPL 模式桥接 |
| 消息路由 | `src/bridge/bridgeMessaging.ts` | 消息传递 |
| 配置轮询 | `src/bridge/pollConfig.ts` | 动态配置更新 |

**设计特点**：Bridge 是 claude-code 的核心创新——**把本地 CLI 和云端 LLM 服务桥接起来**，处理权限、消息、会话生命周期。这是其他本地优先 Agent 没有的模块。

---

## openclaw（个人 AI 助手）

**基本信息**：TypeScript / pnpm / 本地优先 / 多平台  
**架构风格**：Skill 为核心 + 多平台 App + Heartbeat 调度  
**状态**：✅ 源码已就绪（~40+ 内置 skill + 多平台 apps）

**注意**：openclaw **没有传统 `src/` 目录**，架构以 **`.agents/skills/`** 为中心——skill 既是工具，也是工作流。

### 1. Loop / Workflow（执行循环）

| 模块 | 路径 | 说明 |
|------|------|------|
| 内置 Skill | `.agents/skills/` | 40+ 个 skill（爬虫、监控、测试、发布等） |
| 平台网关 | 集成在各 skill 中 | WhatsApp / Telegram / Slack / Discord / iMessage |
| Heartbeat | 集成在各 skill 中 | 定时唤醒，非独立模块 |

**设计特点**：**Skill 即工作流**——没有独立的"主循环"，每个 skill 自带触发条件（定时/消息/事件），Agent 核心是 skill 的调度和执行引擎。

### 2. Tools（工具链）

| 工具类别 | 路径 | 说明 |
|---------|------|------|
| 爬虫类 | `.agents/skills/discrawl/` | Discord 爬虫 |
| 爬虫类 | `.agents/skills/slacrawl/` | Slack 爬虫 |
| 爬虫类 | `.agents/skills/notcrawl/` | Notion 爬虫 |
| 爬虫类 | `.agents/skills/graincrawl/` | Grain 爬虫 |
| 监控类 | `.agents/skills/openclaw-testing/` | 测试自动化 |
| 监控类 | `.agents/skills/openclaw-parallels-smoke/` | 冒烟测试 |
| 发布类 | `.agents/skills/release-openclaw-*` | 多平台发布流水线 |
| 安全类 | `.agents/skills/security-triage/` | 安全事件分类 |
| 代码类 | `.agents/skills/openclaw-refactor-docs/` | 文档重构 |

**设计特点**：工具 = Skill = 工作流。**40+ 个 skill 覆盖"开发-测试-发布-监控"完整 DevOps 流水线**，不是生活场景工具。

### 3. Permissions（权限系统）

| 模块 | 路径 | 说明 |
|------|------|------|
| 沙盒执行 | 内置（各 skill 自行控制） | 受限执行环境 |
| 企业治理 | `deploy/` 或外部（NemoClaw） | NVIDIA 合作的企业版 |
| 安全修复记录 | `CHANGELOG.md` / `SECURITY.md` | WebSocket RCE 等漏洞修复历史 |

**设计特点**：权限分散在每个 skill 内部，没有统一的权限中心。

### 4. State / Memory（状态与记忆）

| 模块 | 路径 | 说明 |
|------|------|------|
| 配置存储 | `config/` | tsconfig 等构建配置 |
| 部署配置 | `deploy/` | Docker 等部署配置 |
| 持久化 | 未在顶层看到 memory/ 目录 | 可能在各 skill 内部或运行时生成 |

**设计特点**：未看到集中式记忆模块，可能**记忆分散在各 skill 的运行时状态**中。

### 5. UI / Output（界面）

| 模块 | 路径 | 说明 |
|------|------|------|
| macOS 原生 App | `apps/macos/` | macOS 桌面应用 |
| macOS MLX TTS | `apps/macos-mlx-tts/` | Apple Silicon 语音合成 |
| iOS App | `apps/ios/` | 移动端 |
| Android App | `apps/android/` | 移动端 |
| 共享代码 | `apps/shared/` | 跨平台共享逻辑 |
| Swabble | `apps/swabble/` | 可能是消息桥接 |

**设计特点**：**多平台原生应用**——不像 claude-code 只做终端，openclaw 有完整的桌面+移动端 App 矩阵。

### 6. Skills（技能系统）

| 模块 | 路径 | 说明 |
|------|------|------|
| 内置 Skill 目录 | `.agents/skills/` | 40+ 个，覆盖开发/测试/发布/监控 |
| Skill 元信息 | `.agents/maintainer-notes/` | Skill 维护文档 |
| ClawHub（在线） | 外部生态 | 33,000+ 社区技能 |

**设计特点**：**Skill 是 openclaw 的架构核心**——不是"附加插件"，而是"主要工作单元"。每个 skill 可能包含自己的触发器、工具调用、状态管理。

---

## hermes-agent（自进化 AI 助手）

**基本信息**：Python / 200+ 模型支持 / MIT License / ~98K stars  
**架构风格**：FTS5 持久记忆 + 自改进 Skill + Subagent 委派 + 多平台网关  
**状态**：✅ 源码已就绪（agent/, gateway/, cron/, optional-skills/）

### 1. Loop / Workflow（执行循环）

| 模块 | 路径 | 说明 |
|------|------|------|
| Agent 核心 | `agent/` | 主循环：消息接收 → 意图理解 → 工具选择 → 执行 → 记忆更新 |
| LSP 语言服务 | `agent/lsp/` | 语言服务器协议客户端 |
| 传输层 | `agent/transports/` | 消息传输抽象 |
| 密钥源 | `agent/secret_sources/` | API key 等敏感信息管理 |
| CLI 入口 | `hermes_cli/` | 命令行交互（含 dashboard_auth, proxy） |

**设计特点**：**网关驱动**——用户从任意消息平台发消息，gateway 统一路由到 `agent/` 核心处理。

### 2. Tools（工具链）

| 工具类别 | 路径 | 说明 |
|---------|------|------|
| GitHub 集成 | 内置在 agent/ | Issue triage、PR review、Actions 监控 |
| MCP 可选扩展 | `optional-mcps/linear/` | Linear 项目管理 |
| MCP 可选扩展 | `optional-mcps/n8n/` | n8n 工作流自动化 |
| 其他工具 | 内置在 agent/ | Web 搜索、代码搜索、通知推送 |

**设计特点**：工具聚焦在**自动化和监控**——GitHub 工作流、定时报告、基础设施监控。MCP 扩展通过 `optional-mcps/` 按需加载。

### 3. Permissions（权限系统）

| 模块 | 路径 | 说明 |
|------|------|------|
| 执行权限 | 内置在 agent/ | 工具调用前的确认机制 |
| ACP 适配器 | `acp_adapter/` | Agent Capability Protocol 适配 |
| ACP 注册表 | `acp_registry/` | 能力注册和发现 |

**设计特点**：资料较少，但 ACP（Agent Capability Protocol）模块的存在说明 hermes 有**标准化的能力声明和权限协商机制**。

### 4. State / Memory（状态与记忆）

| 模块 | 路径 | 说明 |
|------|------|------|
| FTS5 数据库 | 内置在 agent/ | SQLite FTS5 全文索引持久记忆 |
| Agent 策展记忆 | 内置 | Agent 主动选择记忆什么、遗忘什么 |
| 跨会话召回 | 内置 | 重启后仍能记住用户偏好 |
| 数据生成配置 | `datagen-config-examples/` | 数据生成和记忆训练示例 |

**设计特点**：**FTS5 是核心创新**——用 SQLite 的全文搜索做记忆检索，比向量数据库轻量，但功能足够。Agent 自己决定"什么值得记住"。

### 5. UI / Output（界面）

| 模块 | 路径 | 说明 |
|------|------|------|
| CLI | `hermes_cli/` | 命令行交互 |
| Dashboard | `hermes_cli/dashboard_auth/` | Web 仪表盘认证 |
| 代理服务 | `hermes_cli/proxy/` | 代理适配器（含 adapters/） |
| 信息图表 | `infographic/` | 数据可视化（kanban-db-corruption-defense 等） |
| 消息平台 | `gateway/platforms/` | Telegram / Discord / Slack / WhatsApp / Signal / QQ |

**设计特点**：多平台网关是主要交互方式，QQBot 的支持说明对中国市场有考虑。

### 6. Skills（技能系统）

| 模块 | 路径 | 说明 |
|------|------|------|
| 可选技能 | `optional-skills/autonomous-ai-agents/` | 自主 AI Agent 技能 |
| 可选技能 | `optional-skills/autonomous-ai-agents/blackbox/` | Blackbox 集成 |
| 可选技能 | `optional-skills/autonomous-ai-agents/honcho/` | Honcho 用户画像 |
| 可选技能 | `optional-skills/autonomous-ai-agents/openhands/` | OpenHands 编码助手 |
| 社区生态 | 外部 `awesome-hermes-agent` | 社区技能列表 |

**设计特点**：**自进化是核心卖点**——不是静态技能库，而是 Agent 能自己创建、改进技能。`optional-skills/` 目录说明技能是**模块化、可插拔**的。

### 7. Cron 调度（定时任务）

| 模块 | 路径 | 说明 |
|------|------|------|
| Cron 引擎 | `cron/` | 定时任务调度 |
| Docker 编排 | `docker/s6-rc.d/main-hermes/` | 容器内服务管理 |
| Docker 仪表板 | `docker/s6-rc.d/dashboard/` | 容器内 Web UI |
| Docker 用户服务 | `docker/s6-rc.d/user/` | 用户级容器服务 |

**设计特点**：**Docker-first 部署**——整个系统通过 Docker Compose 部署，cron 和主服务都在容器内管理。s6-rc 是进程监督系统。

### 8. Subagent（子代理委派）

| 模块 | 路径 | 说明 |
|------|------|------|
| 子代理技能 | `optional-skills/autonomous-ai-agents/` | 为并行任务生成隔离子代理 |
| OpenHands | `optional-skills/autonomous-ai-agents/openhands/` | 专门的编码子代理 |

**设计特点**：**多 Agent 协作**——主 Agent 可以 spawn 子代理并行处理不同工作流。OpenHands 是一个专门的编码子代理示例。

---

## 对比维度速查

当你想问"三个 Agent 在 X 方面有什么区别"时，直接在下面找对应维度，然后定位到每个 Agent 的代码地图条目。

| 对比维度 | claude-code | openclaw | hermes-agent |
|---------|-------------|----------|--------------|
| **语言/运行时** | TypeScript / Bun | TypeScript / pnpm | Python |
| **工具数量** | 30+，高度模块化 | 70+ 原生 + 33,000+ ClawHub | 聚焦 GitHub/监控/搜索 |
| **权限设计** | 分散式（每个工具自带）+ Bridge 回调 | 沙盒 + 企业版 NemoClaw | 基础执行权限 |
| **记忆实现** | 线性对话历史（无向量检索）| Markdown 文件 + 可能向量 | **FTS5 SQLite 全文索引** |
| **循环控制** | 命令驱动 + 任务生命周期 | **Heartbeat 主动唤醒** | 网关驱动 + Cron 调度 |
| **UI 方案** | React + Ink 终端 | CLI + Web UI + 消息 App | CLI + Web UI + 消息 App |
| **Skill 系统** | MCP + 内置（静态） | ClawHub 社区（33,000+） | **自改进 + 自主创建** |
| **主动能力** | ❌ 被动响应 | ✅ Heartbeat 定时唤醒 | ✅ Cron 定时任务 |
| **多平台** | CLI 为主 | WhatsApp/Telegram/Slack/Discord/iMessage | Telegram/Discord/Slack/WhatsApp/Signal |
| **独特能力** | Bridge 云端桥接 | **生活自动化**（日历、智能家居） | **自进化 Skill**（Agent 自己写技能） |
| **子 Agent** | 有（forkSubagent, runAgent） | 无明确资料 | **Subagent 委派** |
| **定位** | 开发工具（IDE 替代） | 个人生活助手 | 研究与自动化助手 |

---

## 使用指南

**场景 1：你想问"三个 Agent 的权限系统有什么不同"**
1. 打开本文档 → 找到 Permissions 行
2. claude-code 已填 → 直接读 `src/tools/BashTool/bashPermissions.ts` 等
3. openclaw / hermes-agent 显示"待扫描" → 告诉我，我去扫描对应目录

**场景 2：你想问"哪个 Agent 的子 Agent 设计最好"**
1. 找到 Agent/Subagent 相关条目
2. claude-code：`src/tools/AgentTool/`（forkSubagent, runAgent）
3. hermes-agent：待扫描 → 需要我读源码后补充

**场景 3：发现某个条目不够详细**
直接告诉我"claude-code 的 State 模块需要补充"，我会深入扫描 `src/state/`、`src/services/`、`src/memdir/` 等目录，更新本文档。
