---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上の絞り込み機能を確認しました。次は、明確な QA 手順を持つカスタムエージェントで、これらの機能を組み合わせます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選び、絞り込み Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫した方法で確認したいと考えています。カスタムエージェントを使って QA 作業をまとめ、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な構成です。プロファイルに目的、指示、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの規約を説明し、`quality-checks` スキルは繰り返し使えるチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらを使って要件を確認し、結果を報告するよう Copilot に指示します。既存のカスタマイズを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。実行するチェックと守るべき範囲を定義します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトでカスタムエージェントを作成させます。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の手順と制約を設定したか確認します。検証だけを依頼した場合に、エージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、含めると表示名が分かりやすくなります。
3. プロファイルの指示を読み、要件から始めてリポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. 根拠を示した報告を行い、実装コードを変更する前に許可を求め、変更をコミットせず PR も作成しないことを確認します。
5. これらの責務や制約が抜けている場合は、次に進む前に既定のエージェントへ修正を依頼します。

## Issue を対象に QA を実行する

カスタムエージェントを作成した後、同じ絞り込みの会話を再開して QA を選びます。Issue と計画の内容を引き続き参照できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントが利用可能な一覧に表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したらレポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、結果をレビューしました。このレッスンでは、カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認し、QA プロファイルを作成して、Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
