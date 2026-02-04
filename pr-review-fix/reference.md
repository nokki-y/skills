# リファレンス

## 指摘の種別分類

| 種別 | 説明 | 対応 |
|------|------|------|
| **改修必須** | 明確にコード修正を求められている | コード修正が必要 |
| **質問** | 意図や理由を問われている | PRコメントで回答 |
| **提案** | より良い方法の提案 | 対応任意、見送り可 |
| **LGTM/承認** | 問題なし | 対応不要 |

### 種別の判断基準

**改修必須と判断するケース：**
- 「〜してください」「〜すべき」「〜が必要」などの表現
- バグや脆弱性の指摘
- コーディング規約違反の指摘
- 明確なロジックエラーの指摘

**質問と判断するケース：**
- 「なぜ〜？」「〜の理由は？」などの疑問形
- 「〜の意図は？」という確認
- 実装の背景を問う内容

**提案と判断するケース：**
- 「〜した方が良いかも」「〜はどうでしょう」
- 「nit:」「minor:」などのプレフィックス付き
- リファクタリングの提案
- パフォーマンス改善の提案（必須でないもの）

## GitHub CLIコマンドリファレンス

### PR情報取得

```bash
# 現在ブランチのPR基本情報
gh pr view --json number,title,url,state,headRefName

# 指定PRの基本情報
gh pr view [PR番号] --json number,title,url,state,headRefName
```

### レビューコメント取得

```bash
# レビュー一覧（本文付き）
gh pr view --json reviews --jq '.reviews[] | {author: .author.login, state: .state, body: .body}'

# レビュー決定状態
gh pr view --json reviewDecision

# PRコメント（ディスカッション）
gh pr view --json comments --jq '.comments[] | {author: .author.login, body: .body}'
```

### ファイル単位のレビューコメント

```bash
# APIで取得（ファイル・行番号付き）
gh api repos/{owner}/{repo}/pulls/{pr_number}/comments --jq '.[] | {path: .path, line: .line, body: .body, author: .user.login}'
```

### Diff取得

```bash
# PR全体のdiff
gh pr diff

# ファイル名のみ
gh pr diff --name-only
```

### レビューへの返信

```bash
# PRにコメント追加
gh pr comment --body "回答内容"

# 指定PRにコメント
gh pr comment [PR番号] --body "回答内容"
```
