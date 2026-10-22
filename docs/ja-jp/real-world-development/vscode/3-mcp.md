---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルタリング機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーでフィルタリング機能を手動テストするよう依頼する。
- ブランチを公開して、フィルタリング作業のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部に接続できると、より強力になります。Copilot は Model Context Protocol (MCP) を使って外部ツールやサービスに接続します。MCP は、エージェントが外部ツールやサービスと通信するための標準規格です。

![エージェント モードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールを含むコンテキスト、LLM、ツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部ツールやサービスと通信できます。これにより、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わる操作を実行したりできます。

MCP サーバーは AI エージェントと外部ツールやサービスとの間の橋渡しをします。既存の API や NPM パッケージなどのローカル ツールと、AI エージェントとの通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできる異なるツールとリソースのセットを提供します。

よく使われる MCP サーバーには次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。リポジトリの作成や更新、Issue とプルリクエストの管理などを AI エージェントが行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントが行えます。

ほかにも、さまざまなツールやリソースにアクセスする MCP サーバーがあります。GitHub は、エコシステム内のツールの発見と貢献を促進する [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーは他の依存関係と同じように扱ってください。MCP サーバーを使う前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を考慮してください。信頼できる MCP サーバーだけを使用し、機密リソースや操作へのアクセスを許可する場合は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。今回の機能をテストするために使用します。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、Playwright に画面を表示しないブラウザーを実行するよう指示します。デスクトップ画面がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使っているため、必要なブラウザーは通常すでにインストールされています。後で Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してもう一度試すよう依頼してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要になったときに**起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。これを選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認して信頼し、サーバーを起動します。

手動での起動は省略できます。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開いて移動し、フィルターを適用して結果を確認し、観察した内容をまとめます。会話を離れずに機能が期待どおり動作することを確認する最も速い方法です。

Playwright MCP サーバーは、画面のスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ラベル付きのボタン、リンク、リスト項目など、構造化された要素を扱います。そのため、簡単な機能チェックとあわせて、アクセシビリティの基本的な確認もできます。

サーバーを接続してアプリを起動した状態で、作成したフィルタリング機能を Copilot に操作してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、手順を実行して結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせてください。問題があれば、フォローアップの質問をしたり、プルリクエストを作成する前にコードを修正するよう依頼したりします。

> [!NOTE]
> このテストを実行するには、アプリを `http://localhost:4321` で起動しておく必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームにレビューを依頼するプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が変更した内容を確認します。
2. **+** アイコンを選択して、変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot の変更内容が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot を使って作成します。Copilot は関連する Issue を探し、Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるように、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを送信して、Copilot に PR を作成してもらいます。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報の収集や操作を行うときは、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは GitHub への認証が求められる場合があります。画面の手順に従って認証してください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- ブラウザーでフィルタリング機能を手動テストした。
- ブランチを公開して、フィルタリング作業のプルリクエストを作成した。

機能の**テスト**に MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次に、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か。なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
