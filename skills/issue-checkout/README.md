# issue-checkout

GitHub Issueをもとに作業ブランチを作成し、そのブランチへ切り替えるSkillです。

## 使い方

Codexでは、Skill名とIssueのURLを指定します。

```text
$issue-checkout https://github.com/owner/repository/issues/123
```

Claude Codeでは、スラッシュコマンドとして呼び出します。

```text
/issue-checkout https://github.com/owner/repository/issues/123
```

URLを省略した場合は、実行前に入力を求めます。

## 実行内容

1. Gitリポジトリであることと、未コミットの変更がないことを確認する
2. `gh issue view`でIssueのタイトル、ラベル、正式なURLを取得する
3. Issueと現在のリポジトリが一致することを確認する
4. リポジトリ既存の命名規則、または`<種類>/<英語のkebab-case>`形式でブランチ名を生成する
5. ブランチ名がGitで使用可能か検証する
6. 同名のローカルブランチがあれば切り替え、なければ現在の`HEAD`から新規作成する
7. 切り替えたブランチ名とIssueのURLを報告する

リポジトリに明確な命名規則がない場合、ブランチの種類にはIssueのタイトルとラベルに応じて`feature`、`fix`、`refactor`、`chore`、`docs`などを使用します。専用の設定ファイルは使用しません。

## 前提条件

- Gitリポジトリ内で実行すること
- GitHub CLI（`gh`）がインストールされ、対象Issueを参照できる状態であること

このSkillは、ベースブランチの更新、Issueの更新、変更のコミット、ブランチのpush、Pull Requestの作成までは行いません。これらは明示的に依頼された場合のみ実行します。
