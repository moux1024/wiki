# How React streams UI out of order and still manages to keep order

> 来源: https://inside-react.vercel.app/blog/how-react-streams-ui-out-of-order
> 日期: 2026-05-03
> 分类: 前端技术
> 标签: React, Streaming, Suspense, Server Components, Out-of-Order, 性能优化

> **说明**：原文站点不可达，本文基于多个来源（Reddit 讨论、React 官方文档、社区技术博客）综合整理而成，非原文完整翻译。

本文是 Dan Abramov 在 inside-react.vercel.app 系列中的深度文章，探讨 React 如何实现乱序流式渲染（out-of-order streaming）并最终保持 UI 顺序一致性。

## 乱序流式渲染的动机

传统 SSR 是"全有或全无"模式：服务器必须等待所有数据准备好才能发送 HTML，用户在此期间只能看着空白页面。

流式渲染允许服务器边生成边发送 HTML，用户可以更早看到内容。但 Suspense 边界内的不同组件可能在不同时间完成数据获取，导致渲染顺序不确定——这就是乱序问题的来源。

## 从外到内 vs 乱序

React 的默认流式策略是"从外到内"（outside-in）：先渲染外层 shell，再逐步填充内部内容。

乱序流式渲染则允许内部某个慢组件（如评论区）在它准备好后立即发送，不必等待其他同级组件完成。例如：页面有 Sidebar、Main Content、Comments 三个 Suspense 区域，Comments 最后完成，但可以在它就绪时立即流式发送，无需等待前两者。

## 保持顺序一致的机制

React 通过在客户端进行 reorder 操作来保持最终 DOM 顺序正确：

1. **服务端**按完成顺序发送 HTML chunks，每个 chunk 带有位置信息（Suspense boundary 的 ID）
2. **客户端** React 接收后，按照正确的 DOM 树位置插入，即使接收顺序是乱的
3. 这类似于"拼图"——每块到达后放到正确位置

## Suspense 边界的作用

- 每个 Suspense boundary 是一个独立的流式单元
- React 使用 Suspense boundary 的嵌套结构来确定每个 chunk 应该插入的位置
- 没有 Suspense boundary 包裹的组件会阻塞其父级渲染

Suspense boundary 本质上定义了"可以独立等待和流式传输"的粒度边界。

## 内部实现原理

React 内部使用类似 Fiber 的结构追踪每个 Suspense boundary 的状态：

- 服务端渲染时，React 为每个 Suspense boundary 分配 ID（如 `B:0`, `S:0`）
- 流式响应中包含特殊指令（如 `$L0`, `$S0`），告诉客户端在哪里插入内容
- 客户端的 reconciler 根据这些指令，在正确的 Fiber 节点上完成更新

这些指令构成了一个轻量级的"位置寻址协议"，确保乱序到达的 chunk 能被放置到 DOM 树的正确位置。

## 与 RSC (React Server Components) 的关系

- RSC 协议本身就支持乱序流式传输
- Server Components 可以独立流式返回，不受其他组件阻塞
- 这使得复杂页面的 TTFB（Time to First Byte）和 FCP（First Contentful Paint）显著改善

RSC + Suspense + 乱序流式传输三者结合，构成了 React 服务端渲染的现代基础设施。

## 实际效果

- 用户更早看到有意义的内容，而不是等待整个页面加载
- LCP (Largest Contentful Paint) 和 INP (Interaction to Next Paint) 等核心 Web 指标得到优化
- 开发者不需要手动管理加载顺序，React 自动处理

## 相关来源

- https://www.reddit.com/r/reactjs/comments/1sue3rc/how_react_streams_ui_out_of_order_and_still/
- https://react.dev/reference/react/Suspense
- https://jser.dev/react/2022/04/02/suspense-in-concurrent-mode-1-reconciling/
- https://gal.hagever.com/posts/out-of-order-streaming-from-scratch
- https://gitnation.com/contents/out-of-order-streaming-the-secret-powering-modern-react

## 相关页面

- [[卡片式对话的协议方案探索和思考]] — 同样讨论流式渲染场景，侧重前端卡片交互的协议设计

---

*由 knowledge-wiki skill 自动收录并整理*
