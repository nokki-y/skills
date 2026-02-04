---
name: create-skill
description: 新しいClaude Codeスキルを作成する。スキル作成、カスタムコマンド作成、スラッシュコマンド追加時に使用。
argument-hint: "[スキル化したいこと]"
allowed-tools: Bash(mkdir:*), Read, Edit, Write, Glob, WebFetch
---

# スキル作成スキル

新しいClaude Codeスキルを作成するワークフロー。

## 実行手順

### 1. 最新ドキュメントの確認（必須）

**スキル作成前に必ず最新のスキルドキュメントを取得してキャッチアップする。**

```
WebFetch: https://code.claude.com/docs/ja/skills
```

取得した内容から以下を確認：
- フロントマターの新しいオプション
- 推奨されるベストプラクティス
- 新機能や変更点

**このステップを省略してはならない。**

### 2. ユーザーの要望を理解

ユーザーから「スキル化したいこと」を聞き取る：

```
$ARGUMENTS
```

**ユーザーに入力させるのは「やりたいこと」のみ。**
名前や説明などの詳細はAIが考える。

### 3. スキル設計の提案

ユーザーの要望から以下を設計し、**提案して確認を得る**：

- **スキル名**: 小文字、数字、ハイフンのみ（最大64文字）
- **description**: ユーザーが自然に言うキーワードを含める
- **スコープ**: 個人用（`~/.claude/skills/`）かプロジェクト用（`.claude/skills/`）か
- **呼び出し方法**:
  - デフォルト（ユーザーもClaudeも呼び出せる）
  - `disable-model-invocation: true`（ユーザーのみ、副作用あり）
  - `user-invocable: false`（Claudeのみ、バックグラウンド知識）
- **必要なツール**: `allowed-tools` の設定
- **ファイル構成**: SKILL.mdのみか、サポートファイルが必要か

### 4. ディレクトリ作成

ユーザーの確認後、ディレクトリを作成：

```bash
# 個人スキル（デフォルト）
mkdir -p ~/.claude/skills/[スキル名]

# プロジェクトスキル
mkdir -p .claude/skills/[スキル名]
```

### 5. SKILL.md作成

最新ドキュメントの内容と [templates/skill-template.md](templates/skill-template.md) を基にSKILL.mdを作成。

### 6. サポートファイルの作成（必要な場合）

スキルが複雑な場合：
- `templates/` - 出力テンプレート
- `reference.md` - 詳細リファレンス
- `scripts/` - 実行スクリプト

**分割の判断基準：**
- SKILL.mdが100行を超える場合
- テンプレートや定型出力がある場合
- コマンドリファレンスが多い場合

### 7. 動作確認

```bash
# スキルが認識されているか確認
# Claude Codeで「What skills are available?」と聞く

# 直接呼び出しテスト
# /[スキル名] でテスト実行
```

## 注意事項

- **最新ドキュメントの確認を最優先する**
- **ユーザーには「やりたいこと」だけを聞く**
- スキル名・説明・構成はAIが提案する
- 提案後、ユーザーの確認を得てから作成する
- SKILL.mdは500行以下に保つ
- 最小限の `allowed-tools` で権限を制限

## 追加リソース

- **公式ドキュメント（最新）**: https://code.claude.com/docs/ja/skills
- テンプレート: [templates/skill-template.md](templates/skill-template.md)
- フロントマターリファレンス: [reference.md](reference.md)
