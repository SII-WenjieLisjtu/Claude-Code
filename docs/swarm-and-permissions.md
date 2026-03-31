# 多 Agent、权限系统与 Plan Mode

这一部分是 Claude Code 最像“工程系统”而不是“聊天工具”的地方。

## 一、多 Agent 真正难的是什么？

不是“spawn 几个 agent”。

真正难的是：
- 谁负责拆任务
- 谁负责审批
- 谁负责回收结果
- 谁能改文件
- 谁在什么目录里改
- 多个 agent 之间怎么通信
- 出错后怎么恢复

Claude Code 在这方面的做法非常系统化。

## 二、关键文件

### 多 Agent / Team
- `src/tools/TeamCreateTool/TeamCreateTool.ts`
- `src/coordinator/coordinatorMode.ts`
- `src/utils/swarm/**`
- `src/utils/teammateMailbox.ts`

### 权限与计划模式
- `src/Tool.ts`
- `src/utils/permissions/**`
- `src/tools/EnterPlanModeTool/**`
- `src/tools/ExitPlanModeTool/**`
- `src/tools/EnterWorktreeTool/**`
- `src/tools/ExitWorktreeTool/**`

## 三、Plan Mode 的意义

Plan Mode 很重要，因为它把“复杂任务不要直接开干”做成了产品能力。

模型进入 plan mode 后，会被约束为：
1. 先探索
2. 先理解
3. 先写计划
4. 再请求批准
5. 批准后再执行

这是一种明显偏工程治理的思路。

## 四、Swarm / Team 的意义

Team 不是装饰概念，而是落在真实文件结构和任务结构上的：
- team config
- task list
- teammate metadata
- mailbox
- worktree
- leader / worker 关系

这说明 Claude Code 的多代理能力不是 prompt 幻觉，而是有真实底层支撑。

## 五、权限为什么是核心机制？

因为模型一旦能写文件、跑命令、开网络，它就不再只是“回答问题”。

这时系统必须明确：
- 什么能自动做
- 什么必须问用户
- 什么只能规划不能执行
- 什么必须隔离在 worktree 中做

这也是为什么权限、plan mode、task system、worktree 会一起出现。

## 六、一个最值得学习的判断

Claude Code 并不是先有“超强 agent”，再补安全。

它更像是先搭好：
- 权限边界
- 审批流
- 任务系统
- worktree 隔离
- team 协议

然后再让 agent 在这个系统里工作。

这正是它成熟的地方。