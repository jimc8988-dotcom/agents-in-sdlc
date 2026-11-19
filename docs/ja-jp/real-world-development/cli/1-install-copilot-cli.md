---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、登録済みの絞り込み Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールを利用できます。作業を依頼して変更を加えながら、ターミナル内で開発を続けられます。まずはツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込み GitHub MCP サーバーで絞り込み Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに取り組むため、AI エージェントの利用を始めようとしています。Copilot CLI は、多くの開発者が日常的に使うターミナルでエージェントを利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップで使う準備をします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js がインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以降が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントによる認証が求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選んで信頼します。

> [!NOTE]
> Codespace では、GitHub セッションですでに認証されている場合があります。認証を求められずに Copilot CLI が起動すれば、そのまま利用できます。

## 基本を確認する

通常のシェルプロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語でエージェントに指示し、スラッシュコマンドで会話を操作します。

1. `/model` と入力し、矢印キーで **Auto** を選択します。<kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> で確定します。
2. `/help` を入力して、インストール済みのバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問をして、すべてが動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトで絞り込み Issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の小さな変更を行った後、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使用し、自動的に worktree を作成することはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用の簡易設定を使う

Copilot CLI は、許可された操作以外のツールを使う前に確認を求めます。このワークショップでは、Codespace 内の承認プロンプトを省略して演習に集中できるよう、`--yolo` を使って再起動します。

> [!CAUTION]
> `--yolo` は `--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を有効にし、すべての操作を自動的に許可します。Codespace や VM など隔離された環境でのみ使い、日常の開発で既定の設定にしないでください。詳細は[ツールの使用を許可または禁止する方法][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` によって、後のレッスンで Issue やプルリクエストを扱うための読み書き可能な GitHub MCP ツールを有効にします。Codespace からローカルコンピューターへのアクセスは制限されていますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動保存します。後のレッスンでは、指示の変更やエージェントの追加後に `copilot --resume` を使って、同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

お疲れさまでした。このレッスンでは、Copilot CLI をインストールして GitHub アカウントで認証し、リポジトリを信頼して簡単な会話を試し、組み込み GitHub MCP サーバーで絞り込み Issue を見つけました。

次は[最初の小さな変更][next-lesson]に取り組み、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
