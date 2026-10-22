---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を検証し、指示ファイルを使って標準に従うようにできました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルは、テストなどの操作を適切に実行する方法を Copilot が理解できるようにします。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認する。
- 結果のレポート形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys は、ユニット テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、必要に応じてエージェントが読み込みます。スキルの基本は、スキル名を付けたフォルダーと、その中の `SKILL.md` ファイルです。Markdown ファイルには、スキルの名前と説明、概要、呼び出すタイミングのガイダンスを定義するフロントマターが含まれます。呼び出されたときにスキルが使うスクリプトやその他のリソースを、サブフォルダーに追加することもできます。

> [!NOTE]
> スキルにほかのフォルダーやファイルを追加する必要はありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使用するため、`SKILL.md` だけで構成されています。

スキルは、チームが共有して再利用できるリポジトリ内の `.github/skills` フォルダー、またはユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームがユニット テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は、Copilot がいつこのスキルを使うか判断するために役立ちます。
3. 指示を読み、テストと lint のプロセスで Copilot をどう導くか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。3 つのチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したかどうか、ツールが実際に報告した内容が分かるレポートが必要です。レポートを作成するようにスキルを更新しましょう。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、動作を確認します。Copilot CLI では、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと**同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェックを実行する `quality-checks` スキルを確認した。
- 結果のレポート形式をカスタマイズした。
- スキルを再読み込みして実行した。

フィルタリング機能の PR にもこの変更が含まれます。次は [Playwright MCP サーバーを使って][next-lesson]、Copilot にサイトを直接操作してもらいます。

## スキルの例

次のコミュニティの例は参考用です。追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
