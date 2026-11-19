---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に適した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、作業に役立つ指示、ベストプラクティス、コーディングパターン、専門知識を定義します。チームの知識を再利用できるエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを支援するセキュリティエージェント、テストパターンを統一するテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` または全体で使う `~/.copilot/agents` に Markdown ファイルとして定義します。各ファイルには、少なくとも `name` と `description` の YAML フロントマターと、動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義され、AI が作業する方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや別のエージェントを調整できます。このラボでは、アクセシビリティエージェントがサイトをアクセシビリティガイドラインに沿って確認、更新します。その際、プルリクエストワークフローやテスト実行を支援するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの作業と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを使えるように取り組んでいます。現在のダークテーマでは、文字と背景のコントラストが不十分で読みづらいという意見が寄せられました。デザインチームは、ユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

このレッスンでは、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat のアクセシビリティエージェントでハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を読み、Copilot への指示を確認します。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用のブランチを作成します (レッスン 3 の絞り込み PR とは分けて進めます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションを読み、アクセシビリティ対応サイト向けのコードを生成する際の主な役割、ベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント一覧には、`.github/agents` に定義したカスタムエージェントが表示されます。エージェントを選ぶと、そのエージェント専用の指示とツールを使って会話できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント一覧で **Agent** を選択します (まだ選択されていない場合)。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント一覧を開きます。既定では **default** が選択されています。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調査し、変更を提案してプロジェクトファイルに適用します。編集ごとに、ファイルパスとレビューできる差分がチャットに表示されます。
6. 編集が適用されると、**Files changed** の一覧が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> 処理には数分かかることがあります。Copilot はリポジトリに変更を加えながら、Astro コンポーネント、CSS、関連テストなどを編集します。

次のレッスンでは、進行中の作業を確認し、指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門 AI アシスタントです。チームの専門知識や規約を再利用可能なエージェントにまとめられます。

このレッスンでは、カスタムエージェントの定義方法と、Copilot Chat のエージェントモードで使用する方法を確認しました。

次は[エージェントの作業を監視して案内][next-lesson]します。変更が加わる様子を確認し、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
