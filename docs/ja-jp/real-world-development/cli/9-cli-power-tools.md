---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、オプションのクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同様に、GitHub Copilot CLI にも操作用のスラッシュ コマンドが多数用意されています。コマンドを使うと、高度な機能、内部の情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使って、利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に関するオプションのコマンドを確認する。

## シナリオ

CLI の基本的なワークフローを完了しました。次に、コンテキスト管理、モデルの切り替え、セッション共有、オプションとして [Copilot cloud agent][about-cloud-agent] に作業を委任する機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルが扱えるコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的にコンパクト化します。スラッシュ コマンドを使って状態を確認したり、自分でコンパクト化したりできます。

1. Codespace に戻り、まだ起動していなければリポジトリ ルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話をコンパクト化します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、大きな変化は見られないことがあります。

> [!NOTE]
> Copilot CLI はコンテキスト ウィンドウが埋まると、自動的にコンパクト化します。タイミングを選びたい場合は `/compact` を使います。無関係な別のタスクに切り替えるときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルにはそれぞれ異なる強みがあり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示して使うモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業して学んだことを共有すると、AI ツールの使い方を改善できます。`/share` コマンドは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合う出力先を選びます。たとえば `/share file` を使うと、ローカルの Markdown ファイルにエクスポートできます。
3. エクスポートした内容を他の人に送ったり公開したりする前にレビューします。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並列で実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送り、非同期で作業してもらいます。エージェントがプルリクエストを作成する場合があります。

これらのコマンドは worktree やリモート作業を追加する可能性があるため、オプションとして扱っています。試す場合は、範囲を明確にした新しいタスクを始め、通常のワークフローで結果をレビューしてください。非同期エージェントについて詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定の変更、セッションの共有、内部の動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` でコンテキストの管理方法を確認した。
- `/model` で利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。最後に、学んだことと次のステップを[振り返ります][next-lesson]。

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
