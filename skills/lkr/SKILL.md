---
name: lkr
description: LLM Key Ring (lkr) - macOS Keychainを使ったAPIキー管理ツールのリファレンス。APIキーの登録・取得・一覧・環境変数注入が必要なときに使う。
user-invocable: false
---

# LLM Key Ring (lkr)

```bash
lkr set MY_API_KEY           # キーを登録（Keychainに保存）
lkr get MY_API_KEY           # キーを取得
lkr list                     # 登録済みキー一覧
lkr run -- some-command      # 環境変数として注入して実行
```
