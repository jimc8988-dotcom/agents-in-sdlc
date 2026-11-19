---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に行います。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent で次のことを確認します。

- コード生成環境をカスタマイズする方法
- 安全に操作する方法
- Issue の範囲を明確に定める重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した開発者が十分な文書を残さなかったため、まずはアプリケーション内のエクスポートされたすべての関数に TSDoc コメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリの別のゲームを表示し、利用者がほかのゲームも引き続き見られるようにします。まずはユーザー体験の受け入れテストに使える簡単な実装が必要です。これは現在の作業を妨げていますが、優先度がより高い Issue もあります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てると、エージェントがバックグラウンドで作業する間、開発者は別の仕事に集中できます。後で作業に戻り、結果が期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当て後、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境を準備してタスクに取り組みます。作業中でも、完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めることが重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳細を確認すると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を明確に][cloud-agent-best-practices]してください。AI ペアプログラマーとして扱い、段階を踏んで学習、実験、調整します。生成 AI を取り入れても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習ではカスタム指示とその役割を学びました。すでに実習した場合は `.github/copilot-instructions.md` を見たことがあるでしょう。まだの場合は、ここで確認してください。cloud agent に作業を割り当てる前に、このリポジトリの指示ファイルを読み取り専用で確認しておくと、後でその影響を見つけやすくなります。

GitHub Web UI または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — TypeScript の規約と TSDoc コメントに関する **Code standards** を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てるとき、プルリクエストで TSDoc コメント、TypeScript の規約、コメントヘッダーを確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、適切な環境と初期セットアップが必要です。これは Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。このラボ用の設定は事前に用意されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、E2E テスト用ブラウザー、移行とシードが済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係をインストールするなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同じように見えますが、いくつかのポイントがあります。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、まったく用意されていません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新メンバーの受け入れを難しくします。Copilot は文書化にも役立ちます。必要なドキュメントをバックグラウンドで作成するタスクを割り当て、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルのプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が Todo リストを作成します。

> [!NOTE]
> Copilot が PR に Todo リストを表示するまで数分かかる場合があります。前述のワークフローで環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を決めています。

13. Todo リストを読み、実施するタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ブラウザーを更新する必要がある場合があります。

16. セッションをスクロールして、Copilot がコードを調査し、状態を理解し、方針を考えてコードを作成する様子を確認します。

作業には数分かかることがあります。Copilot cloud agent の主な利点は、非同期で作業を任せ、ほかのタスクに集中できることです。2 つ目のタスクを割り当てたら、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent に作業を任せる間に別のタスクを進められます。デザインチーム向けに関連ゲームの表示を追加する作業を割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに目の絵文字 👀 が表示され、Copilot が作業を始めたことが分かります。

![Issue に Copilot が目の絵文字を表示し、作業中であることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目のタスクに取り組んでいます。cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。作業が終わったら確認します。次にカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を Copilot に割り当てて非同期に作業させられます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などを依頼できます。

このレッスンでは、コード生成環境のカスタマイズ、安全な操作、Issue の範囲を明確にする重要性、Copilot への Issue の割り当てを確認しました。

cloud agent がバックグラウンドで作業する間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent では MCP サーバー][cloud-agent-mcp]も使用できます。また、前のレッスンで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
