---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな変更でも、通常は複数のファイルの更新やテストの実行などが必要です。開発者は必要なファイルを探し、変更を加え、テストを実行し、デバッグして、更新し忘れたファイルを見つけ、再び変更します。作業は次々と続きます。

ここで Copilot のエージェント モードが役立ちます。

エージェント モードは IDE 内でより自律的に作業します。開発者と同様に、まず既存のプロジェクト構成を調べ、必要な変更を加え、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って、サイトに新しい機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる種類の作業向けに作られています。エージェント モードは IDE 内で作業するため、すばやくフィードバックを受けながら対話できます。cloud agent はチームの一員のように非同期に作業し、Issue とプルリクエストを通じてやり取りするペア プログラマーです。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調査し、関連ファイルを特定して、複数のファイルに連携した変更を加える方法。
- GitHub Copilot エージェント モードが UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたので、ユーザーがカテゴリーと publisher で絞り込めるようにします。前の演習で publisher ヘルパーを追加しました。ここでは Copilot エージェント モードを使って、データ層、UI、テストの残りの作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認しましょう。

このサイトは、開発者向けテーマのボードゲームを扱うクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を使い、フロントマターからローカル SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習が完了していれば、`main` に戻っています。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用に新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して、Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバー アドレス `http://localhost:4321` を選択するとき、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace へのプライベート トンネルで、Codespace が Web サーバーをホストしています。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリーなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な手順ではその都度説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたとき、Issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。**Agent** は自律的に計画し、複数ファイルの変更、ターミナル コマンドの実行、ツールの呼び出しを行います。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用できるモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、指示にも記載しています。提案内容の一貫性を保ち、学習しやすくすることが目的です。ただし LLM は確率的に動作するため、ワークショップの例とは異なる提案が表示されることがあります。これは正常であり、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーにとって特に有用だと判断した項目を優先していることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを伝える指示ファイルを確認しましょう。Web サイトへの機能追加方法を説明する [user interface (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示へのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、複雑な指示を再利用できる小さな単位に分割し、例やテンプレートを提供できます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) のフィルタリング ロジックの残りを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページを更新して、フィルタリング UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功する必要があります。Copilot エージェント モードにこれらの作業を任せましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue の要件に沿って機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって数分かかる場合があります。作業中にコード エディターに **Keep** と **Undo** が表示されることがあります。作業が完了すると、すべての変更に対して **Keep** または **Undo** を選べるため、作業中に選択する必要はありません。

7. Copilot の確認に応じて **Continue** を選択し、テストを実行します。

   ![Copilot Chat パネルでテスト実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが検出されなくなるまで作業します。

   ![Copilot エージェント モードの完了したチャット セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウの **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードで Tailspin Toys Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェント モードで UI とデータ層に新機能を実装する方法。
- Copilot エージェント モードでプロジェクトを調査し、関連ファイルを特定して連携した変更を加える方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の探索演習 — ページ分割を実装する

ゲームの一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだことを活用し、Copilot にサイトのページ分割を依頼してください。次の点を考慮します。

- 指示ファイルを使うなど、既存のベスト プラクティスに従う。
- ページ サイズをユーザーが選べるようにするか、固定値にするかなど、ページ分割の実装方法を検討する。
- 希望どおりの実装になるよう、プロンプトで Copilot に必要なガイダンスを提供する。
- 変更を依頼したりコンテキストを追加したりして、GitHub Copilot と反復的に作業する場合がある。Copilot を使うときの通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 各モードの役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
