---
name: ghq
description: ghq を使ったリポジトリの検索・クローン・作成・移動を支援するスキル。プロジェクト間の切り替えやリポジトリ管理が必要なときに使う。
allowed-tools: Bash
---

# ghq: リポジトリ管理リファレンス

ghq でリポジトリのクローン・検索・作成を行うためのリファレンス。

## リポジトリの検索

```bash
# 全リポジトリ一覧
ghq list

# キーワードで絞り込み
ghq list <keyword>

# フルパスで表示
ghq list -p

# フルパス + キーワード
ghq list -p <keyword>
```

## リポジトリのクローン

```bash
# GitHub リポジトリをクローン
ghq get <owner>/<repo>

# URL 指定
ghq get https://github.com/<owner>/<repo>

# shallow clone
ghq get --shallow <owner>/<repo>

# 既存リポジトリを更新
ghq get --update <owner>/<repo>
```

## リポジトリの作成

```bash
# 新規リポジトリを作成（ローカルのみ）
ghq create <owner>/<repo>
```

GitHub にもリポジトリを作成する場合は `gh repo create` と組み合わせる:

```bash
gh repo create <owner>/<repo> --public
ghq get <owner>/<repo>
```

## パス解決

```bash
# ghq root ディレクトリ
ghq root

# 特定リポジトリのパスを取得
ghq list -p <keyword>

# リポジトリのディレクトリに移動
cd "$(ghq list -p <keyword> | head -1)"
```

## プロジェクト間の移動

別のリポジトリで作業を行う場合:

```bash
# リポジトリのパスを取得して移動
cd "$(ghq list -p <repo-name> | head -1)"
```
