---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、Codespace の統合ターミナルを使って Copilot CLI をインストールして実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成する。
- Codespace を作成し、プロジェクトを使える状態にする。

## ラボ リポジトリをセットアップする

自分用の Tailspin Toys プロジェクトで作業します。[テンプレート リポジトリ][tailspin-template]から今すぐ作成しましょう。新しいリポジトリには、ラボに必要なすべてのファイルが含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template**、次に **Create a new repository** を選択して、リポジトリのコピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織内に新しいリポジトリを作成します。
4. 作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えてください。ワークショップの後半で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップ全体を通してこれらの Issue を使うため、自分で Issue を作成する必要はありません。

新しいワークショップ テンプレートのコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、演習で使うバックログが含まれています。古いコピーを使う場合は、必要なファイルが揃っているかファシリテーターに確認してください。

## Codespace を作成する

次に、Codespace でワークショップを進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーで直接コードを作成、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応した機能豊富なエディターが用意されています。

1. 新しく作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによって、プロジェクトの依存関係、Playwright Chromium、ローカル データベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と確認された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリ ルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択して Tailspin Toys サイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成した。
- Codespace を作成し、プロジェクトの準備ができていることを確認した。

次は、Codespace に [GitHub Copilot CLI をインストール][next-lesson]して、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
