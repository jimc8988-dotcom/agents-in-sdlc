---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも大切です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。この Mission control では、未完了のプルリクエスト (PR) があるエージェントのタスクを確認できます。作業内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに機能追加を依頼します。

## シナリオ

ハイコントラストモードを依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。エージェントの作業にライトモードの追加を依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン画面と左側の一覧に、Copilot に割り当てた次のタスクが表示されていることを確認します。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用 API を生成する。
   - サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実施した操作を確認します。たとえば、リポジトリのチェックアウト、環境の作成、MCP サーバーのセットアップ、タスク完了に向けた作業などです。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側のプルリクエスト (PR) ペインも確認します。ここから PR と変更ファイルを監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページのタスクと、作業中のセッションに指示を追加するダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加することを確認します。

## Copilot の作業を待つ

更新したタスクに Copilot が取り組みます。メッセージを送信した時点で作業中だった手順が終わると、新しい依頼が作業の流れに追加されます。

これにも数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページで cloud agent のタスクを監視し、作業を案内しました。この Mission control では、進行中や完了したタスクを追跡し、実施内容を確認して、作業中の範囲を調整できます。

このレッスンでは、エージェントページで cloud agent のタスクを監視し、進行中のセッションに機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[作成されたプルリクエストを反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
