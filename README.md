# Claude Code Skill 仓库

> 本仓库不再存放 skill 源码副本。skill 更新频繁，本地副本容易过时，直接按下面方式安装最新版即可。
> 
> 本仓库仅保留 `learning-agent/` 作为本地学习资料。

---

## 快速选择

| 你的场景 | 推荐安装 |
|---------|---------|
| 软件交付：PR 审查、发版、QA、安全审计、性能回归 | [gstack](#gstack) |
| 需求防漂移：先写 spec / 任务 / 验收标准，再实现 | [OpenSpec](#openspec) |
| 零基础学 Web coding、TDD、子 agent 协作 | [Superpowers](#superpowers) |
| 官方文档处理、前端设计、MCP、Claude API | [anthropic 官方 skills](#anthropic-官方-skills) |
| 自媒体、图文创作、翻译、排版、多平台发布 | [baoyu](#baoyu) |
| 商业诊断、内容诊断、执行力、多角色讨论 | [dbskill](#dbskill) |
| 视觉设计原型、幻灯片、海报、多格式导出 | [open-design](#open-design) |
| 全网搜索、社交媒体、代码与内容抓取 | [agent-reach](#agent-reach) |
| 计划或设计审查、防止假设漂移 | [grill-me](#grill-me) |
| 通过飞书/钉钉/Slack 等 IM 调用本地 AI agent | [cc-connect](#cc-connect) |

---

## gstack

**定位**：软件开发全生命周期技能集。CEO 视角审计划、工程经理锁架构、设计师抓 AI slop、reviewer 找生产 bug、QA 开真浏览器、安全官跑 OWASP + STRIDE、发版工程师 ship PR。

### 安装

**Claude Code**：

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack && ./setup
```

团队模式（推荐共享仓库）：

```bash
(cd ~/.claude/skills/gstack && ./setup --team) && ~/.claude/skills/gstack/bin/gstack-team-init required
git add .claude/ CLAUDE.md && git commit -m "require gstack for AI-assisted work"
```

其他 agent（Codex、Cursor、OpenCode 等）：

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack && ./setup --host <name>
```

支持 `--host codex|opencode|cursor|factory|slate|kiro|hermes|gbrain`。

升级：运行 `/gstack-upgrade`，或在 `~/.gstack/config.yaml` 设置 `auto_upgrade: true`。

### 发布与部署

| Skill | 命令 | 功能 |
|-------|------|------|
| ship | `/ship` | 自动发版：检查冲突、跑测试、写日志、提升版本、创建 PR |
| land-and-deploy | `/land-and-deploy` | 合并代码、等待部署、线上验证 |
| canary | `/canary` | 发布后金丝雀监控：截图对比、报错检测、性能告警 |
| document-release | `/document-release` | 发版后更新文档，让文档和代码保持一致 |
| document-generate | `/document-generate` | 用 Diataxis 框架从代码生成缺失文档 |
| setup-deploy | `/setup-deploy` | 检测部署平台并配置生产环境 URL |
| landing-report | `/landing-report` | 版本队列只读看板，显示 PR 排队状态 |

### 浏览器与测试

| Skill | 命令 | 功能 |
|-------|------|------|
| gstack | `/gstack` | AI 控制浏览器：点击、填表、截图、响应式测试、diff 对比 |
| open-gstack-browser | `/open-gstack-browser` | 打开可见 Chrome 窗口，实时观察 AI 操作 |
| setup-browser-cookies | `/setup-browser-cookies` | 导入浏览器 Cookie，测试登录后页面 |
| qa | `/qa` | 自动测网站、找 bug、修 bug、再验证 |
| qa-only | `/qa-only` | 只测试不修复，输出报告 |
| benchmark | `/benchmark` | 性能回归检测：加载时间、指标、前后对比 |
| benchmark-models | `/benchmark-models` | Claude/GPT/Gemini 多模型基准对比 |

### 代码审查与质量

| Skill | 命令 | 功能 |
|-------|------|------|
| review | `/review` | PR 预审：安全隐患、逻辑漏洞、性能问题 |
| autoplan | `/autoplan` | CEO、设计、工程、DX 四视角自动审查计划 |
| codex | `/codex` | OpenAI Codex 独立审查 |
| cso | `/cso` | 安全审计：OWASP、供应链、CI 风险 |
| health | `/health` | 代码质量仪表盘，输出评分和建议 |

### 安全与防护

| Skill | 命令 | 功能 |
|-------|------|------|
| careful | `/careful` | 破坏性命令前警告 |
| freeze | `/freeze` | 限制 AI 只能编辑指定目录 |
| guard | `/guard` | 同时开启 careful 和 freeze |
| unfreeze | `/unfreeze` | 解除编辑范围限制 |

### 调试与根因分析

| Skill | 命令 | 功能 |
|-------|------|------|
| investigate | `/investigate` | 系统性查 bug：调查、分析、假设、实施 |
| context-save | `/context-save` | 保存工作状态：git、决策、待办 |
| context-restore | `/context-restore` | 恢复之前保存的工作状态 |
| plan-tune | `/plan-tune` | 调整 gstack 提问敏感度 |

### 设计相关

| Skill | 命令 | 功能 |
|-------|------|------|
| design-shotgun | `/design-shotgun` | 批量生成多风格设计稿 |
| design-consultation | `/design-consultation` | 从零设计系统，如字体、颜色、排版 |
| design-html | `/design-html` | 将设计稿转为生产级 HTML/CSS |
| design-review | `/design-review` | 设计师视角像素级 QA |
| plan-design-review | `/plan-design-review` | 计划阶段设计审查 |
| devex-review | `/devex-review` | 开发者体验审查 |
| plan-devex-review | `/plan-devex-review` | 计划阶段开发者体验审查 |

### 计划审查

| Skill | 命令 | 功能 |
|-------|------|------|
| plan-ceo-review | `/plan-ceo-review` | CEO/创始人视角审查计划 |
| plan-eng-review | `/plan-eng-review` | 工程经理视角审查技术方案 |
| office-hours | `/office-hours` | YC Office Hours 式追问 |
| spec | `/spec` | 把模糊意图转成可执行 spec（含代码阅读、Codex 质量门、归档） |

### 工程复盘与学习

| Skill | 命令 | 功能 |
|-------|------|------|
| retro | `/retro` | 工程复盘，分析 commit 和质量趋势 |
| learn | `/learn` | 管理跨 session 项目学习 |

### iOS 工具（v1.43.0.0+）

| Skill | 命令 | 功能 |
|-------|------|------|
| ios-qa | `/ios-qa` | 通过 USB CoreDevice 驱动真机做 iOS QA |
| ios-fix | `/ios-fix` | iOS bug-fix 循环 |
| ios-design-review | `/ios-design-review` | 设计师视角 HIG 审查 |
| ios-clean | `/ios-clean` | 清理 iOS debug-bridge |
| ios-sync | `/ios-sync` | 重新同步 iOS accessor |

### 其他工具

| Skill | 命令 | 功能 |
|-------|------|------|
| pair-agent | `/pair-agent` | 与远程 AI Agent 协同工作 |
| gstack-upgrade | `/gstack-upgrade` | 升级 gstack |
| make-pdf | `/make-pdf` | Markdown 转出版级 PDF |
| setup-gbrain | `/setup-gbrain` | 配置跨机记忆同步 |
| sync-gbrain | `/sync-gbrain` | 重新索引当前仓库代码到 gbrain |

---

## OpenSpec

**定位**：spec-driven development。先定义需求边界、验收标准和任务拆分，再进入实现，适合需求容易漂移的项目。

### 安装

```bash
npm install -g @fission-ai/openspec@latest
```

在项目里初始化：

```bash
cd your-project
openspec init
```

然后对 AI 说：`/opsx:propose <what-you-want-to-build>`

如需扩展工作流：`openspec config profile` & `openspec update`

升级：

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

### Core profile（默认）

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| openspec-propose | `/opsx:propose` | 一步创建 change，生成 proposal/specs/design/tasks 等实现前 artifacts | 功能方向已大致清楚，想先拆清楚再写代码 |
| openspec-explore | `/opsx:explore` | 在不创建 artifacts 的情况下探索问题、读代码、比较方案 | 需求还模糊，先想清楚要做什么 |
| openspec-apply-change | `/opsx:apply` | 读取 `tasks.md`，按 checklist 逐步实现并打勾 | artifacts 已确认，开始写代码 |
| openspec-archive-change | `/opsx:archive` | 将完成的 change 归档，并将 delta specs 同步到主 specs | 功能实现/验证完成，需要留痕和收束 |

### Expanded workflow

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| openspec-new-change | `/opsx:new` | 只创建 change scaffold 和 `.openspec.yaml` | 想一步一步学习 artifacts 怎么形成 |
| openspec-continue-change | `/opsx:continue` | 按依赖关系生成下一个 artifact | 复杂功能，需要逐个审阅 |
| openspec-ff-change | `/opsx:ff` | fast-forward，一次性生成所有实现前 artifacts | 需求比较明确，但仍要留一套规格记录 |
| openspec-verify-change | `/opsx:verify` | 检查实现是否匹配 artifacts | 写完代码后，archive 前做规格对齐 |
| openspec-bulk-archive-change | `/opsx:bulk-archive` | 批量归档多个 changes | 多个小功能已完成 |
| openspec-onboard | `/opsx:onboard` | 引导式教程，走一遍完整 OpenSpec workflow | 第一次学习 OpenSpec |

---

## Superpowers

**定位**：agentic coding 方法论。自动在任务开始前澄清 idea、拆计划、TDD、完成前验证、代码审查。

### 安装（Claude Code）

官方插件市场：

```bash
/plugin install superpowers@claude-plugins-official
```

或 Superpowers 市场：

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

其他平台安装方式见官方仓库：<https://github.com/obra/superpowers>

### Skills

| Skill | 触发/用途 | 功能 | 何时使用 |
|-------|-----------|------|---------|
| using-superpowers | 自动前置 | 要求 agent 主动检查并调用合适 skill | 任何任务开始前，确保流程不跑偏 |
| brainstorming | 需求澄清 | 通过问题、方案比较和设计文档把 idea 变清楚 | 想法还模糊、准备做网页或新功能 |
| writing-plans | 写实现计划 | 把 spec/需求拆成小步 implementation plan | 开始写代码前，需要明确任务顺序 |
| using-git-worktrees | 并行分支 | 用 worktree 隔离多个开发方向 | 同时探索多个方案或避免污染主目录 |
| executing-plans | 执行计划 | 按已确认计划逐项实现并记录进度 | plan 已确认，进入落地阶段 |
| subagent-driven-development | 子 agent 开发 | 将计划任务分派给 fresh subagent 并审查结果 | 任务较多、可以并行推进 |
| test-driven-development | TDD | 先写失败测试，再写实现，再重构 | 新增业务逻辑、修 bug |
| systematic-debugging | 系统调试 | 先找 root cause，再修复 | 出现 bug、测试失败、页面异常 |
| verification-before-completion | 完成前验证 | 要求 fresh evidence，不能凭印象宣布完成 | 交付前、说“完成”之前 |
| requesting-code-review | 请求审查 | 主动发起代码审查 | 实现完成后，需要第二视角 |
| receiving-code-review | 处理审查 | 分析 review 意见并修复 | 收到审查反馈后 |
| dispatching-parallel-agents | 并行调度 | 规划多个 agent 的并行调查或实现 | 多个独立问题可并行处理 |
| finishing-a-development-branch | 收束分支 | 完成开发分支、检查状态、准备合并 | 功能完成后收尾 |
| writing-skills | 写 skill | 用清晰 workflow 和测试思维创建新 skill | 想沉淀自己的长期工作流 |

---

## anthropic 官方 skills

**定位**：Anthropic 维护的官方 skill，覆盖文档处理、前端设计、API 开发、MCP 构建、算法艺术、内部沟通。

### 安装

Claude Code 推荐通过插件市场安装：

```bash
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills
# 或示例 skill 集
/plugin install example-skills@anthropic-agent-skills
```

也可以手动克隆后按需复制：

```bash
git clone https://github.com/anthropics/skills.git ~/.claude/skills/anthropic-skills
```

```bash
# 项目级示例
cp -r ~/.claude/skills/anthropic-skills/skills/frontend-design your-project/.claude/skills/

# 全局示例（Claude Code）
cp -r ~/.claude/skills/anthropic-skills/skills/frontend-design ~/.claude/skills/
```

升级：`cd ~/.claude/skills/anthropic-skills && git pull`

### 文档处理

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| docx | `/docx` | 创建、编辑、提取 Word 文档 | 生成报告/编辑合同 |
| pdf | `/pdf` | 提取、合并、拆分、旋转、OCR、表单处理 | 处理 PDF |
| pptx | `/pptx` | 从模板或从零创建 PowerPoint | 做幻灯片 |
| xlsx | `/xlsx` | 创建、编辑、分析表格，用 Excel 公式 | 财务模型/数据整理 |

### 前端与设计

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| frontend-design | `/frontend-design` | 生成高质量前端代码并避免常见 AI 味 | 写 landing page/组件 |
| canvas-design | `/canvas-design` | 先写设计哲学，再生成静态视觉作品 | 海报/封面/视觉设计 |
| algorithmic-art | `/algorithmic-art` | p5.js 交互式生成艺术 | 生成艺术/粒子系统 |
| theme-factory | `/theme-factory` | 统一 deck 或 artifact 风格 | 统一视觉主题 |
| brand-guidelines | `/brand-guidelines` | Anthropic 官方品牌规范 | 按 Anthropic 品牌设计 |
| web-artifacts-builder | `/web-artifacts-builder` | React + TS + Vite + Tailwind 构建复杂 artifact | 带状态的交互界面 |

### 开发与工具

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| claude-api | `/claude-api` | 构建、调试、优化 Claude API 应用 | 开发 Claude API 应用 |
| mcp-builder | `/mcp-builder` | 指导创建 MCP 服务器 | 让 AI 连接私有服务 |
| webapp-testing | `/webapp-testing` | Playwright 测试脚本和服务生命周期管理 | 测本地网站 |

### 沟通与内容

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| internal-comms | `/internal-comms` | 生成内部更新、FAQ、事故报告、项目更新 | 写内部邮件/周报 |
| doc-coauthoring | `/doc-coauthoring` | 结构化共创文档 | 写技术方案/PRD |
| slack-gif-creator | `/slack-gif-creator` | 创建 Slack 优化 GIF | 做 Slack 表情 |

### Skill 工程

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| skill-creator | `/skill-creator` | 创建、修改、优化 skill 并评估性能 | 新建 skill 或改进现有 skill |

---

## baoyu

**定位**：内容创作与多媒体工具集。自媒体运营、图文创作、跨平台发布、视觉设计、翻译。

### 安装

推荐通过 `npx skills` 一键安装全部：

```bash
npx skills add jimliu/baoyu-skills
```

或在 Claude Code 插件市场安装：

```bash
/plugin marketplace add JimLiu/baoyu-skills
/plugin install baoyu-skills@baoyu-skills
```

也可以手动克隆后从 `skills/` 子目录复制：

```bash
git clone https://github.com/JimLiu/baoyu-skills.git ~/.claude/skills/baoyu-skills
```

```bash
# 项目级示例
cp -r ~/.claude/skills/baoyu-skills/skills/baoyu-translate your-project/.claude/skills/
cp -r ~/.claude/skills/baoyu-skills/skills/baoyu-diagram your-project/.claude/skills/

# 全局示例
cp -r ~/.claude/skills/baoyu-skills/skills/baoyu-translate ~/.claude/skills/
```

升级：在 Claude Code 插件市场里选择 **Update marketplace**，或重新运行 `npx skills add jimliu/baoyu-skills`。

### Skills

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| baoyu-translate | `/baoyu-translate` | 快速、标准、精细三档翻译，支持术语一致性 | 文章翻译 |
| baoyu-image-gen | `/baoyu-image-gen` | 多后端文生图，支持主流图像 API | 生成插画/配图 |
| baoyu-danger-gemini-web | `/baoyu-danger-gemini-web` | 通过 Gemini Web 生成图片和文本 | 低成本实验性生成 |
| baoyu-danger-x-to-markdown | `/baoyu-danger-x-to-markdown` | 将 X/Twitter 内容转为 Markdown | 保存推文/长文 |
| baoyu-cover-image | `/baoyu-cover-image` | 自动生成文章封面，支持多种比例 | 公众号/知乎/小红书封面 |
| baoyu-article-illustrator | `/baoyu-article-illustrator` | 分析文章结构并生成风格统一的配图 | 文章插图 |
| baoyu-comic | `/baoyu-comic` | 把知识点转为多格教育漫画 | 概念可视化 |
| baoyu-compress-image | `/baoyu-compress-image` | 压缩图片为 WebP/PNG | 优化图片体积 |
| baoyu-diagram | `/baoyu-diagram` | 生成专业 SVG 图，如流程图、时序图、架构图 | 画技术图 |
| baoyu-format-markdown | `/baoyu-format-markdown` | 自动整理 Markdown 标题、列表、代码块和排版 | 整理文档格式 |
| baoyu-infographic | `/baoyu-infographic` | 生成高密度信息图 | 复杂信息可视化 |
| baoyu-markdown-to-html | `/baoyu-markdown-to-html` | 将 Markdown 转为带样式 HTML | 发布网页/公众号前处理 |
| baoyu-post-to-wechat | `/baoyu-post-to-wechat` | 发布文章到微信公众号 | 发公众号文章 |
| baoyu-post-to-weibo | `/baoyu-post-to-weibo` | 发布微博或头条文章 | 发微博 |
| baoyu-post-to-x | `/baoyu-post-to-x` | 发布普通帖或长文到 X | 发推文 |
| baoyu-slide-deck | `/baoyu-slide-deck` | 根据内容生成幻灯片图片 | 做 PPT/演示稿 |
| baoyu-url-to-markdown | `/baoyu-url-to-markdown` | 抓取网页并转为 Markdown | 保存网页笔记 |
| baoyu-xhs-images | `/baoyu-xhs-images` | 小红书图文卡片生成，支持多种风格/布局/配色 | 小红书图文卡片 |
| baoyu-youtube-transcript | `/baoyu-youtube-transcript` | 下载 YouTube 字幕、章节和封面 | 视频笔记/字幕提取 |
| baoyu-electron-extract | `/baoyu-electron-extract` | 从 Electron 应用提取内容 | Electron 内容抓取 |
| baoyu-wechat-summary | `/baoyu-wechat-summary` | 微信公众号文章摘要/整理 | 公众号内容处理 |

---

## dbskill

**定位**：dontbesilent 商业诊断工具箱。创业诊断、内容创作、商业分析、执行力提升、多角色讨论。

### 安装

Claude Code 推荐通过插件市场安装：

```bash
claude plugin marketplace add dontbesilent2025/dbskill
claude plugin install dbs@dontbesilent-skills
```

通用安装方式（Claude Code / Codex / Cursor 等）：

```bash
npx -y skills add dontbesilent2025/dbskill -g --all
```

也可以手动克隆后从 `skills/` 子目录复制：

```bash
git clone https://github.com/dontbesilent2025/dbskill.git ~/.claude/skills/dbskill
```

```bash
# 项目级示例
cp -r ~/.claude/skills/dbskill/skills/dbs your-project/.claude/skills/
cp -r ~/.claude/skills/dbskill/skills/dbs-diagnosis your-project/.claude/skills/

# 全局示例
cp -r ~/.claude/skills/dbskill/skills/dbs ~/.claude/skills/
```

升级：

```bash
# 插件市场用户
claude plugin marketplace update dontbesilent-skills
claude plugin update dbs@dontbesilent-skills
/reload-plugins

# npx skills 用户
npx -y skills add dontbesilent2025/dbskill -g --all
```

### 诊断工具

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| dbs | `/dbs` | 听你描述商业问题，自动路由到最合适的诊断工具 | “帮我看看这个项目” |
| dbs-diagnosis | `/dbs-diagnosis` | 商业模式诊断：问诊消解问题，体检拆解模式环节 | “我这个生意模式对不对” |
| dbs-content | `/dbs-content` | 从结构、钩子、价值、表达、传播五维度诊断内容 | “怎么把这个选题做成好内容” |
| dbs-content-system | `/dbs-content-system` | 内容结构化系统，把大量本地素材搭成可持续生长的内容工程 | 想把旧内容变成可复用资产 |
| dbs-action | `/dbs-action` | 用阿德勒心理学分析拖延原因，找到卡住的点 | “知道该干嘛但就是动不起来” |
| dbs-benchmark | `/dbs-benchmark` | 五重过滤法筛选值得模仿的对象 | “我该学谁” |
| dbs-hook | `/dbs-hook` | 诊断视频开头问题并生成优化方案 | “前几秒留不住人” |
| dbs-ai-check | `/dbs-ai-check` | 扫描文案中的 AI 生成痕迹，输出检测报告 | “AI 写的会不会被看出来” |
| dbs-xhs-title | `/dbs-xhs-title` | 从验证过的爆款公式中挑选标题方向 | “标题怎么写才好” |
| dbs-slowisfast | `/dbs-slowisfast` | 找到看起来更慢但长期更快的方法 | “走捷径反而更慢” |
| dbs-deconstruct | `/dbs-deconstruct` | 用维特根斯坦和奥派经济学拆解商业概念 | “这个词到底是什么意思” |
| dbs-goal | `/dbs-goal` | 目标清晰化，把模糊目标审计成可检查的交付物 | 目标空转、无法驱动行动 |
| dbs-good-question | `/dbs-good-question` | 好问题生成器，把模糊问题改成 Agent 可推理、可验证的问题说明书 | 问题太松、需要写问题说明书 |
| dbs-decision | `/dbs-decision` | 个人决策系统，把长期跟踪领域做成本地知识工程 | 重大决策沉淀与复盘 |

### 学习工具

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| dbs-learning | `/dbs-learning` | 交互式学习，把课题拆成连续文章，根据反馈生成下一篇 | 系统学习一个主题 |

### 状态管理

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| dbs-save | `/dbs-save` | 把诊断关键结论、否决方向、下一步存到本地 | 一次诊断结束 |
| dbs-restore | `/dbs-restore` | 拉出上次的存档，新对话也能接着诊断 | 下次回来“接着上次” |
| dbs-report | `/dbs-report` | 把多次存档合并成带时间索引的 markdown 报告 | 存档累积、需要分享/归档 |

### 多角色讨论

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| dbs-chatroom | `/dbs-chatroom` | 模拟多角色专家对话，Claude 做裁判 | “听听不同专家的观点” |
| dbs-chatroom-austrian | `/dbs-chatroom-austrian` | 哈耶克、米塞斯、Claude 的奥派经济学对话 | “哈耶克和米塞斯怎么看” |

### Agent 基建

| Skill | 命令 | 功能 | 何时使用 |
|-------|------|------|---------|
| dbs-agent-migration | `/dbs-agent-migration` | 整理项目为 Claude/Codex/Grok 三端一致的 Agent 工作台 | “迁移到 Codex / 统一配置” |

---

## open-design

**定位**：开源 Claude Design 替代。本地优先、模型无关，适合需要视觉产出、交互原型、多格式导出（HTML/PDF/PPTX/MP4）的场景。

**注意**：不是纯 Claude Code skill，以独立桌面/Web 应用 + MCP 服务器形式运行。

### 安装

**推荐：下载桌面应用（零配置）**

- macOS（Apple Silicon / Intel）、Windows（x64）、Linux（AppImage）→ <https://open-design.ai/> 或 <https://github.com/nexu-io/open-design/releases>

**安装到 coding agent（无 UI）**

```bash
curl -fsSL https://open-design.ai/install.sh | sh -s <agent>
# agent = claude | codex | cursor | copilot | openclaw | antigravity | gemini | pi | vibe | hermes | cline | kimi | trae | opencode
```

**从源码运行**

```bash
git clone https://github.com/nexu-io/open-design.git ~/open-design
cd ~/open-design
corepack enable && pnpm install
pnpm tools-dev run web
```

**Docker**

```bash
git clone https://github.com/nexu-io/open-design.git ~/open-design
cd ~/open-design/deploy
cp .env.example .env
echo "OD_API_TOKEN=$(openssl rand -hex 32)" >> .env
docker compose up -d
# 打开 http://localhost:7456
```

升级：`cd ~/open-design && git pull && pnpm install`

### 数据（v0.9.0）

- 100+ skills
- 150 brand-grade `DESIGN.md` 设计系统
- 261 官方 plugins
- 21 个 coding-agent CLI（Claude Code、Codex、Cursor、Copilot、OpenClaw、Antigravity、Gemini、OpenCode、Qwen、Hermes、Kimi、Pi、Kiro、Kilo、Mistral Vibe、DeepSeek、Cline、Trae 等）

### 适用场景

- 需要给非技术人员看交互原型
- 需要导出 PPTX/PDF 直接交付
- 需要统一管理多套视觉风格（Design System）
- 需要 HyperFrames 动效/MP4 视频

### 与 Claude Code skill 的关系

- `anthropic/frontend-design`、`anthropic/canvas-design` → 适合 Claude Code 会话内直接生成代码
- `gstack/design-html`、`gstack/design-shotgun` → 适合工程化交付和批量生成
- `open-design` → 适合需要交互式预览、多格式导出、设计系统统一管理的场景

---

## agent-reach

**定位**：给 AI agent 装上“眼睛”，让它直接访问互联网。17 平台工具集合，覆盖搜索、社交媒体、代码仓库、招聘、视频、网页、RSS 等，零配置或只需本地 cookie。

### 安装

```bash
git clone --single-branch --depth 1 https://github.com/Panniantong/Agent-Reach.git ~/.claude/skills/agent-reach
```

升级：`cd ~/.claude/skills/agent-reach && git pull`

### 检查可用 channel

```bash
agent-reach doctor
```

### 平台分类

| 分类 | 覆盖平台 | 典型用途 |
|------|---------|---------|
| search | Exa 搜索 | 全网搜索、找资料 |
| social | 小红书/抖音/微博/推特/B站/V2EX/Reddit | 社媒内容、趋势、评论 |
| career | LinkedIn | 招聘、职位、人才 |
| dev | GitHub | 代码仓库、issue、PR、commit |
| web | 网页/公众号/文章/RSS | 读文章、抓网页、公众号 |
| video | YouTube/B站/小宇宙/播客 | 字幕、转录、视频笔记 |
| finance | 雪球/股票 | 行情、基金 |

### 常用命令

```bash
# 网页搜索
mcporter call 'exa.web_search_exa(query: "query", numResults: 5)'

# 通用网页阅读
curl -s "https://r.jina.ai/URL"

# GitHub 搜索
gh search repos "query" --sort stars --limit 10

# YouTube/B站字幕
yt-dlp --write-sub --skip-download -o "/tmp/%(id)s" "URL"
```

> 详细命令参考 skill 内的 `references/*.md`。

---

## grill-me

**定位**：来自 [mattpocock/skills](https://github.com/mattpocock/skills) 的计划/设计压力测试 skill。像设计评审一样逐条追问，把决策树的每个分支都走清楚，防止 AI 带着错误假设直接开写。

### 安装

```bash
git clone --single-branch --depth 1 https://github.com/mattpocock/skills.git ~/.claude/skills/mattpocock-skills
cp -r ~/.claude/skills/mattpocock-skills/grill-me ~/.claude/skills/
```

升级：`cd ~/.claude/skills/mattpocock-skills && git pull`

### 使用

在 Claude Code 中输入：

```
/grill-me
```

然后描述你的计划或设计，Claude 会一次问一个问题，给出推荐答案，并优先通过探索代码库来回答可验证的问题。

### 适合场景

- 准备实现一个复杂功能前，先对齐假设
- 设计评审时，把隐式依赖显式化
- 发现方案里还没考虑的边界情况

---

## cc-connect（飞书）

**定位**：本地 AI coding agent 与 IM 平台之间的桥接器，也是飞书所调用的底层开源项目。支持 Claude Code、Cursor、Codex、Gemini CLI 等 agent 接入飞书、钉钉、Slack、企业微信、Telegram、Discord 等。飞书使用 WebSocket 长连接，**无需公网 IP**。

> 注意：这里写的是飞书所调用的 [cc-connect](https://github.com/chenhg5/cc-connect) 开源项目，不是本仓库里的 `feishu` skill；skill 只是快捷启动入口。

### 安装

```bash
npm install -g cc-connect
```

升级：`npm install -g cc-connect@latest`

### 启动

```bash
# 命令行启动
cc-connect

# 或 Web 管理界面
cc-connect web
```

### 飞书配置要点

1. 创建飞书企业自建应用，开启“机器人”能力。
2. 订阅事件：`im.message.receive_v1`（接收消息）、`card.action.trigger`（卡片回调）。
3. 选择**长连接接收事件**（WebSocket），无需回调 URL。
4. 配置权限：`im:message`、`im:message:send_as_bot` 等。
5. 发布应用，把机器人加入目标会话。
6. 编辑 `~/.cc-connect/config.toml`，填入 `app_id`、`app_secret`、`encrypt_key`。

### 常用 IM 命令

| 命令 | 作用 |
|------|------|
| `/shell <命令>` | 在终端执行命令 |
| `/restart` | 重启机器人 |
| `/upgrade` | 升级版本 |
| `/commands` | 查看所有命令 |
| `/whoami` | 获取你的用户 ID |

### 故障排查

| 问题 | 解决 |
|------|------|
| 机器人不回复 | 重新运行启动命令，检查进程 |
| 权限被拒 | `/whoami` 获取 ID，填入 `config.toml` 的 `allow_from` |
| 启动报错 | 查看 `~/.cc-connect/cc-connect.log` |

---

## 项目类型推荐

### 前端 Web 项目

| 来源 | 推荐 Skill | 用途 |
|------|-----------|------|
| Superpowers | brainstorming, writing-plans, test-driven-development, verification-before-completion | 零基础学习型 Web coding 主线 |
| OpenSpec | propose, explore, apply, verify, archive | 需求拆分、验收标准、任务追踪、实现后规格校验 |
| gstack | ship, review, qa, benchmark, design-html, design-review | 发版、审查、测 bug、性能回归、设计图转代码、像素级 QA |
| anthropic | frontend-design, webapp-testing, web-artifacts-builder | 高质量前端代码、Playwright 测试、React 复杂组件 |
| open-design | —（独立应用） | 交互原型、幻灯片、海报、动效 |

### Python / Go / Java / Rust 后端项目

| 来源 | 推荐 Skill | 用途 |
|------|-----------|------|
| OpenSpec | propose, apply, verify, archive | 需求边界、API 行为、验收标准、实现追踪 |
| gstack | ship, review, cso, health, investigate | 发版、审查、安全审计、质量评分、系统性查 bug |
| anthropic | claude-api, mcp-builder | API 应用开发、MCP 服务器构建 |

### 内容创作 / 自媒体 / 写作项目

| 来源 | 推荐 Skill | 用途 |
|------|-----------|------|
| baoyu | translate, diagram, infographic, cover-image, post-to-wechat, post-to-x, markdown-to-html | 翻译、SVG 图表、信息图、封面、发布、转网页 |
| dbskill | dbs, dbs-content, dbs-hook, dbs-xhs-title, dbs-content-system | 商业工具箱、内容诊断、短视频开头、小红书标题、内容资产化 |
| anthropic | docx, pdf, pptx | Word、PDF、幻灯片处理 |
| agent-reach | search, social, web, video | 全网搜资料、读文章、抓社媒内容、做视频笔记 |

### 商业 / 创业 / 分析项目

| 来源 | 推荐 Skill | 用途 |
|------|-----------|------|
| dbskill | dbs, dbs-diagnosis, dbs-benchmark, dbs-action, dbs-chatroom, dbs-decision | 商业工具箱、模式诊断、对标分析、执行力、多角色对话、决策系统 |
| gstack | plan-ceo-review, office-hours, retro | CEO 视角审查计划、YC 式追问、工程复盘 |
| OpenSpec | explore, propose | 把模糊想法转成可执行 change 和验收标准 |
| grill-me | /grill-me | 对计划或设计进行压力测试，防止假设漂移 |

### IM 集成 / 远程触发

| 来源 | 推荐 | 用途 |
|------|------|------|
| cc-connect | —（独立 CLI） | 从飞书/钉钉/Slack/企微等 IM 远程调用本地 AI agent |

### 通用 / 个人知识库

默认先询问用户具体需求，再推荐 `CLAUDE.md`、Superpowers、OpenSpec 或具体 skill。学习型开发优先 Superpowers，复杂功能变更治理优先 OpenSpec，交付型开发优先 gstack。

---

## 项目级配置说明

权限、MCP 和全局 skill 配置以当前工具环境为准。这个 repository 主要作为本机 skill 安装索引和推荐池。

### 建议全局保留权限

```json
{
  "permissions": {
    "allow": [
      "Bash",
      "Read",
      "Write",
      "Edit",
      "Glob",
      "Grep",
      "Agent",
      "WebFetch",
      "WebSearch",
      "Skill"
    ]
  }
}
```

### CLAUDE.md / AGENTS.md（项目级）

`CLAUDE.md` 或 `AGENTS.md` 是项目级编码行为规范，建议随项目入版本库。复制时优先保留项目自己的目标、技术栈、学习方式和验收标准。

示例：让 Claude Code 使用 gstack 的 `/browse` 做网页浏览：

```markdown
## gstack
Use /browse from gstack for all web browsing. Never use mcp__claude-in-chrome__* tools.
Available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /review, /ship,
/qa, /cso, /investigate, /design-shotgun, /design-html, /autoplan.
```

### 注意事项

- 公共项目不要提交 API key、token、Cookie 或私有 MCP 配置。
- 高敏感项目可以用项目级配置缩小命令权限和可编辑目录。
- 学习型项目不要默认启用“一步做完”的自动化 skill；优先让 Superpowers 带着你澄清、拆计划、测试和验证，再逐步实现。

---

## 本仓库保留内容

- `learning-agent/` — AI Agent 学习资料与笔记（本地维护）
- `CLAUDE.md` — AI 编码行为规范
- `PRD.txt` — 历史产品需求文档

> 来源：从本机全局 skill 与桌面仓库整理而来。skill 安装方式会随上游仓库更新，请以上游 README 为准。
