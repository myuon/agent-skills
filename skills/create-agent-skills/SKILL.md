---
name: create-agent-skills
description: 新しい Claude Code スキルを作成するためのガイド。スキルの作り方、SKILL.md の書き方、配置場所を案内する。
allowed-tools: Bash, Read, Write, Edit, Glob, Grep
---

# 新しいスキルの作成

$ARGUMENTS の内容に基づいて新しいスキルを作成する。

## スキルの構造

スキルは `SKILL.md` をエントリポイントとするディレクトリ。

```
my-skill/
├── SKILL.md           # メイン指示（必須）
├── scripts/           # 実行可能スクリプト（任意）
├── references/        # 追加ドキュメント（任意）
└── assets/            # テンプレート、画像、データファイル（任意）
```

### Progressive disclosure

スキルはコンテキストを効率的に使うため段階的に読み込まれる:

1. **メタデータ** (~100トークン): `name` と `description` が起動時に全スキル分読み込まれる
2. **指示** (<5000トークン推奨): スキルがアクティブ化されたときに `SKILL.md` 本文が読み込まれる
3. **リソース** (必要時のみ): `scripts/`, `references/`, `assets/` 内のファイルは必要時にのみ読み込まれる

`SKILL.md` は 500 行以下に保つ。

## 配置場所

| スコープ | パス | 適用対象 |
|---------|------|---------|
| 個人用 | `~/.claude/skills/<skill-name>/SKILL.md` | すべてのプロジェクト |
| プロジェクト用 | `.claude/skills/<skill-name>/SKILL.md` | このプロジェクトのみ |
| プラグイン | `<plugin>/skills/<skill-name>/SKILL.md` | プラグインが有効な場所 |

プロジェクトスキルは同名の個人スキルをオーバーライドする。

## SKILL.md の書き方

### フロントマター

ファイル上部の `---` マーカー間に YAML で記述する。

```yaml
---
name: my-skill
description: スキルの説明。Claude がいつ使うか判断するために使われる
allowed-tools: Read, Grep, Bash
---
```

#### フロントマターフィールド一覧

**Agent Skills 標準フィールド** (https://agentskills.io/specification):

| フィールド | 必須 | 説明 |
|-----------|------|------|
| `name` | はい | 1-64文字。小文字・数字・ハイフンのみ。先頭末尾のハイフン不可、連続ハイフン不可。親ディレクトリ名と一致させる |
| `description` | はい | 1-1024文字。何をするか＋いつ使うか。キーワードを含めてエージェントが識別しやすくする |
| `license` | いいえ | ライセンス名またはバンドルされたライセンスファイルへの参照 |
| `compatibility` | いいえ | 環境要件（1-500文字）。対象プロダクト、必要パッケージ、ネットワーク要件など |
| `metadata` | いいえ | 任意の key-value マッピング。`author`, `version` など |
| `allowed-tools` | いいえ | スキル実行時に許可なしで使えるツール（スペース区切り） |

**Claude Code 拡張フィールド**:

| フィールド | 必須 | 説明 |
|-----------|------|------|
| `argument-hint` | いいえ | オートコンプリートで表示されるヒント。例: `[issue-number]` |
| `disable-model-invocation` | いいえ | `true` で Claude の自動ロードを防止。`/name` での手動トリガー専用に |
| `user-invocable` | いいえ | `false` で `/` メニューから非表示。バックグラウンド知識用 |
| `model` | いいえ | スキル実行時に使用するモデル |
| `context` | いいえ | `fork` でサブエージェントとして分離実行 |
| `agent` | いいえ | `context: fork` 時に使用するサブエージェントタイプ |
| `hooks` | いいえ | スキルのライフサイクルにスコープされたフック |

#### `name` の命名規則

```yaml
# OK
name: pdf-processing
name: code-review

# NG
name: PDF-Processing   # 大文字不可
name: -pdf             # 先頭ハイフン不可
name: pdf--processing  # 連続ハイフン不可
```

#### `description` の書き方

何をするか＋いつ使うかを具体的に書く。エージェントがタスクとの関連性を判断するキーワードを含める。

```yaml
# Good
description: Extract text and tables from PDF files, fill PDF forms, and merge multiple PDFs. Use when working with PDF documents.

# Bad
description: Helps with PDFs.
```

### 呼び出し制御の組み合わせ

| 設定 | ユーザー | Claude | 用途 |
|------|---------|--------|------|
| （デフォルト） | 呼べる | 呼べる | 一般的なスキル |
| `disable-model-invocation: true` | 呼べる | 呼べない | deploy, commit 等の副作用があるもの |
| `user-invocable: false` | 呼べない | 呼べる | バックグラウンド知識 |

## スキルの種類

### リファレンスコンテンツ

Claude が現在の作業に適用する知識。規約、パターン、スタイルガイドなど。インラインで実行される。

```yaml
---
name: api-conventions
description: API design patterns for this codebase
---

API エンドポイント作成時のルール:
- RESTful な命名規則
- 一貫したエラーフォーマット
```

### タスクコンテンツ

特定のアクション用のステップバイステップ指示。`disable-model-invocation: true` と組み合わせることが多い。

```yaml
---
name: deploy
description: Deploy the application
disable-model-invocation: true
---

1. テストスイートを実行
2. アプリケーションをビルド
3. デプロイターゲットにプッシュ
```

## 作成手順

1. ユーザーにスコープ（個人用 / プロジェクト用）を確認
2. スキルディレクトリを作成
3. SKILL.md を作成（フロントマター + コンテンツ）
4. 必要に応じてサポートファイルを追加
5. テスト: `/skill-name` で直接呼び出して動作確認

## Advanced

より高度な機能については [advanced.md](advanced.md) を参照。

- **変数**: `$ARGUMENTS`, `${CLAUDE_SESSION_ID}` による動的な値の埋め込み
- **動的コンテキスト注入**: `` !`command` `` 構文でシェルコマンド出力をスキルに埋め込む
- **サブエージェント実行**: `context: fork` による分離実行とエージェントタイプの選択
- **アクセス制御**: 特定スキルの許可・拒否ルール
- **Extended Thinking**: ultrathink による拡張思考の有効化
- **バリデーション**: skills-ref によるスキルの検証


## 出典

- [Agent Skills Specification](https://agentskills.io/specification) - Agent Skills オープンスタンダードの仕様
- [Claude Code Skills](https://code.claude.com/docs/ja/skills) - Claude Code でのスキルの作成・管理・共有
