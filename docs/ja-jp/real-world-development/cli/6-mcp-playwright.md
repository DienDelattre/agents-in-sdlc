---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または構成し、ブラウザーでフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要もあります。MCP サーバーを使うと、Copilot に CLI 組み込み以外のツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解する。
- まだ利用できない場合は Playwright MCP サーバーを追加する。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認する。

## シナリオ

ユニット テストやエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot に開発中の Web サイトをユーザーと同じように操作させ、変更を自動化して、期待どおりに動く確度を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部ツールやサービスと通信できます。MCP により、AI エージェントはリアルタイムで最新情報にアクセスし、ユーザーに代わって操作できます。

MCP サーバーは AI エージェントと外部ツールやサービスとの間の橋渡しをします。各 MCP サーバーは、AI エージェントがアクセスできる異なるツールとリソースのセットを提供します。

よく使われる MCP サーバーには次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも、さまざまな MCP サーバーがあります。GitHub は、エコシステム内のツールの発見と貢献を促進する [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。使う前にソース コードを確認し、発行元を検証して、セキュリティへの影響を考慮します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ使用できない場合は、Copilot が Web サイトをユーザーのように操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

前の手順では、機能が期待どおりに動作することを手動で確認しました。次は、Copilot の Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを送信し、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、どの MCP サーバーを使うか自動的に判断するため、明示的に指定する必要はありません。ただし利用するサーバーが分かっている場合、方向を示すと結果が安定し、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を見ます。
3. 報告を読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学んだ。
- まだ利用できない場合に Playwright MCP サーバーを追加した。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認した。

次は、スキルとブラウザー ツールを専門的な役割にまとめる[QA カスタム エージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か。なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
