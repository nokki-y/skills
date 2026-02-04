---
name: create-skill
description: 新しいClaude Codeスキルを作成する。スキル作成、カスタムコマンド作成、スラッシュコマンド追加時に使用。
argument-hint: "[スキル名] [スキルの説明]"
allowed-tools: Bash(mkdir:*), Read, Edit, Write, Glob
---

# スキル作成スキル

新しいClaude Codeスキルを作成するワークフロー。

## 実行手順

### 1. 要件の確認

ユーザーに以下を確認：
- **スキル名**: 小文字、数字、ハイフンのみ（最大64文字）
- **目的**: スキルが何をするか
- **スコープ**: 個人用（`~/.claude/skills/`）かプロジェクト用（`.claude/skills/`）か
- **呼び出し方法**: ユーザーのみ / Claudeも自動 / Claudeのみ

### 2. ディレクトリ作成

```bash
# 個人スキル（デフォルト）
mkdir -p ~/.claude/skills/[スキル名]

# プロジェクトスキル
mkdir -p .claude/skills/[スキル名]
```

### 3. SKILL.md作成

[templates/skill-template.md](templates/skill-template.md) を基にSKILL.mdを作成。

フロントマターオプションの詳細は [reference.md](reference.md) を参照。

### 4. サポートファイルの検討

スキルが複雑な場合、以下を検討：
- `templates/` - 出力テンプレート
- `reference.md` - 詳細リファレンス
- `scripts/` - 実行スクリプト

**分割の判断基準：**
- SKILL.mdが100行を超える場合
- テンプレートや定型出力がある場合
- コマンドリファレンスが多い場合

### 5. 動作確認

```bash
# スキルが認識されているか確認
# Claude Codeで「What skills are available?」と聞く

# 直接呼び出しテスト
# /[スキル名] でテスト実行
```

## 注意事項

- SKILL.mdは500行以下に保つ
- descriptionにはユーザーが自然に言うキーワードを含める
- 副作用のあるスキルは `disable-model-invocation: true` を設定
- 最小限の `allowed-tools` で権限を制限

## 追加リソース

- テンプレート: [templates/skill-template.md](templates/skill-template.md)
- フロントマターリファレンス: [reference.md](reference.md)
