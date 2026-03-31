# 源码阅读指南

如果你第一次打开这个仓库，最容易遇到的问题不是“看不懂某一行代码”，而是“不知道从哪开始”。

所以这篇文档只做一件事：给你一个不容易迷路的阅读路径。

## 路线 A：想快速建立整体认知

按这个顺序读：

1. `src/entrypoints/cli.tsx`
2. `src/main.tsx`
3. `src/commands.ts`
4. `src/tools.ts`
5. `src/QueryEngine.ts`
6. `src/Tool.ts`

这一套能帮你建立完整主线。

## 路线 B：你是程序员，想看系统设计

### 入口与调度
- `src/entrypoints/cli.tsx`
- `src/main.tsx`
- `src/QueryEngine.ts`

### 产品层
- `src/commands.ts`
- `src/commands/**`

### 执行层
- `src/tools.ts`
- `src/Tool.ts`
- `src/tools/**`

### 协作与远程
- `src/bridge/**`
- `src/remote/**`
- `src/coordinator/**`
- `src/utils/swarm/**`

### 权限与控制
- `src/utils/permissions/**`
- `src/tools/EnterPlanModeTool/**`
- `src/tools/ExitPlanModeTool/**`
- `src/tools/EnterWorktreeTool/**`

## 路线 C：你不是程序员，但想看懂它的思想

建议只抓三个问题：

1. 用户是怎么和系统交互的？
2. 模型是怎么被系统约束的？
3. 多个代理为什么能协作？

然后分别看：
- `src/commands.ts`
- `src/QueryEngine.ts`
- `src/coordinator/**` 与 `src/utils/swarm/**`

## 最后给一个阅读建议

不要一开始就盯着超长文件逐行读。

先想清楚每一层在解决什么问题，再回去看实现，你会轻松很多。