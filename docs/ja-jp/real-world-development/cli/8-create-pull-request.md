---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキル更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを 1 つのブランチに保存しました。まとめてレビューしてプルリクエストを作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理してもらいます。

> [!NOTE]
> 通常、機能、指示の更新、スキルの更新、QA エージェントは、複数の PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質ワークフローの作業を 1 つの会話とブランチにまとめ、1 つの PR に含めます。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学ぶ。
- 機能変更全体と検証結果を確認する。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリング機能の作成を通じて、Copilot に計画、実装、検証を依頼しました。Tailspin Toys は、マージの承認を開発者が管理しつつ、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能な状態にするために必要な作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど）に対処し、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができます。有効にする前に変更をレビューし、準備ができていると判断してください。

## Agent Merge を使って PR を管理する

すべてのコードが作成されたので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. 「PR #14 was squash-merged successfully.」のような Copilot のメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合に、レビュー済みの手動マージを完了してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発の複数の作業を自動化しました。次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学んだ。
- 機能変更全体と検証結果を確認した。
- フィルタリングの PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に使える[便利な Copilot CLI スラッシュ コマンドを確認][next-lesson]します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
