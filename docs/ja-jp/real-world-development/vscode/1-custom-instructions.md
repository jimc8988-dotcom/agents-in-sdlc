---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

コンテキストは生成 AI を使ううえで重要です。作業方法や Copilot が知っておくべき背景情報がある場合、必要なときに参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのは[指示ファイル][instruction-files]です。コード生成の指針を Copilot に伝えます。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルで、プロジェクト固有の背景、コーディングガイドライン、文書化の規約を Copilot に伝える方法を確認します。
- 既存の指示を適用した状態で、絞り込みに使う最初のデータ関数 (パブリッシャーヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従うことを確認します。
- 指示の更新と絞り込みの基盤を `main` にコミットし、後の演習で Copilot が利用できるようにします。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わないことがあります。Copilot は確率的に動作します。この演習では、生成結果が一字一句同じになることではなく、指示の更新前後で傾向が変わることを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発作業に関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードを採用し、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、内容を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業するようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られます。ほとんどのチャットや CLI のリクエストに関係するプロジェクト全体の情報を記載します。技術スタック、構築するものの概要、ベストプラクティス、全体に適用するガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別の指示を作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成などを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイル作成について詳しく学ぶことは、このワークショップの範囲外です。サンプルプロジェクトにあるファイルから、代表的な方法を確認します。

- `copilot-instructions.md` には、構築するもの、プロジェクト構成、コーディング規約など、プロジェクト全体に関係する内容を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに向けた指示を記載します。
- 自然な言葉で、明確なガイドラインとコード例を示します。

指示ファイルの作成方法は 1 つではありません。実際に試し、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを整備しておくと役立ちます。サンプルには [UI の変更][ui-instructions]や [Astro][astro-instructions] など、複数の作業に対応した指示があります。
>
> Copilot に指示ファイルの生成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で手順が異なります。必要な場合は該当するレッスンで紹介します。
>
> テンプレートを探している場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに付属する `copilot-instructions.md` と `*.instructions.md` を、エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示にも注目します。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクトファイルへのリンクを確認します。指示を再利用しやすい単位に分け、Copilot が参照する例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内の文書化についてはまだ触れていません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用のブランチを作成する

変更をブランチで管理します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) でターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

変更前の指示で Copilot がどのように応答するか確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。後で指示を更新し、同じプロンプトをもう一度送って結果を比較します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント一覧から **Agent** を選択し、ファイルの変更を許可します。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べて変更を適用します。ヘルパーファイルとテストを変更する場合もあります。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 提案されたコードには、TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新して動作の一貫性を高めることがこの演習の目的です。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` に、リポジトリのコーディング規約を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを確認します。インライン文書化の規則がまだないことを確認します。
3. 既存の規約の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、リポジトリ全体の指示は `.github/copilot-instructions.md`、特定の言語やファイル種別、タスク向けの指示は `*.instructions.md` に置けます。今回のようなプロジェクト全体の規約は、リポジトリレベルのファイルに追加します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して新しい会話を開始します。
2. `src/lib/publishers.ts` を再び開き、Copilot に対象ファイルを示します。
3. **同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォーム用のパブリッシャーデータアクセスヘルパー。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべてのパブリッシャーの ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns パブリッシャーオブジェクトの配列で解決する Promise。
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリアセットと同じようにバージョン管理します。変更をコミットしてブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) でターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と Copilot がヘルパー基盤に加えたファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログの文書化規約に関する Issue に関連付けたプルリクエストを作成します。GitHub ツールがプロジェクトに接続されているので、Copilot に Issue を探して PR を作成し、マージを依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこのレッスンが初めてです。プロジェクトテンプレートでは GitHub のツールがワークスペースに設定済みです。初めてツールを使うときに、VS Code で GitHub へのサインインが求められる場合があります。接続方法は後のレッスンで扱う Model Context Protocol です。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) で Copilot Chat ビューを開き、エージェント一覧から **Agent** が選択されていることを確認します。
3. 現在のブランチから PR を作成し、指示の更新とパブリッシャーヘルパーの基盤を説明して、関連する Issue を閉じるよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう Copilot に求められた場合は、コマンドを確認して必要に応じて **Allow** を選択します。
5. PR が作成されたら、Copilot に PR をマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェルコマンドの実行を許可するよう求められた場合は、内容を確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示を参照する方法を確認し、VS Code の Copilot Chat を使って、既存指示の状態でコード生成プロンプトを実行し、`.github/copilot-instructions.md` に全体規約を追加し、同じプロンプトで新しい規約の影響を確認しました。指示の変更とヘルパーの基盤をコミットして `main` にマージしたため、次の演習から利用できます。

次は[エージェントモード][next-lesson]で、コードベース全体にわたる新機能を Copilot に追加させます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
