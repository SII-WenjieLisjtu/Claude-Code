# 整体架构总览

这篇文档回答一个核心问题：**Claude Code 到底是由哪些层拼起来的？**

## 一、先看分层

```text
用户入口
  └─ Terminal / Slash Commands / IDE / Remote
      └─ 入口与会话编排
          └─ QueryEngine
              ├─ Commands
              ├─ Tools
              ├─ Permissions / Plan Mode / Tasks
              ├─ Bridge / Remote / Swarm
              └─ 外部系统：FS / Shell / Web / MCP / IDE
```

## 二、入口层

关键文件：
- `src/entrypoints/cli.tsx`
- `src/main.tsx`
- `src/commands.ts`

这一层负责：
- 识别启动模式
- 注册 slash commands
- 决定是进入普通 REPL、远程模式、后台会话还是 bridge 相关路径

这里最值得注意的是：Claude Code 不是一个单入口程序，而是**多条 fast-path 启动路径**组成的入口系统。

## 三、调度层：QueryEngine

关键文件：
- `src/QueryEngine.ts`
- `src/Tool.ts`

这一层负责：
- 管消息
- 管上下文
- 管工具调用循环
- 管 token/cost
- 管权限拒绝与状态回写

可以把它理解成 Claude Code 的运行时总调度器。

## 四、能力层：Commands 与 Tools

关键文件：
- `src/commands.ts`
- `src/tools.ts`
- `src/commands/**`
- `src/tools/**`

这里的核心思想是：

- **Commands 是给人用的入口**
- **Tools 是给模型用的执行接口**

这是一种很重要的解耦。

## 五、控制层：Permissions / Plan Mode / Worktree

关键文件：
- `src/Tool.ts`
- `src/tools/EnterPlanModeTool/**`
- `src/tools/ExitPlanModeTool/**`
- `src/tools/EnterWorktreeTool/**`
- `src/tools/ExitWorktreeTool/**`
- `src/utils/permissions/**`

这一层回答的是：模型可以做什么、什么时候能做、在哪个边界内做。

## 六、协作层：Bridge / Remote / Swarm

关键文件：
- `src/bridge/**`
- `src/remote/**`
- `src/coordinator/**`
- `src/utils/swarm/**`
- `src/tools/TeamCreateTool/**`

这一层说明 Claude Code 已经不是单点 CLI，而是一个可跨端、可多代理协作的系统。

## 七、外部系统层

关键连接对象包括：
- 文件系统
- Shell
- Git
- Web
- MCP Servers
- IDE
- 认证系统

这也是 Claude Code 真正与“真实工作环境”接轨的地方。

## 八、最值得记住的架构判断

Claude Code 的价值不在于“功能很多”，而在于：

> 它把入口、调度、执行、权限、协作和外部连接组织成了一个清晰分层的系统。

读懂这一点，比记住多少工具名更重要。