# Advanced: スキルの高度な機能

## 利用可能な変数

スキルコンテンツ内で使える動的な値。

| 変数 | 説明 |
|------|------|
| `$ARGUMENTS` | スキル呼び出し時の引数。コンテンツに `$ARGUMENTS` がなければ末尾に `ARGUMENTS: <value>` が追加される |
| `${CLAUDE_SESSION_ID}` | 現在のセッション ID。ログやセッション固有のファイル作成に使う |

```yaml
---
name: session-logger
description: Log activity for this session
---

Log the following to logs/${CLAUDE_SESSION_ID}.log:

$ARGUMENTS
```

## 動的コンテキスト注入

`` !`command` `` 構文でスキル読み込み前にシェルコマンドを実行し、出力をプレースホルダーに埋め込める。Claude はコマンド自体ではなく実行結果のみを受け取る。

```yaml
---
name: pr-summary
description: Summarize changes in a pull request
context: fork
agent: Explore
allowed-tools: Bash(gh:*)
---

## Pull request context
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
- Changed files: !`gh pr diff --name-only`

## Your task
Summarize this pull request...
```

## サブエージェント実行

`context: fork` でスキルを分離されたサブエージェントとして実行する。会話履歴にはアクセスできない。

```yaml
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

$ARGUMENTS を調査し、関連ファイルと分析結果をまとめる。
```

`agent` フィールドで使用するサブエージェントを指定:
- 組み込み: `Explore`, `Plan`, `general-purpose`
- カスタム: `.claude/agents/` に定義したサブエージェント
- 省略時は `general-purpose`

### スキルとサブエージェントの関係

| アプローチ | システムプロンプト | タスク | 追加で読み込むもの |
|-----------|-----------------|--------|------------------|
| `context: fork` を使用したスキル | エージェントタイプから | SKILL.md コンテンツ | CLAUDE.md |
| `skills` フィールドを使用したサブエージェント | サブエージェントの本体 | Claude の委任メッセージ | プリロードされたスキル + CLAUDE.md |

## スキルのアクセス制御

Claude が呼び出せるスキルを制限する方法:

### 全スキルを無効化

`/permissions` の deny ルールに追加:
```
Skill
```

### 特定スキルの許可・拒否

```
# 特定スキルのみ許可
Skill(commit)
Skill(review-pr:*)

# 特定スキルを拒否
Skill(deploy:*)
```

`Skill(name)` で完全一致、`Skill(name:*)` で任意引数のプレフィックス一致。

## Extended Thinking の有効化

スキルコンテンツのどこかに「ultrathink」という単語を含めると、Extended Thinking が有効になる。

## サポートファイルの構成

`SKILL.md` は 500 行以下に保ち、詳細なリファレンスは別ファイルに分離する。

```
my-skill/
├── SKILL.md           # メイン指示（必須）
├── scripts/           # 実行可能スクリプト
├── references/        # 追加ドキュメント
└── assets/            # テンプレート、画像、データファイル
```

SKILL.md から相対パスで参照:
```markdown
- API 仕様: [reference.md](reference.md)
- 使用例: [examples.md](examples.md)
```

## バリデーション

[skills-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref) でスキルを検証できる:

```bash
skills-ref validate ./my-skill
```
