---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進める必要がある場合、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、作成したいコードだけでなく、コードの構成方法も記述できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示ファイルとパス別の指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値 (該当する場合は注入される `db` 引数も含む) を説明する TSDoc/JSDoc を記述します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードの変更に応じてコメントを更新します。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使って Copilot に適切な情報を伝え、これらのプラクティスに沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件のコンテキストと希望を Copilot に伝えられます。希望する規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を設定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**リクエストで Copilot に送られる、単一の指示ファイルです。技術スタックや構築対象、ほとんどのチャットや CLI リクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスク向けの指針を記載します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、作業を始めやすいように指示ファイルが用意されています。変更を加える前に、既存の指示を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。この指示は、このリポジトリにおける Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、どのファイルに指示を適用するかを示す glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクトファイルへのリンクを確認します。これにより、大きな指示を再利用可能な小さなファイルに分け、コード生成時に参照する例を指定できます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、生成された TypeScript に TSDoc コメントを追加する指示がありません。`.github/copilot-instructions.md` にルールを加えます。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します (35行目付近です)。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しい指示を読み込むため、指示を保存した後、フィルター機能の会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められたらフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルを読み取る仕組みを確認し、新しい標準を機能に適用しました。次の作業を行いました。

- リポジトリの指示とパス別の指示がエージェントに適用される仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書くための5つのヒント][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
