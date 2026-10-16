---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルター機能 Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から開始し、Plan モードで方針を定め、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証根拠を確認しました。
- フィルター機能全体の変更をレビューし、フィルター機能の PR に Agent Merge を使うことを承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任用のスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` から作成した個別のブランチで、2つの PR を作成しました。

1. **星評価**: 既存の `starRating` と、評価がないことを明示する表示をゲームカードに追加しました。
2. **フィルターと品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加しました。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、関連作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは、ユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA では、これらの結果に要件とテストカバレッジのレビューを加えて PR 前に確認しました。

追加するテストは、実際に不足しているカバレッジを補うものにします。QA 実行で追加のテストが不要という結果になる場合もあります。マージを承認する前に、コードと根拠を確認し、変更後は影響する根拠を再確認してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールによって、作業結果は変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し行う作業を記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけではなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の方針決定、**Interactive** は対象を絞った変更を対話しながら進める場合、**Autopilot** は範囲が明確な作業に適しています。日常的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。*何を*、*なぜ*、*どのように*構築したいかを明確に伝えると、結果は大きく変わります。

## さらに試す

主要なワークフローを完了しました。次の CLI 機能も確認できます。

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について対話し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで活用方法が上達します。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで試してみてください。学んだことをチームで共有し、ほかの人の学びからも知識を得ましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境を試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
