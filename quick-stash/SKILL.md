---
name: quick-stash
description: Quick capture for notes, tasks, and links. Saves X.com posts with full content. Use when user says "メモ:", "タスク:", "保存:", or shares a URL to save.
---

# Quick Stash - Notes, Tasks & Links

## PURPOSE
Instantly capture notes, tasks, and links with minimal friction. Automatically fetches and summarizes web content, especially X.com posts.

## WHEN TO USE
- TRIGGERS:
  - "メモ: ..." or "Note: ..." → save a note
  - "タスク: ..." or "Task: ..." → save a TODO item
  - "保存: URL" or "Save: URL" → save a link with content
  - User shares a URL and asks to remember/save it
  - **x.com or twitter.com URL appears in message → AUTO-SAVE (no prompt needed)**
  - "今日のメモ" / "Show notes" → list today's captures
  - "タスク一覧" / "Show tasks" → list pending tasks
  - "最近のリンク" / "Recent links" → list saved links
  - "検索: キーワード" → search all captures

## AUTO-EXPAND MODE
- **TRIGGER**: Note ends with "..." or "、、、"
- **BEHAVIOR**: 
  1. Save the original note as-is
  2. AI expands/completes the thought based on context
  3. Add structured analysis, alternatives, conclusions
  4. Format with headers: 元メモ / 補完
- **USE CASES**:
  - Incomplete thoughts → full analysis
  - Quick ideas → detailed breakdown
  - Observations → actionable insights

## AUTO-SAVE RULES
- Any message containing x.com/*/status/* or twitter.com/*/status/* URLs
- Automatically fetch content via fxtwitter API
- Save without asking
- Reply with brief confirmation + summary

## INPUTS
- Note/Task: free text
- Link: URL (supports x.com, twitter.com, and general websites)
- Optional: tags (e.g., #work #idea #読みたい)

## WORKFLOW

### Saving Notes/Tasks
1. Parse the input for content and tags
2. Generate timestamp
3. **Check for "..." or "、、、" at end → trigger auto-expand**
4. If expanding: add structured analysis with 元メモ/補完 format
5. Append to `memory/stash/YYYY-MM-DD.md`
6. Confirm save with preview (include expansion summary if expanded)

### Saving Links
1. Detect URL type:
   - X.com/Twitter: use fxtwitter API (`https://api.fxtwitter.com/status/ID`)
   - Other sites: use web_fetch
2. Extract: title, author, main content/summary
3. Save to `memory/stash/links.jsonl` with metadata
4. Also append human-readable entry to daily file
5. Confirm with content preview

### Retrieving
1. Read from appropriate files
2. Filter by date, type, or search query
3. Format nicely for display

## OUTPUT FORMAT

### Save Confirmation
```
✅ Saved [type]: [preview]
Tags: #tag1 #tag2
File: memory/stash/2026-02-04.md
```

### Link Save Confirmation
```
🔗 Saved link from @author
Title: [title]
Summary: [2-3 sentence summary]
Tags: #tag1
```

### List Format
```
📝 Today's Captures (2026-02-04)

Notes:
- 14:30 | This is a note #idea

Tasks:
- [ ] 15:00 | Do something #work
- [x] 10:00 | Completed task

Links:
- 16:00 | @user: Tweet summary... [x.com/...]
```

## FILE STRUCTURE
```
memory/stash/
├── YYYY-MM-DD.md    (daily notes/tasks)
├── links.jsonl      (structured link data)
└── index.md         (search index, auto-updated)
```

## SAFETY
- Never delete existing entries
- Append-only for captures
- Mark tasks complete, don't remove them
