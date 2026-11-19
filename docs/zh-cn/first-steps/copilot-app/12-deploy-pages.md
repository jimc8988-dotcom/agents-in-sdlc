---
title: "第 12 课 - 使用 GitHub Pages 部署站点"
description: "使用 GitHub Copilot 应用配置 Pages 工作流、审阅其拉取请求并验证已上线的 Space Quiz。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，让它拥有一个公开的主页。Copilot 可以准备工作流并协助部署，但你仍需负责审阅更改并确认站点已上线。

在本课中，你将：

- 请 Copilot 配置 GitHub Pages 部署。
- 合并前审阅工作流和拉取请求。
- 跟踪部署并验证已发布的站点。

## 场景

Space Quiz 已经位于 GitHub 仓库中，但仍需一种简单的方式让用户试用。使用 GitHub Pages 发布这个单文件站点，可以通过公共 URL 访问，无需添加服务器或运行时依赖项。

## 请 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示前，请审阅批准设置和目标仓库。启用 **Approve all** 后，不应再期待系统显示单独确认提示。

在 `space-quiz` 的 Copilot 应用会话中发送以下提示：

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## 审阅工作流和拉取请求

1. 确认仅在仓库尚未包含 README 时才添加 README。
2. 审阅拉取请求中的更改，包括工作流的构建和部署步骤、触发器及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 请求任何必要的修正并审阅更新后的差异。授权合并前，请等待必需的拉取请求检查通过。

## 授权合并与部署

> [!WARNING]
> 以下提示授权代理合并已审阅的拉取请求、更改仓库 Pages 设置并发布站点。只有完成审阅并确认仓库和拉取请求后才发送此提示。批准设置可能允许这些操作，无需再次确认。

将 `<PR_NUMBER>` 替换为你审阅的拉取请求编号，然后发送这个单独的提示：

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段图：已合并的 space-quiz 拉取请求；成功的 GitHub Actions Deploy website 运行，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的上线版 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟踪 GitHub Actions 部署运行直至成功。
2. 打开报告中的 URL，并确认 Space Quiz 已加载。

如果代理无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且实时站点已加载，才可认为任务完成。

## 小结和后续步骤

你的 Space Quiz 现已作为实时站点上线，你也已验证部署，而不是仅依赖代理的报告。继续学习[第 13 课：审阅和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
