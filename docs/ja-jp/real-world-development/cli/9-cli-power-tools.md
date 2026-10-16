---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様、GitHub Copilot CLI にも操作に使うスラッシュコマンドが多数あります。これらのコマンドでは、高度な機能や内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する仕組みを確認します。
- `/model` を使って、利用できるモデルを確認します。
- `/share` でセッションを書き出したり共有したりする方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の基本ワークフローを完了しました。次に、コンテキスト管理、モデル切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加の機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルのコンテキストウィンドウの上限に近づく場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って自分で確認したり、圧縮したりできます。

1. Codespace に戻ります。まだ Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように割り当てられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを自分で決めたい場合は `/compact` を使います。関連のない別のタスクに切り替えて会話を新しくする場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルとして書き出したり、共有可能なリンクや GitHub gist を作成したりできます。

1. `/help` を入力し、インストール済みバージョンで `/share` が利用できるオプションを確認します。
2. セッションを共有する場合は、`/share file` でローカルの Markdown ファイルに書き出すなど、適切な方法を選びます。
3. 書き出した内容を、他の人に送ったり公開したりする前に確認します。セッションの書き出しには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を並列化、または委任する

ワークショップの主要な作業は完了しました。Copilot CLI には、大きなタスクに役立つコマンドも用意されています。

- `/fleet` を使うと、独立したサブタスクを複数のサブエージェントに割り当て、並列実行できます。
- `/worktree` を使うと、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` を使うと、非同期に動作する Copilot cloud agent にタスクを送信できます。cloud agent はプルリクエストを作成する場合があります。

これらのコマンドは新しい作業やリモート作業を発生させる可能性があるため、任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期のエージェント作業をさらに学びたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する仕組みを確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションを書き出したり共有したりする方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、まだ多くのスラッシュコマンドがあります。学んだことを[振り返り][next-lesson]、次の学習方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
