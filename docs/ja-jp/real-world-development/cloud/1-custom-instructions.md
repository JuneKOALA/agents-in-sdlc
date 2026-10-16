---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。作業方法に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、参照できるようにしておきます。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、その進め方も伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通して、プロジェクト固有のコンテキスト、コーディング指針、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- リポジトリ全体に適用される標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code と CLI の演習とは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムの比較が難しいからです。このハーネスの後半で cloud agent が作成した PR をレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc ドキュメントコメントでコードを説明します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。希望する規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を設定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築対象、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`: TypeScript など特定の言語や、UI コンポーネント作成、ユニットテスト作成などのタスク向けのガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にだけ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter がある `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応形式の詳細][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、この演習の範囲外です。サンプルプロジェクトのファイルは、代表的な方法を示しています。大まかな指針は次のとおりです。

- `copilot-instructions.md` には、構築対象、プロジェクト構成、全体に適用されるコーディング標準など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル形式や特定のタスクに関する指針を記載します。
- 自然な言葉で、明確な指針を書きます。コードの良い例と悪い例を含める方法もあります。

指示ファイルや AI の使い方に唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意しましょう。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、利用環境によって操作方法が異なります。関連するレッスンでは、その環境での方法を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。基本となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の入れ子の指針を確認します。これらは Copilot を使うすべての作業に適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリルートからの相対パスとして評価されます。たとえば `**/*.test.ts` に一致するすべての TypeScript テストファイルに指示が適用されます。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや既存のプロジェクトファイルへのリンクがあります。指示を小さな再利用可能なファイルに分割し、Copilot が参照する例を指定できます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーを使うプロジェクト全体のルールを追加することです。cloud agent は、このハーネスの後半で割り当てる Issue に取り組むときにこの指示を読み込みます。

編集前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは編集をきちんとコミットします。

1. codespace ターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を渡すためのファイルです。コーディング標準を記載して、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。ここにはコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。
3. 既存の標準のすぐ下に、ファイルヘッダーと TSDoc ドキュメントコメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、リポジトリ全体の指針は `.github/copilot-instructions.md` に、特定の言語、ファイル形式、タスク向けの指針は `*.instructions.md` に記載できます。今回追加したようなプロジェクト全体の標準は、リポジトリレベルのファイルに適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチが作成されます。cloud agent が更新内容を参照できるように、指示を `main` にマージします。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けてプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` へコミットしてもかまいません。実際のプロジェクトに近い手順を体験するため、ここではブランチを使っています。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る仕組みを確認し、リポジトリ全体に適用される標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成する PR を通して、この標準が適用される様子を確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
