---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを学びます。

- コード生成用の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確にした Issue の重要性を理解する。
- Copilot に Issue を割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを開発した請負業者がドキュメントを適切に整備しなかったため、コードの説明がほとんどありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリの別のゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。現在は作業を妨げる要因になっていますが、より優先度の高い Issue もあります。

どちらも優先順位が下がりやすいタスクであり、Copilot cloud agent に任せるのに適しています。エージェントに非同期で作業させれば、開発者は別の作業に集中し、後から戻って結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行します。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を開始できます。割り当てると、Copilot は進捗を記録するドラフトプルリクエストを作成し、環境を準備してタスクを開始します。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要な理由

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に設定してください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業して、学び、試し、必要に応じて調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を完了した場合は `.github/copilot-instructions.md` を見たことがあるでしょう。まだの場合は、ここで確認してください。cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後でその影響を把握できます。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントに関する要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript の規約、コメントヘッダーが含まれているか確認してください。これらは指示ファイルで定められています。後の演習で PR をレビューするときにも改めて確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は特定の環境と、環境を整えるセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェアエンジニアと同じように作業します。

Cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成し、必要な開発ツールや依存関係を用意できます。学習の流れをスムーズにするため、このワークフローはあらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、マイグレーションとシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot の操作には専用トークンが使われます。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする場合（依存関係のインストールなど）は、`contents: read` 権限が必要です。クローンしない場合は、セットアップ完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は現在 Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、シードデータを追加します。ビルドとテストで利用します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブを含みます。このジョブは Copilot が PR で作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、ワークフローの成功をテストできます。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していますが、多くのプロジェクトでは情報が古いか、十分に整備されていません。こうした技術的負債は放置されやすく、開発の効率を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成が得意なため、これは Copilot cloud agent に割り当てるのに適した Issue です。エージェントはバックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧にない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態を確認するため、画面を再読み込みする必要がある場合があります。

16. ライブセッションをスクロールし、Copilot がどのように作業しているか確認します。コードを調査して状態を把握し、計画を考え、コードを作成する様子を確認できます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的は非同期にタスクを実行し、開発者が別の作業に集中できるようにすることです。この機能を活用して、もう1つタスクを cloud agent に割り当て、その間にアプリケーションの機能を追加します。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の一つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は時間がかからないかもしれませんが、その時間を別の作業に使うこともできます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように動作するため、常に監視する必要はありません。作業が完了した後にレビューすればよいので、次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、非同期で実行する Issue を割り当てられることを学びました。技術的負債の解消、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを Copilot に任せられます。

この演習では、次の概念を学びました。

- コード生成用の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確にした Issue の重要性を理解する。
- Copilot に Issue を割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
