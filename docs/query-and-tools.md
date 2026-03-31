# QueryEngine 与工具系统详解

如果只能选一个模块精读，我最推荐 `src/QueryEngine.ts`。

## 一、为什么 QueryEngine 是核心？

因为它负责的不是“调一次模型”，而是整轮交互的生命周期：

- 用户输入如何进入系统
- 系统 prompt 如何拼装
- 工具调用如何循环发生
- 每一步结果如何回到消息流里
- 权限拒绝如何处理
- token 与 cost 如何累计
- 会话状态如何在多轮之间保留

## 二、关键文件

- `src/QueryEngine.ts`
- `src/Tool.ts`
- `src/tools.ts`
- `src/commands.ts`
- `src/cli/structuredIO.ts`

## 三、工具系统的核心思想

Claude Code 里的工具不是简单函数，而是一套运行时协议。

从 `src/Tool.ts` 能看出，每个工具至少牵涉：
- 输入 schema
- 输出 schema
- 权限上下文
- 执行上下文
- 进度状态
- UI 映射
- 消息映射

这意味着工具系统不仅决定“能做什么”，也决定“怎么被安全地做”。

## 四、命令和工具的分工

### Commands
是用户可见的 `/xxx` 能力入口。

### Tools
是模型在执行时可调用的能力单元。

这种分层有两个直接好处：
1. 用户交互和模型执行不会混在一起
2. 系统可以分别演进产品层和执行层

## 五、常见工具类别

从 `src/tools.ts` 看，核心工具大致分成几类：

### 文件与代码类
- Read
- Write
- Edit
- Glob
- Grep
- NotebookEdit

### 执行与环境类
- Bash
- LSP
- MCP
- WebFetch
- WebSearch

### 协作与控制类
- Agent
- Skill
- TeamCreate
- SendMessage
- TaskCreate / TaskUpdate / TaskList
- EnterPlanMode / ExitPlanMode
- EnterWorktree / ExitWorktree

## 六、真正值得学的不是“工具列表”，而是“工具治理”

Claude Code 最成熟的地方，在于它并不把工具能力直接裸露给模型，而是通过：

- schema
- permission
- mode
- task system
- worktree isolation
- user approval

去约束工具的实际使用方式。

所以真正值得学习的是：

> **模型能力必须被工程制度化。**

## 七、建议阅读顺序

1. `src/tools.ts`
2. `src/Tool.ts`
3. `src/QueryEngine.ts`
4. `src/tools/AgentTool/**`
5. `src/tools/EnterPlanModeTool/**`
6. `src/tools/ExitPlanModeTool/**`

这样读，最容易把“工具”从功能列表读成系统机制。