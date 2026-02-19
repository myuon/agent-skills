# agent-skills

Claude Code skills collection.

## Skills

| Skill | Description |
|-------|-------------|
| `commit` | Conventional Commits に従って git commit を実行 |
| `create-agent-skills` | 新しい Claude Code スキルを作成するためのガイド |
| `gh` | GitHub CLI (gh) で Issue・Sub-issue・PR・CI を操作するためのクイックリファレンス |

## Usecases

### 長期的なプロジェクトや開発

Conventional Commits によるコミット管理と GitHub CLI 操作を導入して、開発ワークフローを整える。

```bash
npx skills add myuon/agent-skills --skill commit --skill gh -g -y
```

### React Native + Expo を使ったアプリ開発

Expo 公式スキルを追加して、Expo/React Native 固有のベストプラクティスを適用する。

```bash
npx skills add expo/skills -g -y
```

## Installation

```bash
# List skills in this repository
npx skills add myuon/agent-skills --list

# Install specific skills (add -g for global installation)
npx skills add myuon/agent-skills --skill commit --skill gh -y

# Install to specific agents
npx skills add myuon/agent-skills -a claude-code -a opencode -y

# Install all skills from this repo to all agents
npx skills add myuon/agent-skills --all -y

# Install all skills to specific agents
npx skills add myuon/agent-skills --skill '*' -a claude-code -y

# Install specific skills to all agents
npx skills add myuon/agent-skills --agent '*' --skill commit -y
```
