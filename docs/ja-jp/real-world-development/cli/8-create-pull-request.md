---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込みの実装、指示の更新、スキルの変更、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの手順を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、絞り込みと品質確認の作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの作業を自動化する方法を確認します。
- 機能全体の変更と検証結果を確認します。
- 絞り込み PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の作業を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys では、開発者がマージを許可する権限を保ったまま、残りの PR 作業を自動化します。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能な状態にするために必要な作業を自動化します。有効にすると、Copilot は CI チェックの失敗への対応、レビューコメントへの返答、必要に応じた rebase など、PR を妨げている問題に取り組みます。リポジトリが許可していれば、最後に GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に任せることもできます。機能を十分にレビューし、準備ができたと判断してから有効にしてください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot が PR の CI を監視します。すべてのチェックが成功するとマージします。
7. `PR #14 was squash-merged successfully.` のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。作業がブロックされた場合は報告内容を確認し、リポジトリの設定で許可されていれば、レビュー済みの PR を手動でマージします。

## まとめと次のステップ

コードの生成、テスト、検証、プルリクエストの処理など、複数の開発作業を自動化しました。このレッスンでは、Agent Merge を確認し、変更と検証結果をレビューし、絞り込み PR を作成して、レビュー後にマージを許可しました。

次は[GitHub Copilot CLI の便利なスラッシュコマンド][next-lesson]を確認します。コンテキスト、モデル、共有、省略可能なクラウドへの委任に使えます。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
