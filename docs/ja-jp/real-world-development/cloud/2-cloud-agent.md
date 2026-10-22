---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ課題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどが考えられます。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズする
- 安全に操作するための方法を確認する
- Issue の範囲を明確にする重要性を学ぶ
- Issue を Copilot に割り当てる

## シナリオ

Tailspin Toys には対応したい技術的負債があります。サイトの初期開発を担当した請負業者が、ドキュメントを整備しないまま作業を終えました。まず、アプリケーションのエクスポートされた関数すべてに TSDoc ドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリーの別タイトルを関連ゲームとして表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要ありません。UX の受け入れテストに使える機能を求めています。この作業は現在ブロッカーですが、より優先度の高い Issue もあります。

どちらも優先順位が下がりやすい作業です。Copilot cloud agent に割り当てれば、非同期に処理を進めている間、開発者は別の作業に集中できます。その後、結果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法があります。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取り組みます。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な操作でも、細部を確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を明確にしてください。AI ペア プログラマーとして扱い、段階的に作業し、学び、試しながら調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示のハンズオンを完了している場合は、`.github/copilot-instructions.md` がどのように使われるか確認済みです。まだの場合は、ここで簡単に確認しましょう。cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認すると、後でその効果を確認しやすくなります。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc ドキュメント コメントの要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示が `**/*.test.ts` に適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、生成されたプルリクエストに TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーが反映されていることを確認できます。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は特定の環境が必要です。環境を準備するスクリプトも実行し、すべて正常に動作することを確認します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアと同じように作業します。

cloud agent は GitHub Actions を作業環境として使います。作業を始める前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、環境をカスタマイズできます。これにより、必要な開発ツールと依存関係を利用できるようになります。このラボでは学習をスムーズに進められるよう、事前に設定済みです。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、移行とシードを済ませた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *このジョブの手順*に必要な最小限の権限を設定します。Copilot は操作用の独自のトークンを使用します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップでクローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが知っていますが、情報が古いままのプロジェクトや、ドキュメントがまったくないプロジェクトは少なくありません。このような技術的負債は対応されないことが多く、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に依頼するのに適した Issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に To Do リストを作成するまで数分かかることがあります。先ほどのワークフローを実行して環境を準備し、プロジェクトを分析し、問題に取り組む方法を決定しているためです。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新後のインジケーターを表示するために、ブラウザーを更新する必要がある場合があります。

16. 実行中のセッションをスクロールして、Copilot が問題に取り組む様子を確認します。コードの調査や状態の把握、計画を考えて決定する過程、コードの作成などが表示されます。

数分かかることがあります。Copilot cloud agent の主な利点は、タスクを非同期に実行して、その間に別の作業ができることです。この機能を活用し、cloud agent にもう 1 つのタスクを割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けられることです。デザイン チームのために関連ゲームのセクションを追加する作業は長時間かからないかもしれませんが、その時間を別の作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![目の絵文字で Issue に取り組んでいることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同様に動作するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタム エージェントを作成して使います。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当てると、非同期に作業を進めてもらえます。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを依頼できます。

次のことを確認しました。

- コード生成環境をカスタマイズする
- 安全に操作するための方法を確認する
- Issue の範囲を明確にする重要性を学ぶ
- Issue を Copilot に割り当てる

cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
