---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の開発ワークフロー全体に取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調査し、用意されたフィルタリング Issue を確認した。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージした。
- フィルタリング Issue から始め、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューした。
- カスタム指示を追加し、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認した。
- QA カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価した。
- フィルタリング変更全体をレビューし、Agent Merge にフィルタリング PR のマージを許可した。
- コンテキスト、モデル、共有、オプションのクラウドへの委任に使うスラッシュ コマンドを確認した。

## リリースした機能

ワークショップでは、更新された `main` からそれぞれブランチを作成し、2 つの PR をマージしました。

1. **星評価:** ゲーム カードに既存の `starRating` と、未評価であることを示す状態を表示する。
2. **フィルタリングと品質ワークフロー:** フィルタリング機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして、QA プロファイルと関連テストを追加する。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは PR の前に、要件とテスト カバレッジのレビューを含め、結果をまとめました。

追加のテストは、本当に不足している箇所を補うために作成します。QA を実行した結果、新しいテストが不要な場合もあります。マージを許可する前にコードと証拠を確認し、変更後は影響を受ける証拠を再確認します。

## ベスト プラクティス

Copilot に提供するコンテキストとツールが、作業の内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し行うタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を考えるとき、**Interactive** は変更を確認しながら作業するとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に説明すると、出力が大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、ほかにも便利な機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を説明し、別の視点から意見を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、長年作ろうと思っていた小さなアプリなどに活用してください。学んだことをチームで共有し、チームの経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境での作業と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

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
