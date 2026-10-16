---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。次のレッスンでは、その統合ターミナルを使って Copilot CLI をインストールし、実行します。

## ラボ用リポジトリを設定する

自分用の Tailspin Toys プロジェクトで作業します。[テンプレート リポジトリ][tailspin-template]から今すぐ作成してください。新しいリポジトリにはラボに必要なすべてのファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. **Use this template** を選択し、続けて **Create a new repository** を選択して、自分用のリポジトリを作成します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてこのワークショップを進めている場合は、メンターから案内された手順に従ってください。そうでない場合は、GitHub Copilot にアクセスできる organization に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後でラボ内で参照するため、作成したリポジトリ パス（**organization-or-user-name/repository-name**）を控えておいてください。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通してこれらの Issue を使うため、自分で起票する必要はありません。

テンプレートの新しいコピーを使ってください。リポジトリには、リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、演習で使うバックログが含まれます。古いコピーを使う場合は、必要なファイルがあるかどうかを進行役に確認してください。

## Codespace を作成する

次に Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境であり、ブラウザーから直接コードの作成、実行、デバッグを行えます。複数のプログラミング言語、拡張機能、ツールに対応したフル機能の IDE を提供します。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりははるかに速く完了します。その間に、次に扱う GitHub Copilot のほかの機能を確認しておくこともできます。

4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. リポジトリ ルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択し、Tailspin Toys が読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

> [!CAUTION]
> 後続の演習で codespace に戻ります。いまのところは、ブラウザーのタブで開いたままにしておいてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] 内で実行する前提で作られています。どちらでも、必要な前提条件がすべてインストールされた環境を用意できるため、スムーズに進められます。ローカルで実行したい場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。VS Code が、codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
## まとめ

準備ができました。このレッスンでは、Tailspin Toys プロジェクトの自分用コピーを作成し、Codespace を作成して、プロジェクトが準備できたことを確認しました。

## 次のステップ

次は、Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces クイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
