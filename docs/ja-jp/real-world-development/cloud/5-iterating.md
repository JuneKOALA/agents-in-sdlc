---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加に関する複数のタスクを GitHub Copilot と進めました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コード変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善してもらいます。

### シナリオ

これまで何度も説明してきたように、生成 AI を加えてもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進める必要があります。チーム内でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視なしでタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、コード用に使用するブランチ**のみ**への書き込みアクセスを持ちます。
- Cloud agent は GitHub Actions 内で動作し、作業専用の一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードにドキュメントを追加する変更を確認します。GitHub.com の標準の PR インターフェイスを使います。

> [!NOTE]
> PR を開くと、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。Copilot は既定で外部リソース (外部 MCP サーバーへの呼び出しを含む) へのアクセスが制限されているため、これは**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR 一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。その場合は Copilot が作業を終えるまで待ちます。数分かかることがあるので、休憩したり、ここまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![変更されたファイルのタブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントなど、更新されたコードを確認します。具体的な変更は異なる場合があります。

   変更を確認するときは、演習 2 の最初に確認した指示ファイルに基づく TSDoc コメント、TypeScript の規約、コメントヘッダーを探します。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローの承認を求められている場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チームメンバーに依頼する場合と同様に、PR やコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的であるため、特定の文面を指定することはできません。次のような変更を依頼できます。

- 各コードファイルの先頭に、そのファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の案や、コードベースのドキュメントに関する別の提案を使って、**@copilot** をメンションし、生成されたドキュメントの変更を依頼するコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。更新用に新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復してかまいません。満足したら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を確認します。
7. ワークフローの承認を求められている場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり (この作業はカスタム指示の更新前に割り当てたため) コメントヘッダーや TSDoc コメントの追加を依頼したりできます。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 で mission control から追加依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。その場合は Copilot が作業を終えるまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持する local storage の使用。
   - ハイコントラストとライトモードの CSS またはスタイル変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローの承認を求められている場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。ターミナルで、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みした後も設定が保持されることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装をガイドしたことを確認してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

実際のバックログ項目を cloud agent に任せ、自分は別の作業に集中する方法が効果的です。習慣を身に付けるため、リポジトリに Issue をいくつか作成して Copilot に割り当ててみましょう。たとえば次のようなタスクがあります。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。各レッスンを通じて、次の作業を行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力に及ぼす影響を確認できるようにしました。
- **Issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使用し**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティ セッションを監視して方向付けました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで関連する内容を実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` 連携を使って、ターミナルから2つの変更をレビューして提供します。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジに関する Issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
