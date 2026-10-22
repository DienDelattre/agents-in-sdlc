---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリング コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。タスクの進め方に決まりがある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]を使うと、必要なコードの内容だけでなく、構成方法も伝えられます。フィルタリング機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認する。
- コーディング標準を守るため、指示ファイルを更新する。
- 指示ファイルがコードに及ぼす影響を確認する。

## シナリオ

優れた開発チームとして、Tailspin Toys には開発上の慣行に関するガイドラインと要件があります。

- コメントでは、コードの内容を言い換えるのではなく、意図や自明ではない判断を説明する。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` 引数が注入される場合を含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明する。
- 再利用可能な Astro コンポーネントでは `Props` の仕様を説明し、関連コードが変わったらコメントも更新する。
- 既存のフォーマットと lint のガイダンスを維持する。

指示ファイルを使って、Copilot がこれらの慣行に沿ってタスクを実行するために必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に伝えて、コーディング スタイルや要件を理解しやすくできます。推奨するコーディング規約やライブラリ、コードに含めるコメントの形式などを指定できます。リポジトリ全体、または特定のファイル種別のタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一のファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを記載するファイルです。

> [!NOTE]
> ほかの形式の指示ファイルや対応状況は、環境によって異なります。特定の形式を前提にする前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

このスターター プロジェクトには、すでに指示ファイルが含まれています。変更の影響を確認するため、まず既存のファイルを見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。これらの指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドで指示の適用先を決める glob が指定されていることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルとプロジェクト内の既存ファイルへのリンクを確認します。これを使うと、大きな指示を再利用可能な小さなファイルに分け、コード生成時に参考となる例を Copilot に示せます。

## チームのガイダンスに合うよう指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ足りない点があります。中核となる `copilot-instructions.md` ファイルを変更し、新しく生成する TypeScript に TSDoc コメントが含まれるようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を更新した後、フィルタリングの会話を終了して再開します。これにより、機能のコンテキストを保ちながら、更新したガイダンスを Copilot に読み込ませます。

1. フィルタリングの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示された場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを送信し、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。次のことを行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認した。
- コーディング標準を守るため、指示ファイルを更新した。
- 指示ファイルがコードに及ぼす影響を確認した。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
