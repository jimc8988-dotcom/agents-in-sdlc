---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな変更でも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグし、変更漏れに気付いて再度編集します。

Copilot のエージェントモードを使うと、この作業を支援できます。

エージェントモードでは、Copilot は IDE 内でより自律的に作業します。プロジェクト構成を調査し、必要なファイルを変更し、テストを実行して、見つけた問題を修正します。ここでは、エージェントモードを使ってサイトに機能を追加します。

> [!NOTE]
> エージェントモードと cloud agent は名前が似ていますが、異なる用途向けです。エージェントモードは IDE 内で作業し、すばやいフィードバックとやり取りを可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

このレッスンでは、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調査し、関連ファイルを特定して変更を調整する方法
- Copilot エージェントモードで UI とデータ層にまたがる機能を実装する方法
- Copilot が生成した変更とテストを、コードベースにマージする前にレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前のレッスンでパブリッシャーヘルパーを追加しました。ここでは Copilot エージェントモードで残りのデータ層、UI、テストを完成させます。

## Tailspin Toys のサイトを実行する

変更を加える前に、Tailspin Toys のサイトを開いて現在の機能を確認します。

このサイトは、開発者向けのテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。Astro アプリはビルド時に静的 HTML を生成します。ページはフロントマターから Drizzle ORM を使ってローカル SQLite データベースを直接参照するため、独立したバックエンド API やクライアント UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

### サイトを起動する

サイトを簡単に起動できるように、単一 Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。

1. Codespace に戻ります。前の演習後に `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドでサイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された `http://localhost:4321` のリンクを選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost の URL を選ぶと、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Codespace の開発サーバーに接続するプライベートなトンネルです。

### サイトを確認する

サイトが起動したら、機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボードゲーム一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェント一覧から **Agent** を選択します。エージェントは自律的に計画し、複数のファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。LLM は確率的に動作するため、提案は記載例と異なる場合があります。これは想定された動作です。

> [!NOTE]
> 確率的な動作により、Copilot は異なる MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを送信し、バックログについて Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して Issue 一覧を取得するコマンドを実行します。
5. Copilot が作成した一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

コードの生成を始める前に、Copilot にコンテキストを与える指示ファイルを確認します。このサイトの機能追加方法を説明する [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性、アクセシビリティ、Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンクが含まれます。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用しやすく、小さな単位に分け、Copilot に例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) に絞り込みロジックを追加または調整します。
- 絞り込みの動作をテストするテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

さらに、コードベースにマージする前にテストが成功することを確認します。Copilot エージェントモードにこれらの作業を依頼します。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

4. **Claude Sonnet 4.5** が引き続き選択されていることを確認します。
5. 次のプロンプトを使い、バックログの関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを見つけて、データ層、UI、テストを変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はなく、完了後に変更全体を確認できます。

7. Copilot に求められたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat でテスト実行の許可を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまで対応します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys のサイトに機能を追加しました。UI とデータ層にわたる変更の実装、関連ファイルの調査と調整、変更やテストのレビューについて学びました。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。このレッスンで学んだことを使い、Copilot にページネーションを追加させます。既存の指示ファイルを使い、ページサイズをユーザーが選べるようにするか固定するかなどの要件を決め、望む実装に必要なガイダンスをプロンプトで伝えます。Copilot に変更やコンテキストを追加で伝えながら反復するのは、通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
