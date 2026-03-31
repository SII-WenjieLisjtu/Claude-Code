# Bridge、Remote 与跨端连接

很多人第一次看 Claude Code，会以为它只是本地终端工具。

但只要深入 `src/bridge/**` 和 `src/remote/**`，你会发现它明显不是这么简单。

## 一、这层在解决什么问题？

它解决的是：

- 本地 CLI 如何被网页端或远程端控制
- 会话如何跨端续接
- IDE 与终端如何共享上下文与操作能力
- Transport 断掉后如何恢复
- 用户如何在不同界面上继续同一个工作会话

## 二、关键文件

- `src/bridge/bridgeMain.ts`
- `src/bridge/replBridge.ts`
- `src/bridge/remoteBridgeCore.ts`
- `src/bridge/replBridgeTransport.ts`
- `src/remote/RemoteSessionManager.ts`
- `src/remote/SessionsWebSocket.ts`

## 三、核心理解

### 1. Bridge 是一个双向通信层
它不是“把终端输出同步一下”，而是要处理：

- 会话创建
- 远程工作项派发
- transport 建立与重连
- token / session / device 信任
- 用户消息与模型消息的双向同步

### 2. 它说明 Claude Code 的产品边界不止于 CLI
CLI 只是体验入口之一。

从架构上看，Claude Code 更像：

> 一个以 CLI 为主入口，但本质支持跨端协作与远程会话的执行系统。

## 四、为什么这层重要？

因为只有做了 Bridge / Remote，Claude Code 才真正具备：
- 网页与终端的连续性
- IDE 与 CLI 的联动可能
- 后台会话与恢复能力
- 多环境协作能力

## 五、建议怎么读

1. 先看 `bridgeMain.ts`，理解主流程
2. 再看 `replBridge.ts`，理解 bridge 核心循环
3. 再看 `RemoteSessionManager.ts`，理解 attach/remote 侧会话管理

## 六、一个重要判断

如果说 QueryEngine 解决的是“模型怎么工作”，
那么 Bridge 解决的是“系统怎么跨端继续工作”。

这是 Claude Code 从单机工具走向平台化产品的重要一步。