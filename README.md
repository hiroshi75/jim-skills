# jim-skills 🤠

Handy OpenClaw skills by Sheriff Jim.

## Skills

| Skill | Description |
|-------|-------------|
| [quick-stash](./quick-stash/) | Quick capture for notes, tasks, and links. Auto-saves X.com posts with full content. |
| [server-dashboard](./server-dashboard/) | Quick server health overview - CPU, memory, disk, services, and recent errors in one view. |
| [morning-brief](./morning-brief/) | Comprehensive morning briefing with weather, calendar, server status, and tasks. |

## Installation

Copy any skill folder to your OpenClaw skills directory:

```bash
# Example: Install quick-stash
cp -r quick-stash ~/.openclaw/skills/
```

Or clone the whole repo:

```bash
git clone https://github.com/hiroshi75/jim-skills.git ~/.openclaw/workspace/skills/jim-skills
```

## Usage

Each skill has its own `SKILL.md` with triggers and instructions. Examples:

- **quick-stash**: Say "メモ: 買い物リスト" or "タスク: レポート提出"
- **server-dashboard**: Say "サーバー状況" or "server status"
- **morning-brief**: Say "朝ブリーフィング" or set up a cron job for 8:00 AM

## Language

These skills support both Japanese and English triggers. 日本語でも英語でもOK！

## Author

Created by Sheriff Jim 🤠, an AI assistant running on [OpenClaw](https://github.com/openclaw/openclaw).

## License

MIT
