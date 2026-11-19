---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードで絞り込み機能を構築しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させます。その後、ブランチを公開して PR を作成します。

このレッスンでは、Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学び、Playwright MCP サーバーをワークスペースに追加し、ブラウザーで絞り込み機能をテストするよう Copilot に依頼して、ブランチを公開しプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外にあるツールを利用すると、さらに強力になります。Model Context Protocol (MCP) は Copilot が外部ツールやサービスと通信するための標準的な仕組みです。

![エージェントモードがコンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) と連携する仕組みの図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

MCP サーバーは AI エージェントと外部ツールやサービスの間をつなぎます。既存 API や npm パッケージなどのローカルツールとの通信を管理します。MCP サーバーごとに、エージェントが利用できるツールとリソースが異なります。

よく利用される MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスします。リポジトリの作成や更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作します。Web ページの移動、フォームの入力、ボタンの選択などを行えます。

ほかにも多くの MCP サーバーがあります。GitHub は、見つけやすさとエコシステムへの貢献を促進する [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。利用前にソースコードを確認し、公開元を検証し、セキュリティへの影響を考慮します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前のレッスンで Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。これを使って機能をテストします。

1. `.vscode/mcp.json` の `github` と並べて `playwright` の設定を追加します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面にブラウザーを表示せずに実行する設定です。デスクトップ環境のない Codespace で必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使うため、通常ブラウザーはすでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** が表示されたら選択します。サーバーが起動し、設定が正しいことを確認できます。
2. VS Code からサーバーの信頼を求められた場合は、設定を確認し、サーバーを起動するため信頼します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときに VS Code が起動し、信頼を確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]によって、Copilot は実際のブラウザーを操作できます。アプリを自分で操作する代わりに、エージェントがページを開き、移動し、絞り込みを適用して、結果を読み取って要約します。会話を続けながら機能を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を通じてエージェントが操作します。支援技術と同じようにページを認識するため、機能確認とともにアクセシビリティの基本チェックもできます。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を確認して結果を報告します。レポートを Issue の受け入れ条件と照らし合わせます。問題があれば、プルリクエストを開く前に追加の質問をするか、コードを修正させます。

> [!NOTE]
> テストするにはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーの初回利用時にブラウザーがダウンロードされる場合があります。不足が報告されたら、`npx playwright install chromium` を実行して再度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームのレビューに向けてプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot の変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

PR は github.com や GitHub コマンドラインインターフェイス (CLI) などで作成できます。ここでは、接続済みの GitHub ツールを使い、Copilot に関連 Issue を探して PR を作成させます。

1. Copilot Chat パネルに移動し、**New Chat** を選択します。
2. エージェント一覧で **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を取得したり操作したりするために **Continue** を求めたら選択します。GitHub ツールの初回利用時には GitHub への認証を求められることがあります。画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成します。
6. Copilot が表示したリンクを選択して PR を確認しますが、**まだマージしないでください**。

## まとめと次のステップ

このレッスンでは、MCP と MCP サーバーが Copilot にツールを追加する仕組みを学び、Playwright MCP サーバーをワークスペースに追加して、機能をリリースする前にブラウザーでテストしました。また、ブランチを公開し、絞り込み機能の PR を作成しました。

MCP サーバーを使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次はアクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
