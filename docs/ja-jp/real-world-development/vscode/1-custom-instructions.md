---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも、コード生成のガイダンスを提供する[指示ファイル][instruction-files]が重要です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- 現在の指示を使ってフィルタリング用の最初のデータ スライス（publisher ヘルパー）を生成する。
- リポジトリ全体に適用する新しい規則を `.github/copilot-instructions.md` に追加する。
- 同じプロンプトを再実行し、生成されたコードに新しい規則が反映されることを確認する。
- 指示の変更とフィルタリングのスライスを `main` にコミットし、次の演習で Copilot が更新後のガイダンスを使えるようにする。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わない場合があります。Copilot の動作は確率的です。この演習の目的は、指示を更新した後の動作の**傾向**を確認することであり、出力のすべての文字を完全に一致させることではありません。

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

## 変更用ブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な手順ではその都度説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後でファイルを更新し、同じプロンプトを再送して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンで **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパー ファイルとテストなどにコード変更を適用します。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、publisher の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作の**一貫性**が改善することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるファイルです。コード提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内ドキュメントの規則はまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に次の Markdown 行を追加し、ファイルのコメント ヘッダーと TSDoc ドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に作成して全体に適用することも、言語、ファイル種別、タスクに応じた `*.instructions.md` として作成することもできます。今回追加したドキュメント コメントの規則など、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを認識させます。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで、新しいプロジェクト標準に従うよう Copilot を導き、次の演習で利用するコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリのほかのファイルと同様に、ソース管理で管理します。変更をコミットして、リポジトリにブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤作成で Copilot が変更したファイルすべてをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなど追加の関連ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けたプルリクエストを作成します。手動でも作成できますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せます。Issue を探して PR を作成し、Issue を完了するよう依頼します。

> [!NOTE]
> Copilot が GitHub を操作するのは、この演習が初めてです。プロジェクト テンプレートでは GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。画面の手順に従って許可してください。この接続の仕組みである Model Context Protocol については、後の演習で学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、新しい publishers ヘルパーの基盤作成を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成するとき、GitHub 操作の実行を **Allow** するよう求められたら、コマンドを確認して適切に許可します。
5. PR を作成したら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作とシェル コマンドの実行を **Allow** するよう求めたら、コマンドを確認して適切に許可します。

GitHub Copilot の支援を使い、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成プロンプトを送り、**既存**の指示を使った Copilot の出力を確認する。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加する。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認する。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤を `main` にコミットする。

次は[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
