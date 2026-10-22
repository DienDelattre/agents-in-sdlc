---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。このミッション コントロールでは、オープンなプルリクエスト (PR) があるエージェント タスクをすべて確認できます。実行内容を詳しく見たり、進行中のセッションに指示を出して作業を導いたりできます。

このレッスンでは、次のことを行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに指示を出し、機能を追加で依頼します。

## シナリオ

チームはハイ コントラスト モードの作成を割り当てた後、ライト モードも追加することにしました。サイトのスタイル変更と切り替え機能を実装しているため、ライト モードも含めるのが自然です。エージェントにハイ コントラストだけでなくライト モードも実装するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン パネルと左側のパネルにあるタスクの一覧を確認します。次のタスクなど、Copilot に割り当てたタスクが表示されます。
    - コードベースのドキュメントを更新する
    - ゲームを変更する API を生成する
    - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような作業が表示されます。
    - リポジトリからコードをチェックアウトする
    - Copilot が作業する環境を作成する
    - MCP サーバーをセットアップする
    - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) パネルも確認します。ここで PR と変更されたファイルを確認し、進行状況をさらに把握できます。

## cloud agent に指示を出す

実行中のタスクを確認できたので、エージェントがハイ コントラスト モードを実装している間にライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページで、cloud agent のタスクと Steer active session while Copilot is working ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が完了すると、新しい依頼を作業に組み込みます。

今回も数分かかります。その間に、ここまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページを確認しました。このミッション コントロールを使うと、実行中または完了したすべてのタスクを追跡し、作業内容をレビューし、進行中のタスクに追加の指示を出せます。

次のことを確認しました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視する
- 実行中のセッションに追加機能を依頼する

Copilot がアクセシビリティ機能を完成させている間に、Copilot が作成したプルリクエストを[反復的に改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
