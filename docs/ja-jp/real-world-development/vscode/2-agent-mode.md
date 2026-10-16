---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な変更でも、通常は複数のファイルを更新し、テストを実行するなどの作業が必要です。開発者は必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを見つけてさらに更新します。こうした作業は続いていきます。

このような場面で役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者のように既存のプロジェクト構成を調査し、必要な更新を行い、テストなどのタスクを実行し、問題が見つかれば自動で修正します。Agent Mode を使ってサイトに機能を追加してみましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる体験を提供します。エージェントモードは IDE 内でタスクを実行するため、短いフィードバック サイクルで対話できます。Cloud agent はチームの同僚のように非同期で作業し、Issue とプルリクエストを通してやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、連携した変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層をまたぐ新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、ユーザーがカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。ここでは Copilot Agent Mode を使い、データ層、UI、テストの残りを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。Astro アプリはビルド時にページを静的 HTML として生成します。ページは frontmatter で Drizzle ORM を使い、ローカル SQLite データベースに直接問い合わせます。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の作業を終えて `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択してサイトを開きます。

> [!NOTE]
> codespace ターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは codespace でホストされている Web サーバーへのプライベート トンネルです。

### Web サイトを確認する

サイトを開いたら、主な機能を確認します。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトを設定したときに Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。**Agent** は自律的に計画を立てて複数ファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、体験をそろえるため手順にも記載しています。大規模言語モデルの動作は確率的なので、提案内容が説明と異なる場合があります。これは正常であり、想定される動作です。

> [!NOTE]
> 確率的に動作するため、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目に基づいて、優先順位も付けたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトへの機能追加方法を説明する[ユーザー インターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - ファイル形式別の指示へのリンク (Astro、Tailwind CSS、Drizzle など)。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより指示を再利用しやすくし、複雑な指示を小さく分け、参照用の例やテンプレートを指定できます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3つの更新が必要です。

- データ層 (`src/lib/`) のフィルター ロジックを追加または改良する。
- フィルターの動作に関するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、すべて成功する必要があります。Copilot Agent Mode にこれらの作業を任せます。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選び、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要なファイルを探して、コードを変更しテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行います。タスクやコードベースの複雑さによっては数分かかります。作業中は、コードエディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了すると、すべての変更に対して **Keep** または **Undo** が表示されるので、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat がテストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで進めます。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードをレビューすることは重要です。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに新機能を追加する方法を学びました。具体的には、次の方法を確認しました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して連携した変更を行う。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションを実装してもらいましょう。次の点を検討してください。

- 指示ファイルを使うなど、既存のベストプラクティスに従う。
- ページサイズをユーザーに選ばせるか、固定値にするかなど、ページネーションの実装方法を決める。
- 希望どおりの実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更を依頼してコンテキストを伝えながら、反復が必要になる場合がある。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
