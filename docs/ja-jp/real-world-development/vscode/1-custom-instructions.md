---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。作業方法に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。主な方法の一つが[指示ファイル][instruction-files]です。指示ファイルを使って、コード生成に関するガイダンスを提供します。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通して、プロジェクト固有のコンテキスト、コーディング指針、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルター機能の最初のデータスライス (発行元ヘルパー) を生成します。
- リポジトリ全体に適用される標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を採用する様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で更新済みの指針を利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、指示を更新した後に出力がどのように変わるかを確認します。同じ文字列の出力を得ることが目的ではありません。

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

- `.github/copilot-instructions.md`: リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築対象、ベストプラクティスなど、ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`: TypeScript など特定の言語や、UI コンポーネント作成、ユニットテスト作成などのタスク向けのガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にだけ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter がある `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応形式の詳細][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、この演習の範囲外です。スタータープロジェクトには、代表的な方法を示す例が用意されています。大まかな指針は次のとおりです。

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

このリポジトリに含まれる指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の入れ子の指針も確認します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリルートを基準に、指示を適用するファイルを決定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや既存のプロジェクトファイルへのリンクがあります。指示を小さな再利用可能なファイルに分け、Copilot が参照する例を指定できます (ここにあるパスは指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここでは、ベストプラクティスに従って作業用ブランチを作成します。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、ファイルを編集できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。ヘルパーファイルとテストの両方が変更される場合があります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付けされた発行元の配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が改善することを確認するのが目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コーディング標準を記載し、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。既存の標準は記載されていますが、コード内ドキュメントのルールはまだありません。
3. 既存の標準のすぐ下に、ファイルヘッダーと TSDoc ドキュメントコメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、リポジトリ全体の指針は `.github/copilot-instructions.md` に、特定の言語、ファイル形式、タスク向けの指針は `*.instructions.md` に記載できます。プロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を再度開き、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys のクラウドファンディング プラットフォーム用の発行元データアクセス ヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべての発行元の一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのリポジトリ資産と同じように、ソース管理で管理します。指示の更新とヘルパーの変更をコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、Copilot がヘルパー用に変更したすべてのファイルをステージしてコミットします。

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

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue と関連付けてプルリクエストを作成します。GitHub ツールをすでに使える Copilot に依頼できます。Issue を見つけ、Issue を完了する PR を作成して、マージを依頼します。

> [!NOTE]
> Copilot が GitHub 上で操作するのは、この演習が初めてです。プロジェクト テンプレートには GitHub ツールがあらかじめ設定されています。Copilot が初めて使うときに、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では Model Context Protocol による接続の仕組みを学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と、今後のフィルター機能開発用に追加した発行元ヘルパーの基盤の両方を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot による GitHub 操作の実行を **Allow** するよう求められたら、コマンドを確認して適切に許可します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルブランチを `main` に戻して最新状態にします。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot による GitHub 操作とシェル コマンドの実行を **Allow** するよう求められたら、内容を確認して適切に許可します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る仕組みを確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案コードが新しい標準を採用する様子を確認しました。
- 指示の更新とヘルパーの基盤をコミットし、`main` にプッシュしました。これにより、次の演習で利用できるようになりました。

次は、[エージェントモード][next-lesson]で Copilot にコードベース全体の機能を追加してもらい、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
