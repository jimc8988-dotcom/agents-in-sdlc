---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot と共にユーザー体験や機能を改善してきました。Copilot にコードのドキュメント化、関連ゲーム機能の実装、ハイコントラストモードとライトモードによるアクセシビリティ対応を依頼しました。コードの変更を確認し、必要があれば Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。ここでは、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案をレビューし、チームのレビューを有効にする前に問題を解決します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視されずに作業するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスと、作業ブランチへの書き込みアクセスのみを持ちます。
- Cloud agent は GitHub Actions 内で動作し、作業用の一時的な環境を作成します。
- GitHub Actions のワークフローは、人間が承認するまで実行されません。
- 既定では [外部リソースへのアクセスが制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した 1 つ目のプルリクエスト (PR)、コードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォール][agent-firewall]をカスタマイズまたは無効にできます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選び、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを確認します。具体的な変更内容は状況によって異なります。

    Exercise 2 の最初に確認したカスタム指示ファイルによって追加された TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。単一の Astro アプリのプロジェクトチェックが成功したことを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同じように、PR やコードのインラインコメントで **@copilot** を指定できます。Copilot はコメントを確認し、更新に対応する新しいセッションを開始します。結果は確率的に変わるため、決まった依頼文はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** を指定して、生成されたドキュメントへの変更を依頼するコメントを追加します。上記の例や、必要な別の提案を使ってください。
2. **View Session** を選択し、Copilot が作業を行う様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、更新内容をレビューします。

満足するまで改善を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローが承認待ちであることを示す表示を探します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローの実行を確認します。単一の Astro アプリに対するプロジェクトチェックが成功するまで待ちます。完了には数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能が実装されました。ほかの PR と同様に、Copilot cloud agent に追加の変更を依頼できます。表示する関連ゲームの数を調整する、コメントヘッダーと TSDoc コメントを追加する、といった依頼ができます (この Issue はカスタム指示を変更する前に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が新しいセッションで対応します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には Exercise 3 で依頼したハイコントラストモードと、Exercise 4 の Mission control から追加を依頼したライトモードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するローカルストレージ
   - ハイコントラストとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローの実行を確認します。単一の Astro アプリに対するプロジェクトチェックが成功するまで待ちます。完了には数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。ターミナルで Copilot が作成したブランチ名に置き換え、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が維持されることを確認してください。

アクセシビリティ用カスタムエージェントによって、アクセシビリティのベストプラクティスに沿った実装が促されました。改善すべき点があれば、前の PR と同じようにコメントで **@copilot** を指定して変更を依頼できます。

## 省略可能な演習 — 委任を続ける

Cloud agent には実際のバックログ作業を依頼し、その間に別の作業を進めると効果的です。リポジトリに Issue を追加し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。このレッスンでは、リポジトリに含まれるカスタム指示ファイルを確認して cloud agent の出力への影響を知り、Issue を割り当てて環境構築、計画、非同期の実行を確認し、アクセシビリティ用カスタムエージェントでハイコントラストモードとライトモードを追加しました。さらに Copilot agents ページからセッションを監視して案内し、`@copilot` に変更を依頼し、ワークフローを承認して PR をレビューしました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。別の作業環境で学ぶ場合は、次のハーネスも参考になります。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 連携を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテスト範囲の拡充を Issue にして cloud agent に割り当てる。
- 別の分野に特化したカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
