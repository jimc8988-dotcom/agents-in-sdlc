---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づく絞り込み機能を実装して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、しっかりした手順が必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って、必要なものを正しい方法で構築できるようにします。このレッスンから、Issue を起点に機能を作り、コードの妥当性と動作を確認し、最後にプロジェクトへマージする、エージェントを使った開発手順を学びます。

> [!NOTE]
> 機能の作業は、同じ会話とブランチで続けます。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主要な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、`quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリやパブリッシャーでゲームを絞り込みたいと考えています。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要です。Plan モードで判断を確定し、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発フローに取り入れても、基本は変わりません。一般的な開発フローは次のようになります。

1. 必要な作業を記述した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって細部は異なりますが、多くの場合は上記の流れを調整したものになります。

この標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューと検証を経るようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、次の操作に進む前に入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、絞り込み用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることに注目してください。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## 絞り込み機能を計画する

計画を立てることで、Copilot に実装を依頼する前に機能の作り方や作業内容を決められます。複雑なタスクでは、事前に計画することをお勧めします。Plan モードに切り替えて計画を作成します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. 絞り込みの動作について質問された場合は、希望に応じて回答します。選択肢に正解や不正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読みます。
6. 決めた内容と異なる箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Copilot に実装を依頼します。Autopilot を使うと、完了と判断するまで問題に取り組ませることができます。

1. **Accept plan and build on autopilot (recommended)** (またはインストール済みバージョンで同様のラベルの項目) を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画を確認してください。依存関係が不足している場合やポートが競合している場合は、チェック完了とみなす前にセットアップを修正します。

## 実装をレビューして検証する

コードをマージする前に、他のコードと同様にレビューします。変更内容を確認して、サイトを起動し、動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、絞り込みの実装とテストを確認します。
3. 結果を Issue と計画中に合意した内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> で差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードに問題がなくても、アプリが動作することを確認します。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリ絞り込み、パブリッシャー絞り込み、組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、GitHub Issue から Copilot の会話を開始し、Plan モードで要件を決め、Autopilot モードで実装し、コードをレビューして転送ブラウザーで動作を検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化された規約に沿うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
