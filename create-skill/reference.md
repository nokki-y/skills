# スキル作成リファレンス

## フロントマター全オプション

| フィールド | 必須 | 説明 |
|-----------|------|------|
| `name` | いいえ | スキル名。省略時はディレクトリ名。小文字・数字・ハイフンのみ（最大64文字） |
| `description` | 推奨 | スキルの説明。Claudeが自動呼び出しの判断に使用 |
| `argument-hint` | いいえ | 引数のヒント。例: `[PR番号]`, `[ファイル名] [形式]` |
| `disable-model-invocation` | いいえ | `true`でClaude自動呼び出しを無効化 |
| `user-invocable` | いいえ | `false`で`/`メニューから非表示 |
| `allowed-tools` | いいえ | スキル実行時に許可するツール |
| `model` | いいえ | 使用するモデル指定 |
| `context` | いいえ | `fork`でサブエージェント実行 |
| `agent` | いいえ | サブエージェントタイプ（`Explore`, `Plan`, `general-purpose`） |
| `hooks` | いいえ | スキルスコープのフック設定 |

## 変数置換

| 変数 | 説明 |
|------|------|
| `$ARGUMENTS` | スキル呼び出し時の引数 |
| `${CLAUDE_SESSION_ID}` | 現在のセッションID |

## 動的コンテキスト注入

シェルコマンドの出力をスキルに注入：

```markdown
現在のブランチ: !`git branch --show-current`
最新コミット: !`git log -1 --oneline`
```

## ディレクトリ構造パターン

### シンプルなスキル

```
my-skill/
└── SKILL.md
```

### 中規模スキル

```
my-skill/
├── SKILL.md
└── reference.md
```

### 複雑なスキル

```
my-skill/
├── SKILL.md
├── reference.md
└── templates/
    ├── output-format.md
    └── report.md
```

### スクリプト付きスキル

```
my-skill/
├── SKILL.md
└── scripts/
    └── helper.py
```

## スキル配置場所

| 場所 | パス | 対象 |
|------|------|------|
| 個人 | `~/.claude/skills/[name]/SKILL.md` | 全プロジェクト |
| プロジェクト | `.claude/skills/[name]/SKILL.md` | 該当プロジェクトのみ |
| プラグイン | `[plugin]/skills/[name]/SKILL.md` | プラグイン有効時 |

## ベストプラクティス

### description の書き方

**良い例：**
```yaml
description: PRレビューの指摘を確認・一覧化し、改修が必要なものを修正する。PRレビュー対応、レビュー指摘の確認、レビューコメントの対応時に使用。
```

- 何をするか明確
- ユーザーが使う言葉を含む（「PRレビュー」「指摘」「改修」）

**悪い例：**
```yaml
description: PRを処理する
```

- 曖昧で判断材料が少ない

### allowed-tools の設定

**最小権限の原則：**
```yaml
# 読み取り専用スキル
allowed-tools: Read, Grep, Glob

# 編集が必要なスキル
allowed-tools: Read, Edit, Write, Grep, Glob

# 特定コマンドのみ
allowed-tools: Bash(gh:*), Bash(git:*)
```

### サブエージェント実行の判断

**`context: fork` を使う場合：**
- 独立したタスクを実行する
- 会話コンテキストが不要
- 長時間実行される可能性がある

**インライン実行（デフォルト）の場合：**
- 会話コンテキストを参照する必要がある
- ガイドラインや規約の適用
- 対話的な作業
