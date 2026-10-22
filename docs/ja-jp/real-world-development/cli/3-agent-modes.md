---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。大きな変更には、より堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存ワークフローに沿って作業できるため、適切な方法で必要な機能を作成できます。このレッスンは、一般的なエージェント主導の開発プロセスを進める一連のレッスンの最初です。Issue から新機能を作成し、コードが有効で期待どおりに動作することを確認して、プロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話とブランチを使います。通常、ファイル種別ごとにブランチや PR を分けますが、ここでは中核的な概念に集中するため、手順を簡略化しています。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot 会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

引き続き機能を作成する過程で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズして、MCP で検証し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリーや publisher でゲームを絞り込める必要があります。バックログの Issue に機能の概要は記載されていますが、複数カテゴリーの組み合わせなど、実装前に合意が必要な詳細もあります。Plan モードで決定し、Autopilot に範囲を定めた実装を許可します。

## 背景

AI コーディング エージェントを開発ワークフローに取り入れても、基本は変わりません。むしろ、基本がいっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を説明する Issue から始める。
2. 実装内容を計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードのレビューと継続的インテグレーションが成功した後、コードをマージする。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くは上記の流れを基本としています。

この標準的なアプローチに沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ審査プロセスを経るようにします。

## 会話モード

**会話モード**は、エージェントに与える自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、ユーザーの入力を待ちながら一緒に作業します。
- **Plan**: エージェントが最初に計画を作成します。計画中はプロジェクト ファイルを編集できません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` などの番号から始まることを確認します。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に任せる前に、機能の実装やタスクの実行方法を決められます。複雑な作業では、少し時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作ってもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot が計画を作成します。まずプロジェクトを調査し、その後、最適な方法を決定します。

3. フィルタリング機能の動作について Copilot から質問される場合があります。希望に応じて回答してください。間違った答えはありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して、計画を展開します。
5. 上下にスクロールして計画をレビューします。
6. 決定した内容に合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot がタスク完了と判断するまで問題に対して反復的に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同等の選択肢を選びます。
2. プロンプトの下のモード表示が **Autopilot** であることを確認します。
3. Copilot が作成した計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係がない、またはポートが競合していると報告された場合は、チェックが完了したと判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同じように、マージ前にレビューします。コードを確認し、サイトを起動して問題がないことを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力して、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時に決定した内容と比較します。
4. コードのレビュー後、<kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題はなさそうですが、実際に動作するでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリー フィルター、publisher フィルター、組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを使い分けて機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot 会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、コード生成が文書化された慣行に従うよう、[カスタム指示を使って][next-lesson]詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
