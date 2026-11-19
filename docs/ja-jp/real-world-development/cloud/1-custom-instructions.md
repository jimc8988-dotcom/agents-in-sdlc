---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

コンテキストは生成 AI を使ううえで重要です。作業の方法や Copilot が知っておくべき背景情報がある場合は、必要なときに参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行内容だけでなく方法も伝えられます。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有の背景、コーディングガイドライン、文書化の規約を Copilot に伝えます。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムの比較が難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューし、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発作業に関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードを採用し、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの内容を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体に適用する指示も、特定のファイルやタスクに限る指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに向けたガイドラインです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

このワークショップでは、サンプルプロジェクトの指示ファイルを通じて代表的な方法を紹介します。

- `copilot-instructions.md` には、構築するもの、プロジェクト構成、コーディング規約など、プロジェクト全体に関係する内容を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに向けた指示を記載します。
- 自然な言葉で、明確なガイドラインとコードの例を示します。

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

## このプロジェクトのカスタム指示を確認する

このリポジトリにある `copilot-instructions.md` と `*.instructions.md` を、エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどに関する指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイルやプロジェクトファイルへのリンクを確認します。指示を再利用しやすい単位に分け、Copilot が参照する例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内の文書化についてはまだ触れていません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリの標準を追加する

ここでは、コード内に TSDoc コメントとファイルレベルのコメントヘッダーを追加する規則をプロジェクト全体に設定します。後の演習で Issue を割り当てると、cloud agent がこの指示を参照します。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、ここでは編集内容を独立してコミットします。

1. Codespace のターミナルでブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` を開き、次の手順でプロジェクトのコーディング規約を追記します。

1. **Code formatting requirements** セクションを探します。通常は 27 行目付近です。既存の規約を確認します。コード内の文書化に関するルールはまだありません。
2. 既存の規約の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスク向けの `*.instructions.md` に配置できます。今回追加するようなプロジェクト全体の規約には、リポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

次の演習で Issue を Copilot に割り当てると、cloud agent は `main` からブランチを作成します。cloud agent が更新した指示を使えるように、この変更を `main` にマージします。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent がこの指示を読み取ります。

> [!TIP]
> ワークショップでは `main` に直接コミットしても構いません。実際のプロジェクトの進め方に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルをどのように使うかを確認し、リポジトリ全体に適用する規則を `.github/copilot-instructions.md` に追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューすると、この規則の影響を確認できます。

次は[最初の Issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
