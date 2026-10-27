# 🧠 Agent Brain — Private Backup

Ye repo mere Termux agent ke **knowledge, skills aur helper scripts** ka private backup hai.

## Kya include hai
- `knowledge/` — LEARNINGS.md (lessons) + memory.jsonl (Memory MCP data)
- `skills/` — installed agent skills (find-skills, android-reverse-engineering, apk-reverse, bytecode-viewer, learnings)
- `bin/` — `token-usage`, `context-reset`, `model-rotate`, `port-rotate`, `agent-backup`
- `config/` — free model pool + port pool (no secrets)
- `AGENTS.md` — workflow rules

## Kya include NAHI hai
- API keys, tokens, passwords
- `opencode.json` (secrets)
- Logs, caches, APK files

## Sync kaise kare
```bash
agent-backup "update: kya badla"
```

Script automatically:
1. Copy karta hai latest files
2. Secret patterns scan karta hai (leak hone par abort)
3. Commit + push karta hai

## Restore (naye device par)
```bash
git clone https://github.com/<user>/agent-brain.git
cp -a agent-brain/knowledge/. ~/.agents/knowledge/
cp -a agent-brain/skills/. ~/.agents/skills/
cp agent-brain/bin/* ~/.local/bin/ && chmod +x ~/.local/bin/*
```
