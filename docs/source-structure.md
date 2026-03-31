# 源码结构导读

这篇文档只回答一个问题：`src/` 里每一层大概是做什么的。

## 顶层结构

```text
src/
├── entrypoints/     # CLI / SDK / MCP 等入口
├── commands/        # slash commands
├── tools/           # 模型可调用的工具
├── bridge/          # IDE / Remote / Bridge 通信
├── remote/          # 远程会话管理
├── coordinator/     # 多 Agent 协调模式
├── tasks/           # 各种任务执行器
├── services/        # API、MCP、compact、oauth、analytics 等服务层
├── memdir/          # 持久记忆系统
├── state/           # 全局状态
├── components/      # Ink UI 组件
├── screens/         # REPL/全屏界面
├── query/           # QueryEngine 相关子逻辑
├── utils/           # 各类运行时工具函数
├── skills/          # Skills 体系
├── plugins/         # 插件体系
├── buddy/           # companion/buddy 能力
├── voice/           # 语音模式
└── vim/             # Vim 模式
```

## 如何理解这些目录

### `entrypoints/`
决定从哪里进入系统。这里最重要的是 `cli.tsx`。

### `commands/`
放的是用户显式输入的能力入口，也就是 `/xxx` 命令。

### `tools/`
放的是模型执行任务时真正调用的能力单元。

### `query/` + `QueryEngine.ts`
负责一次对话轮次如何变成“模型响应 + 工具调用 + 状态更新”的完整过程。

### `services/`
负责连接外部世界，比如 API、MCP、OAuth、analytics、compact。

### `bridge/` 与 `remote/`
负责把本地 CLI、网页、IDE、远程会话串起来。

### `coordinator/`、`tasks/`、`utils/swarm/`
负责多 Agent 团队、任务分发与协作细节。

### `memdir/`
负责持久记忆。这里能看到 MEMORY.md 为什么会成为整个系统的一部分。

## 建议怎么读

如果你不熟悉这个仓库，先不要逐目录全部展开。推荐顺序是：

1. `entrypoints/`
2. `commands.ts`
3. `tools.ts`
4. `QueryEngine.ts`
5. 再根据兴趣回来看 `bridge/`、`services/`、`coordinator/`

先抓主干，再看分支。