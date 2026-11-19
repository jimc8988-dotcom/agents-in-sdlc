---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两次练习中，你让 Copilot 云端智能体执行了三项任务，以改善用户体验并添加功能。云端智能体以异步、自主的方式工作，但监控这些任务仍然很重要。

有许多工具可用于管理分配给云端智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心可以查看所有拥有未关闭拉取请求（PR）的智能体任务，查看它执行的操作，甚至可以在会话进行中调整方向，帮助引导工作。

本课将：

- 查看智能体页面并监控云端智能体任务。
- 在会话进行期间引导智能体添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入这项功能也很合理。现在引导智能体，让它在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云端智能体任务

查看当前分配给 Copilot 云端智能体的所有任务的状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表应包含已分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择正在运行的任务之一，查看 Copilot 已执行的工作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和所采用的方法而变化。

4. 同时留意右侧显示的拉取请求（PR）窗格。可以在其中查看 PR 和已更改的文件，以便进一步监控。

## 引导云端智能体

已经查看正在进行的任务。现在要求 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云端智能体任务截图，突出显示了 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 如何确认提示词，并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的具体步骤，然后再将新请求纳入工作流程。

这同样需要几分钟。可以利用这段时间暂停片刻，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云端智能体任务的中心。借助这个任务控制中心，可以跟踪所有正在进行和已完成的任务、审查工作，也可以在会话进行中调整任务范围或提供额外指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云端智能体任务。
- 在会话进行中调整方向，要求实现其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
