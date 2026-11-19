---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** から、普段使っているコードエディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat のエージェントモードを使い、MCP で外部ツールに接続し、カスタムエージェントを活用します。IDE を離れずに、ファイル、ターミナル、問題一覧を参照できます。

まずカスタム指示を追加し、Copilot が指示に従う様子を確認します。次にエージェントモードで、UI、データ層、テストをまたぐ絞り込み機能を実装します。続いて Playwright MCP サーバーを接続し、プルリクエストを開く前に Copilot にブラウザーで機能をテストさせます。最後にアクセシビリティ用カスタムエージェントを確認して使い、エディターを離れずに Copilot の変更を監視、案内し、反復して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加し、適用を確認します |
| [2. エージェントモード][ex2] | コード生成 | エージェントモードで絞り込み機能を構築します |
| [3. Playwright MCP によるテスト][ex3] | 外部ツール | Playwright MCP サーバーでブラウザーから機能をテストします |
| [4. カスタムエージェント][ex4] | 専門エージェント | カスタムエージェントを確認して使用します |
| [5. エージェントの監視と管理][ex5] | 監視 | エージェントセッションを監視し、指示を追加します |
| [6. 反復して改善する][ex6] | レビュー | ローカルで Copilot の作業をレビューし、次の手順を選びます |
| [省略可能: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit でモデルを準備し、エージェントをデプロイしてサイトに接続します |

## 前提条件

ワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランの GitHub アカウント
- [ ] GitHub Codespaces にアクセスできること

> [!TIP]
> 有料プランをお持ちでない場合、認定された学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[callout-student-plan-education]: https://github.com/education/students

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
