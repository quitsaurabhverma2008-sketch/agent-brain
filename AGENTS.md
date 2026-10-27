# AGENTS.md — Permanent Project Workflow Rules

> Ye rules HAR project par, HAR session me apply hote hain — chahe chat/session badal jaye.
> Source of truth: Memory entity `⭐ MASTER PROJECT RULES (ALL PROJECTS)`.

## 1. FREE & OPEN SOURCE ONLY
- Sirf FREE aur OPEN-SOURCE tools, libraries, services use karo.
- Paid cheez NAHI. Free tiers allowed: Vercel (free hosting), Supabase (free tier), Cloudflare Pages, Netlify, GitHub Pages, etc.
- Har project ke START me hi **free me kaise banega + free me kahan host hoga** — ye plan pehle banao.

## 2. WORKFLOW ORDER (Compulsory — har naye project se pehle)
1. **Learnings check** — `~/.agents/knowledge/LEARNINGS.md` me relevant tags grep karo aur purani lessons apply karo.
2. **BRAIN.md padho** — agar project purana hai (tokens bachane ke liye).
3. **Skills pehle** — project-related skills dhoondo aur install karo:
   `npx skills find <topic>` → `npx skills add <owner/repo@skill> -g -y`
4. **Tools baad me** — sirf zaroorat-mand tools. Agar tool zaroori nahi, **user se puch ke** install karo. Internet se find karke sahi tool chuno.
5. **Memory me TODO** add karo; project ke parts define karo.
6. **Final Plan Summary** do: kya banega, stack, free hosting, parts, risks.
7. **User confirmation** ke BAAD hi build shuru karo.

## 3. BRAIN FILE (COMPULSORY — har project me)
- ⚠️ **Har naye project ke root me `BRAIN.md` banana COMPULSORY hai** — BRAIN.md ke bina koi project start NAHI hoga.
- Isme sirf: (a) project kya hai (b) kya karna hai / next steps (c) stack + free hosting plan (d) parts + status (e) important decisions.
- Wapas kaam karte waqt **sabse pehle BRAIN.md padho**. Har session ke baad update karo.

## 4. TESTING BEFORE REPORTING
- APK/web banne ke baad **khud saare options aur saare functions test karo**.
- Sab actually kaam kare tabhi user ko "done" batao.
- Bina test kiye complete mat bolo.

## 5. ISSUE REPORTING
Agar build ke time issue aaye, user ko batao:
1. Ye issue hai
2. Ye aise solve hoga
3. Ye changes honge
Fir **YES/NO** pucho. Permission ke bina approach change mat karo.

## 6. PERSISTENCE
- Ye rules har naye chat/session me follow karo.
- Important project state: Memory + BRAIN.md dono me save karo.

## 7. CONTINUOUS LEARNING (AI khud seekhta rahe)
- **Kaam se pehle:** `~/.agents/knowledge/LEARNINGS.md` me relevant tags search karo (`grep -i`), pehle seekhi cheez apply karo.
- **Kaam ke baad:** Non-trivial problem solve hua / naya tool-quirk mila / reusable command sequence mila → usi din concise entry add karo (date, tags, problem, cause, fix, verify, reuse).
- **Secrets kabhi mat likho** (tokens/passwords/keys) knowledge base me.
- **Project-specific** lessons project ke `BRAIN.md` me; **general** lessons global `LEARNINGS.md` me.
- **Session end:** BRAIN.md update + max 3 best lessons `LEARNINGS.md` me.
- File 400+ lines ho jaye to consolidate karo (duplicates merge → Playbook).
- Skill: `~/.agents/skills/learnings/SKILL.md`

## 8. TOKEN 80% → COMPACT FIRST (SESSION SAME)
- Har major step ke baad check karo: `token-usage` (ya `token-usage --json`).
- Jab context usage **80% ya zyada** ho:
  1. Turant user ko batao: "⚠️ Token 80% ho gaye hain."
  2. Options do (line-wise):
     - A. **Compact** — session same, context free (recommended): `context-reset --apply`
     - B. **Model switch** — free model rotate: `model-rotate --apply`
     - C. **Port rotate** — server address change: `port-rotate --apply`
  3. User ke YES par chuna hua action chalao.
  4. Verify: `token-usage` + `opencode service status`.
- Pools: `~/.config/opencode/ports.txt` (20 ports, 28960–28979) · `~/.config/opencode/models.txt` (9 free models) — line-wise.
- State: `port-index` / `model-index` · Logs: `~/.local/state/opencode/`.
- ⚠️ Port change tokens reset NAHI karta. Context free karne ke liye compact; zyada capacity ke liye free model switch.
- Auto-compaction ~85% pe khud hota hai; manual 80% pe better control.
