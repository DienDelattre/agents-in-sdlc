---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送ブラウザーで確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、最初の変更をプロジェクトに加えましょう。変更は小さくします。ゲーム データにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示してもらい、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始する。
- エージェントにプロジェクトの小さな変更を依頼する。
- `/diff` で変更をレビューする。
- アプリを実行し、転送されたブラウザーで変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys のゲームには星評価が設定されていて、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードにはタイトル、カテゴリー、publisher、説明しか表示されません。まずエージェントに既存の評価を各カードに表示してもらいます。最初のセッションで試すのに適した、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話は専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話に戻って同じ話し合いを続けられます。ファイルとブランチは、ディスク上の通常の Git 状態のままです。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能を実装します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. `/new` スラッシュ コマンドを入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを送信して変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを特定し、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルすべてを確認します。
2. 評価がある場合は数値で表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方の状態がテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが完了したら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同じように、Copilot は確率的に動作するため、コードは例と異なる場合があります。特定の実装と一致するかではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで、正常に動作すると判断してはいけません。Copilot に Web サイトを起動してもらい、Codespaces から転送されたブラウザーで更新後のユーザー インターフェイス (UI) を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能が完成しました。新しいコードをプロジェクトに取り込むプルリクエスト (PR) を作成します。

1. 既定のエージェントに次のプロンプトで変更をコミットしてもらいます。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。次のことを行いました。

- 機能ブランチで Copilot の会話を開始した。
- ゲーム カードに小さな変更を加えるようエージェントに依頼した。
- `/diff` で変更をレビューした。
- アプリを実行し、転送されたブラウザーで星評価を確認した。
- 最初のプルリクエストを作成してマージした。

次は [フィルタリング Issue から作業を始め、Plan モードと Autopilot モードを使って][next-lesson]大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
