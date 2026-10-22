---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーのフィルタリング機能を確認しました。次に、これらの機能を、明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選択し、フィルタリング Issue に照らして結果をレビューする。

## シナリオ

Tailspin Toys はプルリクエスト (PR) の作成前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使って QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門バージョンです。プロファイルにエージェントの目的、指示、利用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、Copilot が要件を評価して結果を報告する際に、これらの機能をどう使うかを説明します。既存の機能に置き換わるものではなく、別の会話が必要になることもありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実行するチェックと守るべき制約を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送信して、新しいカスタム エージェントを作成します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA ワークフローと制約を適切に反映したか確認します。これにより、検証だけを依頼したつもりなのにエージェントが機能を変更してしまうことを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件を出発点とし、リポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を使用することを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に質問し、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. 責任や制約に不足があれば、続行する前に既定のエージェントにプロファイルの修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイル作成後に同じフィルタリングの会話を再開し、QA を選択します。これにより、すでにコンテキストにある Issue と計画の決定事項を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、選択肢の一覧に表示されないことがあります。それでも存在するため、上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、生成されたレポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、作業結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認した。
- 再利用可能な品質保証プロファイルを作成して確認した。
- QA エージェントを選択し、フィルタリング Issue に照らして結果をレビューした。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
