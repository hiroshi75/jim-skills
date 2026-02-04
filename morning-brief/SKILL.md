# Morning Brief Skill 🌅

Comprehensive morning briefing combining weather, calendar, server status, notifications, and pending tasks.

## Triggers

- 「朝ブリーフィング」「モーニングブリーフ」
- "morning brief", "morning briefing", "daily brief"
- Cron: 毎朝8:00 JST (推奨)

## What It Includes

1. **天気** - 今日・明日の天気予報
2. **カレンダー** - 今日と明日の予定
3. **サーバー状況** - CPU/メモリ/ディスク/サービス簡易チェック
4. **通知** - 未読メール、SNS mentions (Moltbook等)
5. **タスク** - quick-stash の未完了タスク
6. **昨日のハイライト** - memory/YYYY-MM-DD.md から

## Execution Flow

```
1. 天気取得 (weather skill使用)
2. カレンダーチェック (gcalcli または API)
3. サーバー簡易チェック (server-dashboard skill 軽量版)
4. 通知チェック (メール、SNS)
5. タスク読み込み (stash/tasks.md)
6. 昨日のメモリ読み込み
7. 整形して送信
```

## Output Format

```
🌅 おはよう、Hiroshi！

☀️ **天気**
今日: 晴れ 12°C / 明日: 曇り 10°C

📅 **今日の予定**
- 10:00 ミーティング
- 14:00 歯医者

🖥️ **サーバー**
✅ 正常 (CPU 15%, Mem 45%, Disk 62%)

📬 **通知**
- 未読メール: 3件
- Moltbook: 2 mentions

📝 **タスク** (3件)
- [ ] レポート提出
- [ ] 買い物リスト作成

📖 **昨日のハイライト**
- quick-stash スキル作成完了
- X.comアカウント設定

良い一日を！🤠
```

## Cron Setup Example

```json
{
  "name": "morning-brief",
  "schedule": {
    "kind": "cron",
    "expr": "0 8 * * *",
    "tz": "Asia/Tokyo"
  },
  "payload": {
    "kind": "systemEvent",
    "text": "朝ブリーフィングの時間だ。morning-brief スキルを実行して、Hiroshiに送信。"
  },
  "sessionTarget": "main",
  "enabled": true
}
```

## Customization

`TOOLS.md` に以下を追加して調整可能:

```markdown
### Morning Brief
- 天気の場所: Tokyo
- カレンダー: Google Calendar
- 通知チェック: メール, Moltbook
- 送信先: Telegram
```

## Dependencies

- weather skill (天気)
- server-dashboard skill (サーバー)
- quick-stash skill (タスク)
- gcalcli または Google Calendar API (カレンダー)

## Notes

- 休日モード: 週末は簡略版（天気+予定のみ）
- 遅起き検出: 10時以降に起動したら「おそよう」に変更
- 重要アラートは太字で強調
