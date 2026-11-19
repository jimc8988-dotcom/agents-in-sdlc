---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント規約を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際、コンテキストは重要です。特定の方法でタスクを実行したい場合、その指示を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成したいコードだけでなく、その構成方法も説明できます。絞り込み機能を実装したので、Copilot が参照した指示を確認し、ドキュメント規約を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング規約に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- コメントには、コードの言い換えではなく、目的や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを行うようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られます。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに向けたガイドラインを提供します。

> [!NOTE]
> ほかの形式の指示への対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート一覧][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターターリポジトリに含まれる指示ファイルを確認し、変更を加える前に現在の内容を把握します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます (ターミナルではありません)。
3. プロジェクトの簡単な説明とコーディングガイドラインを確認します。この指示はリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが指示の対象ファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクトファイルへのリンクを確認します。大きな指示を再利用しやすい単位に分け、Copilot が参考にできる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

用意されている指示ファイルにはまだ不足があります。`.github/copilot-instructions.md` に TypeScript の新しいコードへ TSDoc コメントを追加するルールを設定しましょう。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。通常は 35 行目付近です。
2. 最後の箇条書きに次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指示を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませつつ機能のコンテキストを保つため、絞り込みの会話を終了して再開します。

1. 絞り込みの会話を開いたままの場合は `/exit` で終了します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートで `game-filters-cli` ブランチにいることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合は絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトで更新した指示を適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストを読み込む方法を確認し、新しい規約を機能に適用しました。このレッスンでは、リポジトリの指示とパスに応じた指示ファイルを確認し、指示ファイルを更新して、コードに対する影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行][next-lesson]し、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
