---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業があります。手動でコードを検証し、指示ファイルで規約に従わせる方法を学びました。では、テストや lint、継続的インテグレーション (CI) に関係するほかの作業はどうでしょうか。

このような作業には**エージェントスキル**が適しています。スキルを使うと、こうした操作の適切な実行方法を Copilot に伝えられます。

このレッスンでは、次の作業を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。結果を読みやすくするため、チームはレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめ、エージェントが必要なときに読み込めるようにします。基本的には、スキル名を付けたフォルダーに `SKILL.md` を置きます。Markdown には、スキル名と説明を定義する frontmatter、概要、スキルを呼び出す状況のガイダンスを記述します。必要に応じてスクリプトやその他のリソースをサブフォルダーに含められます。

> [!NOTE]
> 追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクト既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルをチームで共有して再利用する場合はプロジェクトの `.github/skills` フォルダーに、個人用にする場合はユーザーの `~/.copilot/skills` フォルダーに配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は、いつスキルを呼び出すかを Copilot が理解するのに役立ちます。
3. 指示を読み、テストと lint のプロセスがどのように示されているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot にスキルの3つのチェックを実行してもらいます。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したコマンド、成功したかどうか、ツールが報告した内容を示すレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前と同じプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 結果レポートを読み、最初のレポートと比べます。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次の作業を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果レポートの形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能の PR に含めます。次は、[Playwright MCP サーバーを使って][next-lesson]、Copilot にサイトを直接操作させます。

## スキルの参考例

以下のコミュニティ例は参考情報であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
