---
name: gh
description: GitHub CLI (gh) で Issue・Sub-issue・PR・CI を操作するためのクイックリファレンス
allowed-tools: Bash
---

# gh: GitHub CLI リファレンス

GitHub Issues・PR・CI を CLI から操作するためのクイックリファレンス。

## Issue の作成

```bash
gh issue create --title "タイトル" --body "本文"

# ラベル付き
gh issue create --title "タイトル" --body "本文" --label "bug"

# 複数行の本文（HEREDOC）
gh issue create --title "タイトル" --body "$(cat <<'EOF'
## 概要
説明

## 受け入れ条件
- [ ] 条件1
- [ ] 条件2
EOF
)"
```

## Issue の確認

```bash
# Issue の詳細を表示
gh issue view <number>

# Web ブラウザで開く
gh issue view <number> --web
```

## Issue の一覧・検索

```bash
# オープンな Issue 一覧
gh issue list

# 状態を指定
gh issue list --state closed
gh issue list --state all

# ラベルで絞り込み
gh issue list --label "bug"

# 自分にアサインされた Issue
gh issue list --assignee @me

# キーワード検索
gh issue list --search "キーワード"

# 複合検索
gh issue list --search "label:bug sort:updated-desc"
```

## Sub-issue の作成

[gh-sub-issue](https://github.com/yahsan2/gh-sub-issue) 拡張を使用する。

```bash
# インタラクティブモード
gh sub-issue create <parent-number>

# タイトル指定
gh sub-issue create <parent-number> --title "サブタスクのタイトル"

# タイトル + 本文
gh sub-issue create <parent-number> --title "タイトル" --body "詳細"

# URL で親 Issue を指定
gh sub-issue create https://github.com/owner/repo/issues/10 --title "タイトル"

# 別リポジトリ
gh sub-issue create <parent-number> --repo owner/other-repo

# 一覧
gh sub-issue list <parent-number>
```

### gh-sub-issue のインストール

未インストールの場合:

```bash
gh extension install yahsan2/gh-sub-issue
```

## PR の作成

```bash
gh pr create --title "タイトル" --body "$(cat <<'EOF'
## Summary
- 変更内容

Closes #<issue-number>
EOF
)"

# draft PR
gh pr create --title "タイトル" --body "..." --draft
```

`Closes #<number>` を本文に含めるとマージ時に Issue が自動クローズされる。

## CI の確認

```bash
# CI のステータスを確認
gh pr checks <pr-number>
```

### pass するまで watch

```bash
gh pr checks <pr-number> --watch --fail-fast
```

### 失敗時のログ確認

```bash
gh run list
gh run view <run-id> --log
```

## PR へのコメント

```bash
gh pr comment <pr-number> --body "コメント内容"
```

## PR のマージ

```bash
# merge commit（推奨）
gh pr merge <pr-number> --merge --delete-branch

# squash merge
gh pr merge <pr-number> --squash --delete-branch
```
