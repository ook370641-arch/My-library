# Agent 学习计划

> 我每天都在用 Claude Code，但只是个会用工具的人。
> 这份计划的目标，是让我变成"能自己造一个 agent 的人"。

---

## 快速导航

- **[agent-code-map.md](agent-code-map.md)** —— 三大 Agent（claude-code / openclaw / hermes-agent）的代码地图。对比架构前先读这个，快速定位每个模块对应的源码位置。

---

## 终点

学完这一遍，我应该能：

- 不看任何资料解释清楚一个 agent 的核心架构（loop / memory / tools / planner / state）
- 在不依赖任何框架的情况下，从零手写一个能跑的最小 agent
- 看懂主流 agent 框架做了哪些"加厚"，并知道哪些是必要的、哪些是装饰
- 用真实框架搭出一个能解决我自己日常问题的 agent

## 三阶段路线图

```
Phase 1: 学习    (~10–14 天)  →  心里有零件图
Phase 2: 祛魅    (~2–3 天)   →  框架不再是黑盒
Phase 3: 搭建    (~1 周)     →  我有一个属于自己的、能用的 agent
Phase 4: 对比    (~3–5 天)   →  理解 production agent 的完整拼图
```

---

### Phase 1 · 学习 — 拆开发动机

**仓库**: [`./ai-agents-from-scratch/`](./ai-agents-from-scratch) (`pguso/ai-agents-from-scratch`)

**为什么是它**: 教程仓库，不是框架。用 `node-llama-cpp` + 本地 GGUF 模型，把 LLM 调用 → memory → function calling → ReAct → planning → 错误处理 一层层手写出来。**没有任何 magic 被隐藏**。

**目标**: 心里有一张完整的"agent 由哪些零件组成"的脑图。

**做法**:

1. 跟着每一课写代码（不要 copy-paste，手敲一遍）
2. 每课结束后用一句话回答：**"如果没有这一层，agent 会在什么情况下崩？"**
3. 记下任何"为什么这里要这样写"的疑问，留到 Phase 2 求解

**前置准备**:

- Node.js 20+
- 一个 GGUF 小模型（仓库 README 推荐 Qwen3-1.7B 这种量级，CPU 也能跑动）
- 在 `./ai-agents-from-scratch/models/` 下放好模型文件

**完成标志**: 不看教程，从零写出一个有 memory + 1 个 tool 的 ReAct agent。

> 备选：如果觉得 9 章不够细，可以切到同作者的 Python 版 `pguso/agents-from-scratch`（12 lessons，含 evals + telemetry）。

---

### Phase 2 · 祛魅 — 看一个真框架的全部源码

**仓库**: [`./PocketFlow/`](./PocketFlow) (`The-Pocket/PocketFlow`)

**为什么是它**: 100 行核心源码，号称 "the smallest LLM framework"。Phase 1 让我知道零件长什么样，Phase 2 让我看到"那些零件被框架包起来时，最简形态是什么"。

**目标**: 对"框架到底在做什么"祛魅——意识到大多数框架不过是把我 Phase 1 写的那些东西用 `Node` / `Flow` 抽象包起来。

**做法**:

1. 一晚上读完整个 PocketFlow 核心 (~100 行)
2. 用 PocketFlow 重写 Phase 1 最后做出来的那个 agent
3. 对比：**我手写版** vs **PocketFlow 版**，记下哪些地方更优雅、哪些地方更繁琐
4. 把 Phase 1 留下来的疑问拿出来，看 PocketFlow 是怎么解的

**完成标志**: 能向别人解释"一个 agent framework 至少要做哪 3 件事"，并指出 PocketFlow 是怎么做的。

---

### Phase 3 · 搭建 — 用真框架做一个自己用得上的 agent

**仓库**: [`./smolagents/`](./smolagents) (`huggingface/smolagents`)

**为什么是它**: HuggingFace 出品的轻量但生产级框架。最大特色是 **Code Agent**——LLM 直接写 Python 代码作动作，而不是用 JSON tool call。这种范式更贴近 Claude Code 的真实工作方式。

**目标**: 跑出一个**我自己日常用得上的** agent，证明这一路学下来的东西能转化成产出。

**做法**:

1. 跑通 smolagents 的 hello world
2. 理解 `CodeAgent` vs `ToolCallingAgent` 的差异，记录什么场景下选哪个
3. **选一个真实场景**自己造一个 agent。候选（按"对我桌面 repository 生态的相关度"排序）：

| 候选场景           | 它能做什么                                                      | 价值                 |
| -------------- | ---------------------------------------------------------- | ------------------ |
| **Skill 仓库管家** | 扫描 `repository/` 下所有 skill，自动生成 / 更新索引、检测重复、根据项目类型推荐 skill | 直接服务我自己的工作流        |
| **学习笔记 → 测验**  | 读一组 markdown 笔记，生成 Anki 卡片或自测题                             | 配合我"学习型开发"路径       |
| **GitHub 探险者** | 给一个 repo URL，自动 clone 并写出架构介绍 / 入门指南                       | 配合我后续学习其他 agent 项目 |
| **每日信息流**      | 抓 GitHub trending / HN，按偏好筛选写日报                            | 长期信息消化工具           |

**完成标志**: 能在终端里跑 `python my_agent.py`（或同等命令），它真的解决了一个我当下的问题，而不是 hello world。

---

### Phase 4 · 对比 — 看一个 Production Agent 的实现

**仓库**: [`./claude-code/`](./claude-code) (`ultraworkers/claw-code` — Claude Code 的 clean-room Rust 实现)

**为什么是它**: Phase 1-3 让你知道 agent 由哪些零件组成、框架怎么包装它们、怎么用框架做出东西。Phase 4 让你看到**一个真实的 production agent 产品**是怎么把这些零件拼起来的——不是玩具 demo，而是每天被数万人使用的 CLI 工具。

**核心洞察**: 这是 `ultraworkers/claw-code`，用 Rust 重写的 Claude Code 参考实现，~48K 行代码（73% Rust + 27% Python）。核心模块包括 `OmO`（多 agent 协调）、`OmX`（指令到协议转换）、`TaskRegistry`（任务状态管理）、`clawhip`（通知路由外置）、`PermissionEnforcer`（权限检查）等。读它不是为了抄代码，而是理解"production 级 agent"在工具调用、权限控制、上下文管理、命令路由、多 agent 协调等方面做了哪些"教程不会教"的工程决策。

**目标**: 理解"从教程级 agent 到 production 级 agent"的 gap 在哪里。

**做法**:

| 天 | 主题 | 做法 |
|---|---|---|
| 1 | 系统级视角 | 读 `PHILOSOPHY.md`，画一张"人类 → 协调层 → agent"的关系图 |
| 2 | 模块解剖 | 读 `PARITY.md` 的 9-lane 表，逐个理解每个 lane 解决什么问题 |
| 3 | 代码对照 | 选 2-3 个最关心的 lane（建议 Permission + MCP + TaskRegistry），读对应源码 |
| 4 | 回看你的 agent | 对比：你的 Phase 3 agent 缺了哪些 production 模块？哪些是不必要的？ |
| 5 | 输出 | 写一段 ≥300 字的对比分析，回答"如果从零再写一个 production agent，我会保留/增加/删除什么？" |

**通用 Agent 架构 ↔ Claude Code 源码对照表**:

| 通用架构模块 | claw-code 对应实现 | 学习内容 |
|-------------|---------------------|---------|
| **Planner** | `OmO` (oh-my-openagent) | 多 agent 协调：Architect / Executor / Reviewer 如何分工、如何处理分歧 |
| **Loop / Workflow** | `OmX` (oh-my-codex) | 把一句人类指令转成可重复执行协议：planning keywords → execution modes → verification loops |
| **State / Memory** | `TaskRegistry` + `Team+Cron` | 任务注册表、团队调度、定时任务如何管理持久状态 |
| **Tools** | File-tool + Bash + MCP + LSP | 文件操作、命令执行、MCP 桥接、LSP 客户端——生产级工具链 |
| **Notification / Routing** | `clawhip` | 事件和通知路由为什么**必须**放在 agent context window 之外 |
| **Permissions** | `PermissionEnforcer` | 权限检查如何嵌入每个工具调用路径 |
| **Testing / Verification** | `Mock Parity Harness` | 如何用 mock 服务做端到端行为验证 |

**完成标志**: 能画出 Claude Code CLI 的 architecture diagram，标出"命令输入 → 解析 → 工具调用 → LLM → 输出渲染"的完整流水线，并能解释"为什么权限检查必须放在工具调用路径的每一层"。

> **注意**: 这是 TypeScript 源码（Bun 运行时），~1,900 文件、51 万行代码。读代码时**不必读完所有文件**，重点是理解架构分层和模块职责。如果某段实现看不懂，跳过它，先看模块级接口和类型定义。

---

## 时间表

| 周                   | 任务                                                         |
| ------------------- | ---------------------------------------------------------- |
| Week 1              | Phase 1 · Lesson 01–04（LLM 调用 / memory / tools / ReAct 雏形） |
| Week 2              | Phase 1 · Lesson 05–09（planning / 错误处理 / 多步任务）             |
| Week 3 (上)          | Phase 2 · 读完 PocketFlow + 重写练习                             |
| Week 3 (下) – Week 4 | Phase 3 · smolagents + 自造一个 agent                          |
| Week 4 (下) – Week 5 | Phase 4 · claude-code 架构对比                                     |

每天 1 小时为基线。可压缩到 2 周（每天 2 小时），也可拉长到 6 周（每天 30 分钟）。

---

## 目录使用约定

```
learning-agent/
├── README.md                    # 这份计划（手动维护）
├── ai-agents-from-scratch/      # 教程仓库（pguso），从 LLM 调用到 ReAct 逐层手写
├── smolagents/                  # HuggingFace 轻量框架（CodeAgent + ToolCallingAgent）
├── PocketFlow/                  # 最小 LLM 框架（~100 行核心源码）
├── claude-code/                 # ultraworkers/claw-code（Claude Code clean-room Rust 实现，~48K 行）
├── openclaw/                    # 个人 AI 助手（TypeScript，本地优先 + 多平台 + Skills 生态）
├── hermes-agent/                # NousResearch 自进化 AI 助手（Python，持久记忆 + Cron + 多平台网关）
├── notes/                       # 我自己的学习笔记（建议建）
└── experiments/                 # 我自己的实验代码（建议建）
```

- 三个 clone 的 `.git` 各自保留 → 想升级直接 `cd <repo> && git pull`
- 自己的笔记 / 实验放 `notes/`、`experiments/`，不和第三方 repo 混在一起
- 整个 `repository/` 不是 git 仓库 → 没有需要 ignore 的东西

---

## 进度追踪

### Phase 1

- [ ] 环境就位（Node.js + GGUF 模型 + 跑通 intro 示例）
- [ ] Lesson 01–02：本地 LLM + memory
- [ ] Lesson 03–04：tools + ReAct 雏形
- [ ] Lesson 05–07：planning + 错误处理
- [ ] Lesson 08–09：多步任务 + 综合
- [ ] **里程碑：能裸写一个 memory + tool 的 ReAct agent**

### Phase 2

- [ ] 读完 PocketFlow 100 行核心
- [ ] 用 PocketFlow 重写 Phase 1 最终的 agent
- [ ] 写一段"我对 framework 的理解"自述（≥200 字）

### Phase 3

- [ ] smolagents hello world
- [ ] `CodeAgent` vs `ToolCallingAgent` 各跑一个 demo
- [ ] 选定真实场景
- [ ] **里程碑：跑出一个自己日常用得上的 agent**

### Phase 4

- [ ] 读完 `PHILOSOPHY.md`，理解"人类设定方向 → 协调层调度 → 多 agent 并行执行"的架构哲学
- [ ] 读 `PARITY.md` 的 9-lane 表，逐个理解每个 lane 解决什么问题
- [ ] 选 2-3 个最关心的 lane 读源码（建议 Permission + MCP + TaskRegistry）
- [ ] 对比分析：Phase 3 agent 缺了哪些 production 模块
- [ ] **里程碑：画出 claude-code 的 architecture diagram，标出 6 个核心模块之间的调用关系，并能解释"为什么通知路由必须外置"**

---

## 卡住时怎么办

| 症状                        | 处理                                        |
| ------------------------- | ----------------------------------------- |
| Phase 1 某课看不懂             | 跳过去看下一课，回头再啃；或把疑问交给 Claude Code 解释        |
| 本地模型跑不动                   | 换更小的 GGUF（如 Qwen3-0.6B），或临时改用 Ollama 后端   |
| Phase 2 觉得 PocketFlow 太抽象 | 改读 `huggingface/smolagents` 核心源码（也只有几百行）  |
| Phase 3 不知道做什么场景          | 默认目标：**Skill 仓库管家**，直接服务我自己的桌面 repository |
