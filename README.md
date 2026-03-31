# Claude Code 源码快照深度解析（中文重写版）

> 这是一个面向**教育、架构研究、供应链安全分析与防御性安全研究**的 Claude Code 源码快照仓库。
>
> 它并非 Anthropic 官方仓库，而是基于一次 **npm 发布产物中 source map 暴露**后可公开访问到的 TypeScript 源码快照整理而成，适合用来系统研究：**现代 Agentic CLI 到底是如何构建出来的。**

---

## 一句话看懂这个仓库

如果你一直在用 Claude Code、Cursor、Copilot 或各类 AI 编程助手，但总想知道：

- 一个真正可落地的 AI 编程 CLI 内部是怎么组织的？
- 工具调用、权限控制、Slash Commands、MCP、Hooks、多 Agent 协作是怎么串起来的？
- 为什么这类系统既像终端工具，又像一个微型操作系统？

那么这个仓库，就是一份非常难得的**真实工程样本**。

它不是“玩具 demo”，也不是“概念验证”。从源码规模、模块拆分、能力边界到运行路径，这份快照展示的是一个已经高度工程化的 Agentic 开发环境实现。

---

## 仓库定位

本仓库维护的是一份 Claude Code 的**公开可访问源码快照镜像**，主要用于：

- 教育学习
- 防御性安全研究
- 软件供应链暴露分析
- Agentic Developer Tooling 架构研究
- 打包与发布流程失误的案例分析

**不用于：**

- 冒充官方源码仓库
- 宣称原始代码所有权
- 提供任何对原产品的恶意利用建议

原始 Claude Code 相关知识产权归 **Anthropic** 所有。

---

## 研究背景：这份源码为什么会公开可见？

2026-03-31，一份发布到 npm 的构建产物中暴露了 source map 线索，进一步指向了未混淆的 TypeScript 源码快照位置，使得 `src/` 目录可被公开下载。

这件事本身就是一个很典型、也很值得学习的安全案例：

- **构建产物**可能泄露源码结构信息
- **source map** 可能成为敏感元数据出口
- **对象存储中的构建中间产物**如果可公开访问，会带来意料之外的信息暴露
- **CLI 工具链的供应链安全**不仅是依赖问题，也包括发布流程与制品治理问题

因此，这个仓库既是源码阅读材料，也是一个现实世界的**软件供应链与发布安全案例**。

---

## 为什么这个仓库值得认真读一遍

这份快照最有价值的地方，不只是“看到了源码”，而是它非常集中地展示了一个成熟 AI 工程助手系统的关键设计问题：

### 1. 它不是单纯的聊天工具
Claude Code 在架构上并不是“终端里的聊天窗口”，而是一个能协调：

- 文件系统
- Shell
- 搜索工具
- 外部网页
- MCP 服务
- IDE
- 多 Agent
- 权限系统
- 计划审批流
- 会话持久化与记忆

的**任务执行系统**。

### 2. 它把“大模型调用”嵌进了工程系统，而不是反过来
从 `src/QueryEngine.ts`、`src/tools.ts`、`src/commands.ts` 可以看出，模型并不是孤立存在的。真正驱动系统的是：

- 查询循环
- 工具注册表
- 命令调度
- 权限检查
- 上下文构造
- 状态同步
- UI 渲染

也就是说，大模型只是其中一个核心计算节点，整个 CLI 更像一个**围绕模型构建的可控执行环境**。

### 3. 它提供了非常完整的 Agentic CLI 工程范式
如果你正想自己做：

- AI 编程助手
- Agent 框架
- 工具调用系统
- 团队协作型代理
- CLI + IDE 双端协同产品

那么这份仓库几乎可以当成一份**架构范本样本**来研究。

---

## 这份快照大致包含什么

当前仓库是一个**源码镜像快照**，根目录非常简洁：

```text
claude-code-main/
├── README.md
└── src/
```

也就是说，它更接近“源码层快照”，而不是完整的官方开发仓库。当前快照中**没有完整的构建与发布元文件**（例如这里未见到常规根级 `package.json`），因此它更适合：

- 阅读架构
- 分析模块
- 提炼设计模式
- 做文档化解释

而不是直接拿来一键构建运行。

---

## 从源码规模看，这不是小项目

根据当前快照与源码目录结构，这个项目具备如下特征：

- 语言：**TypeScript**
- 运行时方向：**Bun**
- 终端 UI：**React + Ink**
- CLI 解析：**Commander.js**
- Schema 验证：**Zod v4**
- 代码搜索：**ripgrep**
- 协议层：**MCP SDK + LSP**
- 认证与会话：**OAuth 2.0 / JWT / Keychain**
- 源码规模：约 **1,900+ 文件 / 50 万行级别代码**
- 模块复杂度：覆盖 CLI、工具系统、桥接层、远程控制、权限体系、计划模式、工作树、MCP、团队协作、任务编排等

换句话说，这不是“AI 调个 API 再包一层命令行”的项目，而是一个**系统级工程产品**。

### 一眼看懂技术栈

| 类别 | 技术 |
|---|---|
| Runtime | Bun |
| Language | TypeScript（严格模式） |
| Terminal UI | React + Ink |
| CLI Parsing | Commander.js |
| Schema | Zod v4 |
| Search | ripgrep |
| Protocol | MCP SDK、LSP |
| API | Anthropic SDK |
| Telemetry | OpenTelemetry + gRPC |
| Feature Flags | GrowthBook + `bun:bundle` |
| Auth | OAuth 2.0、JWT、macOS Keychain |

---

## 推荐阅读路线

如果你不想一上来就淹没在大量目录里，建议按下面顺序看：

### 第一层：先理解入口与总调度
- `src/entrypoints/cli.tsx`
- `src/main.tsx`
- `src/commands.ts`
- `src/tools.ts`
- `src/QueryEngine.ts`
- `src/Tool.ts`

这几份文件基本定义了：

- CLI 如何启动
- 命令如何注册
- 工具如何暴露给模型
- 查询循环如何驱动整个交互过程
- 工具上下文与权限模型如何组织

### 第二层：看“能力系统”
- `src/commands/**`
- `src/tools/**`
- `src/services/**`
- `src/bridge/**`

这一层会告诉你：Claude Code 的功能并不是堆在一个入口文件里，而是拆成了命令、工具、服务和桥接协议四层。

### 第三层：看“高级控制能力”
- `src/tools/EnterPlanModeTool/**`
- `src/tools/ExitPlanModeTool/**`
- `src/tools/TeamCreateTool/**`
- `src/tools/EnterWorktreeTool/**`
- `src/tools/ExitWorktreeTool/**`
- `src/coordinator/**`
- `src/utils/swarm/**`
- `src/bridge/**`

这一层最值得研究，因为它体现的是 Claude Code 和普通 AI 助手拉开差距的地方：

- 计划模式
- 多 Agent 团队
- Worktree 隔离
- 远程桥接
- 会话恢复
- 复杂状态管理

---

## 源码结构导读

下面是对当前 `src/` 的核心目录导读。

```text
src/
├── main.tsx                    # 主 CLI/REPL 初始化逻辑
├── entrypoints/cli.tsx         # 真正的入口分发器，处理 fast-path 与特殊子命令
├── commands.ts                 # 所有 slash commands 的注册中心
├── tools.ts                    # 所有工具的注册中心
├── Tool.ts                     # 工具类型、工具上下文、权限上下文等核心抽象
├── QueryEngine.ts              # 查询引擎核心：消息、工具调用循环、用量跟踪、上下文整合
│
├── commands/                   # Slash 命令实现
├── tools/                      # 工具实现
├── services/                   # API / MCP / auth / analytics / memory / compact 等服务层
├── bridge/                     # IDE 与 remote bridge 双向通信层
├── coordinator/                # 多 Agent 协调逻辑
├── utils/swarm/                # swarm / teammate / team 运行辅助能力
├── tasks/                      # 任务系统 UI 与状态
├── state/                      # AppState 与状态更新相关实现
├── query/                      # 查询过程辅助逻辑
├── memdir/                     # 记忆目录与记忆注入
├── skills/                     # Skills 相关实现
├── plugins/                    # 插件系统
├── cli/                        # CLI 输出、传输、结构化 IO 等
├── buddy/                      # Buddy / Companion sprite 相关能力
├── voice/                      # 语音模式相关能力
└── vim/                        # Vim 模式相关能力
```

---

## 核心架构亮点

下面这些点，是这份源码最值得写进 README、也最值得认真研究的部分。

### 1. CLI 启动不是“一个入口”，而是一组 Fast Path 分发器
从 `src/entrypoints/cli.tsx` 可以看到，Claude Code 在正式加载完整 CLI 之前，会先检查多种特殊路径：

- `--version`
- dump system prompt
- chrome/native host 相关入口
- remote-control / bridge 模式
- daemon 模式
- background session 模式
- worktree + tmux 快速路径
- self-hosted runner / environment runner

这意味着它把启动过程做成了一个**按场景拆分的多入口调度层**，而不是把所有逻辑都塞进主进程启动后再判断。

这种设计的好处是：

- 冷启动更快
- 某些模式几乎不需要加载整套 UI
- 功能边界清晰
- 更利于 feature flag + dead code elimination

### 2. 命令系统与工具系统是分层的
`src/commands.ts` 和 `src/tools.ts` 显示了两个非常清晰的概念：

#### 命令（Commands）
命令是用户显式输入的 `/xxx` 交互入口，例如：

- `/help`
- `/doctor`
- `/compact`
- `/memory`
- `/mcp`
- `/tasks`
- `/review`
- `/vim`
- `/status`
- `/theme`

它更偏向“面向用户的产品能力”。

#### 工具（Tools）
工具是模型在执行过程中可调用的能力单元，例如：

- Bash
- Read / Write / Edit
- Glob / Grep
- WebFetch / WebSearch
- Agent
- Skill
- TeamCreate
- SendMessage
- EnterPlanMode / ExitPlanMode
- EnterWorktree / ExitWorktree
- TaskCreate / TaskUpdate / TaskList

它更偏向“面向模型的执行能力”。

这是一个很关键的架构思想：

> **命令是人机交互入口，工具是模型执行接口。**

很多 AI 工具会把二者混在一起，而 Claude Code 的这份源码把这两个层次区分得很清楚。

### 3. QueryEngine 才是真正的心脏
`src/QueryEngine.ts` 是整个系统最值得重点阅读的文件之一。

它负责的不是单一 API 请求，而是完整的会话查询生命周期，包括：

- 消息维护
- 工具调用循环
- 系统 prompt 拼装
- memory prompt 注入
- token / cost 跟踪
- 文件状态缓存
- 权限拒绝处理
- 紧凑化（compact / snip）相关机制
- SDK/会话消息输出映射

如果说 `tools.ts` 提供了“手和脚”，那么 `QueryEngine.ts` 提供的就是“大脑里的执行回路”。

### 4. Tool 抽象做得非常完整
`src/Tool.ts` 不只是一个简单接口定义文件，它实际承载了很多 Claude Code 的工程抽象：

- Tool schema
- Tool permission context
- ToolUseContext
- AppState 读写接口
- notification / JSX / OS notification 能力
- compact progress 事件
- tool progress 数据类型
- message / SDK 映射边界

从这里可以看出 Claude Code 的工具系统不是“函数表”，而是一套真正的**运行时协议层**。

### 5. 权限模型不是附加功能，而是核心机制
阅读 `Tool.ts`、相关 permissions 类型与 `EnterPlanModeTool` 这类实现会发现：

Claude Code 的权限系统并不是事后补上的弹窗，而是深深嵌在工具调用协议里的。

它关注的问题包括：

- 当前模式是不是 `plan`
- 当前工具是否需要 defer
- 当前调用是否允许自动批准
- 是否避免权限弹窗
- 子 agent 能否触发某类行为
- 计划模式能否退出

这就是为什么 Claude Code 能支持：

- 默认模式
- 自动模式
- 计划模式
- 子 Agent 权限隔离
- 背景任务限制
- 用户确认流

本质上，它在做的是：

> **把“模型能不能做某件事”变成一个系统级约束，而不是 prompt 里的约定俗成。**

### 6. Plan Mode 是非常强的产品设计
`src/tools/EnterPlanModeTool/EnterPlanModeTool.ts` 展示了一个很有代表性的能力：

复杂任务不是直接执行，而是先进入 plan mode，要求模型：

1. 先探索代码
2. 先理解上下文
3. 先写计划
4. 再请求用户批准
5. 批准后才进入实现阶段

这相当于把“先想清楚再写”做成了产品级工作流，而不是单纯依赖模型自觉。

对于复杂工程任务，这是一个非常关键的可控性设计。

### 7. 多 Agent 团队能力是原生内建的
从 `src/tools/TeamCreateTool/TeamCreateTool.ts`、`src/tools.ts`、`src/coordinator/**`、`src/utils/swarm/**` 可以看到：

多 Agent 并不是外挂，而是 Claude Code 内部原生的能力模型。

它支持的东西包括：

- TeamCreate / TeamDelete
- SendMessage
- 共享任务列表
- teammate metadata
- session/team file 持久化
- leader / teammate 角色分工
- 任务目录与团队目录自动管理

这意味着 Claude Code 并不是只有“一个 agent + 一组工具”，而是已经具备：

> **把多个代理作为团队来协调的产品抽象。**

### 8. Bridge 层说明它不是纯终端工具
`src/bridge/bridgeMain.ts` 与相关 bridge 模块非常长，也非常关键。

它揭示了 Claude Code 的另一个本质：

> 它不是只运行在终端里，而是在终端、IDE、remote control、session bridge 之间做统一编排。

Bridge 层涉及：

- remote-control 会话
- session spawning
- token / trusted device
- heartbeat
- work item 生命周期
- 多 session / bridge polling
- 环境注册
- 远程工作执行与状态回传

也就是说，Claude Code 的很多能力并不是“本地 CLI 独享”，而是设计成了**跨端协同系统**。

### 9. Feature Flags 的使用非常工程化
从 `src/commands.ts`、`src/tools.ts`、`src/entrypoints/cli.tsx`、`src/QueryEngine.ts` 可反复看到 `feature('...')`。

它不是装饰性开关，而是和 Bun 的构建能力配合，实现：

- 条件引入
- 编译期 dead code elimination
- 内部能力与外部构建分层
- runtime gate 与 compile-time gate 协同

典型能力开关包括：

- `BRIDGE_MODE`
- `DAEMON`
- `VOICE_MODE`
- `KAIROS`
- `PROACTIVE`
- `COORDINATOR_MODE`
- `AGENT_TRIGGERS`
- `WORKFLOW_SCRIPTS`
- `HISTORY_SNIP`

这说明项目在“实验能力、商业能力、内部能力、外部发布能力”之间，有非常成熟的拆分方式。

### 10. Bridge、Swarm 与权限委托让它更像一个“分布式开发操作台”
结合 `src/bridge/**`、`src/coordinator/**`、`src/utils/swarm/**` 可以看到，Claude Code 不只是本地单 agent 工具，而是具备明显的分布式协作倾向：

- **Bridge 双传输层**：支持通过远程会话把网页/移动端操作桥接回本地 REPL
- **Coordinator + Worker 多智能体架构**：协调者负责研究、综合、实施、验证四阶段工作流
- **文件邮箱式跨进程通信**：team 成员通过持久化 inbox 交换消息与权限请求
- **Leader 权限委托**：worker 的高风险动作统一回到 leader UI 审批
- **Worktree 隔离**：每个 teammate 可运行在独立 git worktree 中，降低并发改动互相污染的风险

这一层能力使 Claude Code 更接近“可编排的智能工程工作台”，而不只是“能调工具的大模型壳”。

---

## 你能从这个项目学到什么

### 对 AI 产品工程师
你能学到如何把一个大模型应用从“对话工具”升级成“任务执行系统”。

### 对做 CLI 的开发者
你能学到终端产品如何同时兼顾：

- 交互体验
- 模块边界
- 执行权限
- 状态持久化
- 扩展能力

### 对安全研究者
你能学到：

- source map 暴露带来的真实风险
- 构建制品泄露如何放大攻击面
- 发布流程为什么本身就是供应链的一部分

### 对做 Agent 框架的人
你能学到：

- 工具系统怎么抽象
- Agent 如何与工具解耦
- 团队协作如何原生建模
- 如何避免“Agent 只是一个 for-loop 套 API”

---

## 关键文件速读

### `src/entrypoints/cli.tsx`
建议把它当作“CLI 启动分发总表”来读。

你会看到：

- 哪些能力走 fast-path
- 哪些模式根本不需要完整启动所有模块
- remote / daemon / bg session / bridge 等模式是如何切开的

### `src/commands.ts`
建议把它当作“产品功能目录”来读。

它告诉你：

- Claude Code 面向用户到底暴露了哪些 slash commands
- 哪些命令是内置命令
- 哪些能力是 feature-gated
- skills / plugins / workflows / builtin commands 如何汇合成同一套命令表

### `src/tools.ts`
建议把它当作“模型可调用能力总表”来读。

它告诉你：

- 模型到底能调用哪些工具
- 哪些工具是默认可用
- 哪些能力依赖环境或 feature flag
- 团队、多任务、cron、browser、workflow、MCP 等工具是怎么挂进来的

### `src/Tool.ts`
建议把它当作“运行时协议定义”来读。

这里是理解整个系统为什么能稳定工作的关键。

### `src/QueryEngine.ts`
建议花时间精读。

如果你只能挑一个大文件研究，我会优先推荐它。

### `src/bridge/bridgeMain.ts`
如果你想理解 IDE / remote control / 跨端桥接，就看这个文件。

### `src/tools/TeamCreateTool/TeamCreateTool.ts`
如果你想理解多 Agent 团队是怎么落到真实文件结构与任务系统上的，这个文件非常关键。它直接揭示了 team config、leader session、teammate 元数据、共享任务目录是如何建立的。

### `src/tools/EnterPlanModeTool/EnterPlanModeTool.ts`
如果你想理解“复杂任务为什么要先规划、再审批、再执行”，这个文件是最佳切入口。它体现了 Claude Code 非常重要的产品哲学：先建立可控流程，再把执行权交给模型。

---

## 这个仓库最吸引人的地方，不是“泄露”，而是“工程密度”

互联网上很多所谓“源码分析”，最后只是截图和情绪输出。

但这份快照真正值得花时间的原因在于：

1. **它信息密度极高**
2. **它展示的不是概念，而是产品级实现**
3. **它把 AI Agent、终端工具、远程控制、权限系统、计划模式、团队协作放在了同一个工程体系里**
4. **它能帮助你建立对下一代开发工具形态的更真实理解**

如果你真的在做 AI 编程产品，这类代码比十篇“AI Agent 趋势分析”有用得多。

---

## 使用建议

这份仓库更适合以下阅读方式：

### 方式一：按模块主题读
- CLI 启动
- 命令系统
- 工具系统
- 权限体系
- 多 Agent 协作
- Bridge / Remote

### 方式二：按用户体验倒推
从用户在终端里的一次操作出发，顺着链路看：

1. 输入命令
2. 命令路由
3. QueryEngine 处理
4. Tool 调用
5. Permission 决策
6. 状态更新
7. UI 回显

这种读法特别适合建立整体心智模型。

### 方式三：按源码角色读
- `entrypoints/`：谁负责启动
- `commands/`：谁负责用户入口
- `tools/`：谁负责执行能力
- `services/`：谁负责外部依赖
- `bridge/`：谁负责跨端协同
- `state/`：谁负责生命周期与状态一致性

---

## 适合谁阅读

这个仓库特别适合：

- 想做 AI Coding Agent 的开发者
- 想研究 Agentic CLI 架构的人
- 想研究 MCP/LSP/Tool Use 体系的人
- 对 IDE 与终端协同架构感兴趣的人
- 做安全研究、供应链安全、打包发布安全分析的人
- 想系统提升自己 AI 工具工程能力的学生和工程师

---

## 重要说明

### 1. 这不是 Anthropic 官方仓库
请不要将本仓库误认为官方开源仓库。

### 2. 这是一份研究快照
当前内容更接近源码镜像与研究材料，不保证具备完整构建上下文。

### 3. 请把它当作学习和分析材料
最有价值的用法不是“试图复原原始产品”，而是：

- 学架构
- 学工程组织方式
- 学产品化能力设计
- 学安全边界与供应链教训

---

## 建议你接下来怎么读

如果你刚进入这个仓库，我建议：

### 第一步
先看：
- `src/entrypoints/cli.tsx`
- `src/commands.ts`
- `src/tools.ts`

### 第二步
再看：
- `src/Tool.ts`
- `src/QueryEngine.ts`

### 第三步
最后按兴趣分支深入：
- 多 Agent：`src/tools/TeamCreateTool/**`、`src/coordinator/**`、`src/utils/swarm/**`
- 计划模式：`src/tools/EnterPlanModeTool/**`、`src/tools/ExitPlanModeTool/**`
- 远程协作：`src/bridge/**`
- 任务系统：`src/tasks/**`
- 记忆与上下文：`src/memdir/**`、`src/query/**`

---

## 致谢与引用背景

这份快照的研究背景来自公开可见的发布产物暴露事件与公开讨论。维护本仓库的目的，是帮助更多人从**工程、产品与安全**三个角度，认真理解这一代 AI 开发工具的内部结构。

如果你在研究中使用本仓库，建议重点关注两个主题：

1. **Agentic CLI 的系统架构**
2. **构建制品暴露与供应链安全治理**

---

## 最后

如果你只是把它当成“某个工具的源码泄露”，你会很快失去兴趣。

但如果你把它当成：

- 一份现代 AI 编程系统的真实横切面
- 一份高复杂度 CLI 产品的架构样本
- 一次值得反复复盘的供应链安全案例

那它就非常值得完整读一遍。

---

## 免责声明

- 本仓库内容仅用于**教育、研究、架构分析与防御性安全研究**。
- 原始 Claude Code 相关权利归 **Anthropic** 所有。
- 本仓库**不隶属于**、**不代表**、也**不由** Anthropic 维护。
- 请勿将此仓库用于任何恶意目的。
