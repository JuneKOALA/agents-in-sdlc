---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の改善や機能追加に関する3つのタスクを GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進行状況を確認することも重要です。

cloud agent に割り当てた作業は、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープン中のプルリクエスト (PR) があるエージェントタスクを一覧できます。実行された操作を確認したり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の作業を行います。

- cloud agent のタスクを確認するため、エージェント ページを見ます。
- 実行中のセッションに追加の機能を依頼します。

## シナリオ

ハイコントラスト モードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと気付きました。サイトのスタイルと切り替え機能を更新しているため、これも同時に実装するのが合理的です。ハイコントラストに加えてライトモードも追加されるよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の領域にあるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメント更新。
   - ゲームを変更する API の生成。
   - Web サイトのハイコントラスト モード追加。
3. 実行中のタスクを1つ選び、Copilot が実行した操作を確認します。たとえば、次のような操作があります。
   - リポジトリのチェックアウト。
   - Copilot 用環境の作成。
   - MCP サーバーの設定。
   - 割り当てられたタスクを完了するための各種作業。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) の領域も確認します。PR と変更されたファイルを確認し、さらに進捗を監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイコントラスト モードの作業と並行してライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラスト モードを追加しているセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページの cloud agent タスクで、作業中のセッションに指示を送るダイアログを選択した状態](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に加えたことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを受け取った時点で進めていた手順が完了した後に、新しい依頼を作業に加えます。

完了までには数分かかります。その間に、これまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、方向付ける中心となる Copilot agents ページを確認しました。この mission control では、実行中と完了済みのタスクを追跡し、実行内容をレビューし、進行中のタスクの範囲を調整したり追加の指示を送ったりできます。

この演習では、次の概念を学びました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、作成されたプルリクエストを[反復して改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
