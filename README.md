# agent-skills

Claude Code skills collection.

## Skills

使いたいスキルの Install コマンドをそのまま実行すればインストールできる。インストールには [apm](https://github.com/microsoft/apm) を使う。`-g` を付けるとグローバル（ユーザースコープ）に、付けないとカレントプロジェクトにインストールされる。

| Skill | Description | Install |
|-------|-------------|---------|
| `babysit` | コメントのトリアージ・コンフリクト解消・CI 修正を繰り返して PR をマージ可能な状態に保つ | `apm install myuon/agent-skills --skill babysit -g` |
| `commit` | Conventional Commits に従って git commit を実行 | `apm install myuon/agent-skills --skill commit -g` |
| `create-agent-skills` | 新しい Claude Code スキルを作成するためのガイド | `apm install myuon/agent-skills --skill create-agent-skills -g` |
| `dispatch` | 複数のイシューを同時に振り分け・実装するワークフロー | `apm install myuon/agent-skills --skill dispatch -g` |
| `gh` | GitHub CLI (gh) で Issue・Sub-issue・PR・CI を操作するためのクイックリファレンス | `apm install myuon/agent-skills --skill gh -g` |
| `lkr` | macOS Keychain を使った API キー管理ツールのリファレンス | `apm install myuon/agent-skills --skill lkr -g` |
| `react-no-useeffect` | React の useEffect を避け、より適切なパターンに置き換えるためのガイド | `apm install myuon/agent-skills --skill react-no-useeffect` |
| `react-router-loader` | React Router v7 の loader パターンのリファレンス | `apm install myuon/agent-skills --skill react-router-loader` |
| `expo/skills` | Expo / React Native 固有のベストプラクティス（[Expo 公式スキル](https://github.com/expo/skills)） | `apm install expo/skills -g` |
| `architecture-decision-records` | 設計上の意思決定を構造化された ADR ドキュメントとして残す（[wshobson/agents](https://github.com/wshobson/agents)） | `apm install wshobson/agents --skill architecture-decision-records -g` |

## Tips

```bash
# 複数のスキルをまとめてインストール
apm install myuon/agent-skills --skill commit --skill gh -g

# このリポジトリのすべてのスキルをインストール
apm install myuon/agent-skills --skill '*' -g

# 特定のエージェント向けにインストール
apm install myuon/agent-skills --skill commit --target claude
```
