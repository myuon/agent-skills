---
name: ghq
description: ghq を使ったリポジトリ管理のクイックリファレンス。リポジトリの取得・一覧・検索・作成・パス解決が必要なときに使う。
allowed-tools: Bash
---

# ghq: リポジトリ管理リファレンス

## リポジトリの取得

```bash
# GitHub リポジトリをクローン
ghq get https://github.com/owner/repo

# 短縮形
ghq get owner/repo

# shallow clone
ghq get --shallow owner/repo

# 既存リポジトリを更新
ghq get --update owner/repo
```

## リポジトリの一覧・検索

```bash
# 全リポジトリ一覧
ghq list

# フルパスで表示
ghq list --full-path

# キーワードで絞り込み
ghq list owner/repo
ghq list keyword
```

## リポジトリの作成

```bash
# 新規リポジトリを作成（ghq root 配下に配置）
ghq create repo-name

# オーナー付き
ghq create owner/repo-name
```

## パス解決

```bash
# ghq root のパスを取得
ghq root

# 特定リポジトリのパスを取得
ghq list --full-path | grep owner/repo

# リポジトリに移動
cd "$(ghq list --full-path | grep owner/repo)"
```

## プロジェクト間の切り替え

```bash
# リポジトリを選択して移動（fzf 連携）
cd "$(ghq list --full-path | fzf)"

# 特定リポジトリに直接移動
cd "$(ghq root)/github.com/owner/repo"
```
