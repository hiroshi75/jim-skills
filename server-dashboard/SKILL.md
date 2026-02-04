---
name: server-dashboard
description: Quick server health overview. Shows CPU, memory, disk, services, and recent errors in one view. Use when user asks for "サーバー状況" or "server status".
---

# Server Dashboard - Quick Health Check

## PURPOSE
Provide instant overview of server health without digging through multiple commands.

## WHEN TO USE
- TRIGGERS:
  - "サーバー状況" / "サーバーの状態"
  - "server status" / "server health"
  - "システム確認"
  - "何か問題ある？" (in server context)

## WORKFLOW
1. Gather system metrics (parallel when possible)
2. Check critical services
3. Scan recent logs for errors
4. Format into dashboard view
5. Highlight any issues

## DATA COLLECTION

### System Resources
```bash
# CPU & Load
uptime
cat /proc/loadavg

# Memory
free -h

# Disk
df -h | grep -E '^/dev/'
```

### Services (auto-detect)
```bash
# Systemd services (user)
systemctl --user list-units --state=running --no-pager | head -20

# Systemd services (system, if accessible)
systemctl list-units --state=failed --no-pager 2>/dev/null

# PM2 (if available)
pm2 list 2>/dev/null

# Docker (if available)
docker ps --format "table {{.Names}}\t{{.Status}}" 2>/dev/null
```

### Recent Errors
```bash
# Journal errors (last hour)
journalctl --user -p err -S "1 hour ago" --no-pager 2>/dev/null | tail -10

# Nginx errors (if exists)
tail -5 /var/log/nginx/error.log 2>/dev/null
```

### Network
```bash
# Listening ports
ss -ltnp 2>/dev/null | head -15
```

## OUTPUT FORMAT
```
🖥️ SERVER DASHBOARD - [hostname]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 RESOURCES
  CPU Load: [1m] [5m] [15m]  [status emoji]
  Memory:   [used]/[total] ([%])  [status emoji]
  Disk /:   [used]/[total] ([%])  [status emoji]

🔧 SERVICES
  ✅ openclaw-gateway (running, 4h)
  ✅ nginx (running)
  ⚠️ some-service (failed)

🌐 PORTS
  :18789 → openclaw-gateway
  :80    → nginx
  :443   → nginx

⚠️ RECENT ERRORS (last hour)
  [timestamp] [service] [message]
  ...

📈 STATUS: [🟢 HEALTHY | 🟡 WARNING | 🔴 CRITICAL]
```

## STATUS THRESHOLDS
- CPU Load > cores: 🟡, > 2x cores: 🔴
- Memory > 80%: 🟡, > 95%: 🔴
- Disk > 80%: 🟡, > 95%: 🔴
- Any failed service: 🟡
- Multiple errors: 🔴

## SAFETY
- Read-only commands only
- No service restarts without explicit request
- Works with linux-service-triage for deeper diagnosis
