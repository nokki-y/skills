# SKILL.mdテンプレート

以下をコピーして新しいスキルのSKILL.mdを作成してください。

```yaml
---
name: [スキル名]
description: [スキルの説明。Claudeがいつ使うか判断できるよう、ユーザーが自然に言うキーワードを含める]
argument-hint: "[引数のヒント（省略可）]"
allowed-tools: [許可するツール（省略可）]
---

# [スキル名]

[スキルの概要を1-2行で]

## 実行手順

### 1. [最初のステップ]

[手順の説明]

### 2. [次のステップ]

[手順の説明]

## 注意事項

- [注意点1]
- [注意点2]
```

## フロントマターの選択ガイド

### 呼び出し制御

| 目的 | 設定 |
|------|------|
| ユーザーもClaudeも呼び出せる（デフォルト） | 設定なし |
| ユーザーのみ呼び出せる（副作用あり） | `disable-model-invocation: true` |
| Claudeのみ呼び出せる（バックグラウンド知識） | `user-invocable: false` |

### ツール制限

```yaml
# 読み取り専用
allowed-tools: Read, Grep, Glob

# GitHub CLI使用
allowed-tools: Bash(gh:*)

# ファイル編集あり
allowed-tools: Read, Edit, Write, Grep, Glob
```

### サブエージェント実行

```yaml
# 分離コンテキストで実行
context: fork
agent: Explore  # または Plan, general-purpose
```
