---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同じように、GitHub Copilot CLI にも多くのスラッシュコマンドがあります。高度な機能、内部情報、追加の設定を利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキストを確認します。
- `/model` で利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使える省略可能なコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えました。追加機能として、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への委任を確認します。

## Copilot CLI のコンテキストを調べる

大きなタスクや複雑なタスクでは、モデルのコンテキストウィンドウが上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に短縮します。また、スラッシュコマンドを使って自分で確認したり、コンパクトにしたりできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているか確認します。
4. 会話をコンパクトにします。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的に会話をコンパクトにします。タイミングを選びたい場合は `/compact` を使います。関係のない別タスクを始める場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルによって得意な作業が異なり、開発者ごとに好みも異なります。Copilot CLI では、モデルと使用状況を確認し、利用するモデルを選べます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` を使うと、Markdown または HTML ファイルへのエクスポート、共有可能なリンクの作成、GitHub gist の公開ができます。

1. `/help` を入力し、インストール済みバージョンの `/share` の選択肢を確認します。
2. 共有する場合は、たとえば `/share file` でローカルの Markdown ファイルにエクスポートするなど、目的に合った方法を選びます。
3. エクスポートした内容を確認してから、送信または公開します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は省略可能です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業の拡張や委任

ワークショップの主要な手順は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分配し、並行して実行します。
- `/worktree` は別のタスク用に分離された Git worktree を作成します。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期に作業させます。プルリクエストが作成される場合があります。

これらのコマンドは追加の worktree やリモート作業を作成するため、省略可能です。試す場合は、範囲の明確な新しいタスクから始め、通常のワークフローで結果を確認してください。非同期エージェントについて詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更し、セッションを共有して、内部の動作を確認できます。このレッスンでは `/context` と `/compact` でコンテキストを調べ、`/model` でモデルを確認し、`/share` でセッションを共有し、並列作業やクラウドへの委任に使えるコマンドを確認しました。

Copilot CLI にはほかにもスラッシュコマンドがあります。次は[これまでの学習内容を振り返り][next-lesson]、さらに学ぶための手順を確認します。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
