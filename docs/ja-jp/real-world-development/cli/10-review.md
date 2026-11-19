---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、参考情報を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys で一連の作業を進めました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みの絞り込み Issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込みを確認しました。
- 品質保証 (QA) カスタムエージェントを作成、選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認結果をレビューしました。
- 絞り込み機能全体をレビューし、Agent Merge に絞り込み PR のマージを許可しました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使えるスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップでは、更新済みの `main` からブランチを作り、2 つの PR を完成させます。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

絞り込みの計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、これらの変更は 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA はこれらの結果と要件、テスト範囲のレビューをまとめ、PR 作成前の確認を支援しました。

追加のテストは、本当に不足している部分を補うものにします。QA を実行しても新しいテストが不要な場合があります。レビュー後もコードと検証結果を確認し、変更があった場合は影響する検証をやり直します。

## ベストプラクティス

Copilot に与えるコンテキストやツールが、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は規約を、スキルは繰り返し作業を、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方法を検討するとき、**Interactive** は範囲を絞った変更を自分で進めるとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストは、インフラストラクチャと同じように重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローは完了しました。次の CLI 機能も確認できます。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。本番コード、趣味のコード、作りたいと思いながら手をつけていなかった小さなアプリなどで試し、学んだことをチームで共有し、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。別の作業環境と比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
