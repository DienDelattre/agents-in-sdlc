---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組んできました。コードへのドキュメント追加、デザイン チームが反復的に改善できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、結果を改善しましょう。

### シナリオ

何度も説明してきたように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進めます。チーム メンバーにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ監視なしでタスクを実行するため、安全を確保するためのセキュリティ制約があります。

- Copilot がリポジトリに対して持つのは読み取りアクセスで、書き込みアクセスはコード作成に使用するブランチ**のみ**です。
- cloud agent は GitHub Actions 内で実行され、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードにドキュメントを追加する変更を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスが既定で制限されているため、**想定される動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、ドキュメントの追加を示すタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、ここまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。新しい TSDoc ドキュメント コメントなどが追加されています。変更の正確な内容は状況によって異なります。

    変更を確認する際は、TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーを探します。これらは演習 2 の開始時に確認したカスタム指示ファイルに基づいています。

6. 更新を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行に承認が必要であることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションで、ワークフローがキューに入ったことを確認します。正常に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は一方通行ではありません。チーム メンバーと同様に、プルリクエストのコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、依頼する文面を具体的に定めることはできません。たとえば、次のような更新を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をチーム メンバーと同じようにメンションし、生成されたドキュメントの変更を依頼するコメントを追加します。上記の例や、コードベースのドキュメントに関する別のアイデアを使ってください。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復的に改善してください。問題がなければ、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームを示すタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの実行に承認が必要であることを示すインジケーターを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションで、ワークフローがキューに入ったことを確認します。正常に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace を開いてターミナルを起動し、Copilot が作成したブランチ名（例: **copilot/fix-8**）に `<branch-name>` を置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。前と同じように cloud agent にコメントを追加して、変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc ドキュメント コメントを追加したりできます。この Issue はカスタム指示を更新する前に割り当てたことに注意してください。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能をレビューします。この PR には、演習 3 で割り当てたハイ コントラスト モードと、演習 4 でミッション コントロールから追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、ハイ コントラスト モードを示すタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は数分待ち、作業が完了するまで待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保持するためのローカル ストレージの使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - 表示モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行に承認が必要であることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションで、ワークフローがキューに入ったことを確認します。正常に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
10. **オプション:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace を開いてターミナルを起動し、Copilot が作成したブランチ名に `<branch-name>` を置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導いたことを確認してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## オプションの演習 — 委任を続ける

cloud agent には、実際のバックログ項目を任せて、その間に別の作業を進める方法が適しています。この使い方に慣れるため、リポジトリに Issue をいくつか追加して Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力の検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に反映されることを確認しました。**
- **Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページをミッション コントロールとして使用し、作業中のアクセシビリティ セッションを監視して指示しました。**
- **Copilot のプルリクエストをレビューして反復的に改善し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学習する場合は、ほかのハーネスで関連する内容を体験できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻るオプションの `/delegate` を使い、ターミナルから 2 つの変更をレビューして反映します。

自分のリポジトリでは、次の作業も試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
