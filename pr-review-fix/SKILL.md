---
name: pr-review-fix
description: PRレビューの指摘を確認・一覧化し、改修が必要なものを修正する。PRレビュー対応、レビュー指摘の確認、レビューコメントの対応時に使用。
argument-hint: "[PR番号（省略時は現在のブランチのPR）]"
allowed-tools: Bash(gh:*), Read, Edit, Write, Grep, Glob
---

# PRレビュー指摘確認・改修スキル

PRレビューの指摘を確認して一覧化し、改修が必要なものから順次対応する。

## 実行手順

### 1. 対象PRの特定

```bash
# 現在のブランチに紐付くPR（基本）
gh pr view --json number,title,url,state,headRefName

# PR番号指定時
gh pr view $ARGUMENTS --json number,title,url,state,headRefName
```

### 2. レビューコメントの取得

```bash
# レビュー一覧
gh pr view $ARGUMENTS --json reviews,comments,reviewDecision

# ファイル単位のコメント（行番号付き）
gh api repos/{owner}/{repo}/pulls/{pr_number}/comments

# diff確認
gh pr diff $ARGUMENTS
```

※ `$ARGUMENTS`が空の場合、現在ブランチのPRを自動対象

### 3. 指摘の一覧化

[templates/issue-list.md](templates/issue-list.md) の形式で一覧化。

種別分類の詳細は [reference.md](reference.md) を参照。

### 4. 改修の実行

「改修必須」から順に対応：
1. 該当ファイルを読み込む
2. 指摘内容を理解し修正
3. 変更内容を報告

### 5. 対応完了報告

[templates/completion-report.md](templates/completion-report.md) の形式で報告。

## 注意事項

- 改修前に指摘の意図を正確に理解すること
- 不明な点はユーザーに確認を求めること
- 改修は最小限に留め、関係ない箇所は変更しないこと
- PRが存在しない場合はエラーを表示すること

## 追加リソース

- コマンドリファレンス: [reference.md](reference.md)
- 一覧テンプレート: [templates/issue-list.md](templates/issue-list.md)
- 完了報告テンプレート: [templates/completion-report.md](templates/completion-report.md)
