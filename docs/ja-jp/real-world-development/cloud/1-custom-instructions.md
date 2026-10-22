---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- リポジトリ全体に適用する新しい規則を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの「変更前と変更後」を比較しません。Copilot cloud agent は GitHub Issue を非同期に処理するため、リアルタイムで影響を比較しにくいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの効果を確認します。

## 指示ファイル

### シナリオ

優れた開発チームとして、Tailspin Toys には開発上の慣行に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニット テストを用意する。
- UI はダーク モードにし、モダンな見た目にする。
- TSDoc のドキュメント コメントをコードに追加する。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加する。

指示ファイルを使って、Copilot がこれらの慣行に沿ってタスクを実行するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝えて、コーディング スタイルや要件を理解しやすくできます。推奨するコーディング規約やライブラリ、コードに含めるコメントの形式などを指定できます。リポジトリ全体、または特定のファイル種別のタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。Copilot に送るほとんどのチャットや CLI リクエストに関係する、プロジェクト レベルの情報を記載します。技術スタック、開発中の内容の概要、ベスト プラクティス、全体に適用するガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に使う指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、新しいユニット テストの作成などに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの規格に基づく指示ファイルにも対応しています。[サポートされる指示ファイル][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成に関するすべてを扱うことは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が示されています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発する内容、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに必要な指示を記載します。
- 自然な言葉を使い、明確なガイダンスとコードの例を示します。

AI の使い方に決まった方法がないのと同じように、指示ファイルの作り方にも唯一の正解はありません。試行錯誤しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを見ると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクのファイルがあることに気付くでしょう。
>
> Copilot に指示ファイルを作成してもらうこともできます。画面ごとに方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。関連する場合は、その画面を使うレッスンで説明します。
>
> テンプレートや出発点をお探しですか。[awesome-copilot][awesome-copilot]では、多数の指示ファイル、カスタム エージェント、その他のリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭にある `applyTo` フィールドは、指示を適用するファイルを決める glob（リポジトリ ルートからの相対パス）です。たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分けたり、コード生成の際に参考となる例を Copilot に示したりできます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc ドキュメント コメントとファイルのコメント ヘッダーに関する規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つだけです。ドキュメントを TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーとしてコード内に記載する規則を、プロジェクト全体に追加します。このハーネスの後半で課題を割り当てると、cloud agent はこの規則を参照します。

編集を始める前に、作業用のブランチを作成します（cloud agent は課題の対象ブランチから指示を読み取りますが、ここでは変更を整理してコミットします）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど説明したように、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるためのファイルです。リポジトリのコーディング標準を文書化し、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの規則はまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に次の Markdown 行を追加し、ファイルのコメント ヘッダーと TSDoc ドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に作成して全体に適用することも、言語、ファイル種別、タスクに応じた `*.instructions.md` として作成することもできます。今回追加したドキュメント コメントの規則など、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、課題の対象ブランチから指示ファイルを読み取ります。次の演習で Copilot に課題を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を参照できるよう、変更を `main` に反映する必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で課題を割り当てると、cloud agent はこの指示を参照します。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せず `main` にコミットしてもかまいません。ここでは、実際のプロジェクトに近い流れを体験するためにブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、その標準が反映されることを確認します。

次に、[最初の課題を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
