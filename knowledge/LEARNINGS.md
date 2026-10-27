# 📚 LEARNINGS — Global Knowledge Base

> Har non-trivial kaam se seekhi cheez yahan save hoti hai. Naya session isi se purani knowledge use karta hai.
> **Rule:** Secrets/tokens/passwords kabhi yahan mat likho.
> **Skill:** `~/.agents/skills/learnings/SKILL.md`

## How to use (AI)

1. Quick Index me tag search karo:
   `grep -in "<tag>" ~/.agents/knowledge/LEARNINGS.md`
2. Sirf matching entry padho (line number se).
3. Kaam ke baad nayi entry append karo + Quick Index me row add karo.

---

## Quick Index

| ID | Date | Topic | Tags |
|---|---|---|---|
| L-001 | 2026-10-26 | OpenCode MCP env key | opencode, mcp, config |
| L-002 | 2026-10-26 | `opencode mcp add` corrupts commands | opencode, mcp, cli |
| L-003 | 2026-10-26 | npx cold start > MCP timeout | mcp, npm, timeout |
| L-004 | 2026-10-26 | jadx needs JAVA_HOME on Termux | termux, jadx, java, apk |
| L-005 | 2026-10-27 | apksigner yes, zipalign no | termux, apk, signing |
| L-006 | 2026-10-27 | gh login via token (non-interactive) | github, gh, auth |
| L-007 | 2026-10-26 | skills add needs gh/git auth | skills, gh, npx |
| L-008 | 2026-10-27 | pkg search truncated by mirror | termux, pkg |
| L-009 | 2026-10-26 | Many MCPs = token cost | mcp, tokens, opencode |
| L-010 | 2026-10-27 | Code Mode catalog rules | opencode, code-mode, tools |
| L-011 | 2026-10-27 | Memory MCP file in npx cache | mcp, memory, npx, backup |
| L-012 | 2026-10-27 | Context % via API + safe port rotation | opencode, tokens, api, port |
| L-013 | 2026-10-27 | Switch session model via API | opencode, model, api, session |

---

## L-001 — OpenCode MCP env key is `environment`, not `env`

- Date: 2026-10-26
- Tags: opencode, mcp, config
- Problem: MCP credentials (Notion/GitHub) not reaching the server → 401.
- Cause: OpenCode ignores `env` in MCP config; the correct key is `environment`.
- Fix: In `opencode.json` → `mcp.servers.<name>.environment = { "TOKEN": "..." }`.
- Verify: `opencode mcp list` shows `connected`; an API call succeeds.
- Reuse: Adding or editing any local MCP server config.

## L-002 — `opencode mcp add` corrupts command arrays

- Date: 2026-10-26
- Tags: opencode, mcp, cli
- Problem: After `opencode mcp add`, the server command lost `npx` and failed to start.
- Cause: The CLI writes the `command` array incorrectly for some servers.
- Fix: Edit `opencode.json` directly; keep `"command": ["npx", "-y", "pkg-name"]` intact.
- Verify: `opencode mcp list` → `connected`.
- Reuse: Any time adding an MCP (prefer manual JSON edit).

## L-003 — npx cold start exceeds MCP connect timeout

- Date: 2026-10-26
- Tags: mcp, npm, timeout, termux
- Problem: `Request timed out` while connecting MCP servers started via `npx`.
- Cause: First npx run downloads the package (~14s) which exceeds OpenCode's connect timeout.
- Fix: `npm i -g <pkg>`, then use the installed binary as `command` (e.g., `notion-mcp-server`, `sentry-mcp`).
- Verify: Server connects instantly after restart.
- Reuse: Any slow-starting MCP npm package.

## L-004 — jadx needs JAVA_HOME on Termux

- Date: 2026-10-26
- Tags: termux, jadx, java, apk
- Problem: `jadx --version` → "JAVA_HOME is not set and no 'java' command could be found".
- Cause: Termux jadx wrapper requires `JAVA_HOME`, even though `java` is in PATH.
- Fix: `export JAVA_HOME=$PREFIX/lib/jvm/java-21-openjdk` (or `java-17-openjdk`).
- Verify: `jadx --version` → `1.5.6`.
- Reuse: Running jadx or jadx-mcp on Termux.

## L-005 — Termux: apksigner available, zipalign missing

- Date: 2026-10-27
- Tags: termux, apk, signing
- Problem: Need to sign a rebuilt APK; `zipalign` not found.
- Cause: Termux repo ships `apksigner` but has no `zipalign` package.
- Fix: `pkg install apksigner`; sign with `apksigner sign --ks <keystore>`. Local install works unaligned. For Play Store, build via Gradle (auto-aligns) or use PWABuilder/Capacitor pipeline.
- Verify: `apksigner --version` → `0.9`.
- Reuse: Any APK re-sign step on this device.

## L-006 — gh non-interactive login using an existing PAT

- Date: 2026-10-27
- Tags: github, gh, auth
- Problem: `gh auth login` is interactive (device flow needs a browser); skills.sh installs failing due to missing auth.
- Cause: No gh auth configured; device flow requires opening a URL.
- Fix:
  ```bash
  printf '%s' "$TOKEN" | gh auth login --with-token
  gh auth setup-git
  ```
- Verify: `gh api user --jq .login` returns the username.
- Reuse: Re-authenticate after token rotation, or on a new device.

## L-007 — `npx skills add` requires git/gh access

- Date: 2026-10-26
- Tags: skills, gh, npx
- Problem: `npx skills add <repo> -g -y` failed with a GitHub auth hint.
- Cause: The skills CLI clones via git; no credentials were configured.
- Fix: Authenticate gh first (`gh auth login --with-token` + `gh auth setup-git`), then retry. Fallback: download the repo zip → copy `skills/<name>` into `~/.agents/skills/<name>`.
- Verify: `npx skills list` shows the skill.
- Reuse: Installing any skills.sh skill.

## L-008 — `pkg search` output truncated by mirror prompt

- Date: 2026-10-27
- Tags: termux, pkg
- Problem: `pkg search <name>` printed mirror-selection text and no usable results.
- Cause: No mirror group selected; pkg runs a mirror test first.
- Fix: Use `pkg list-all | grep -E '^(package)'` for reliable lookups. Optionally run `termux-change-repo` once.
- Verify: `pkg list-all` shows the expected package.
- Reuse: Checking package availability on Termux.

## L-009 — Many MCP servers = token cost per request

- Date: 2026-10-26
- Tags: mcp, tokens, opencode
- Problem: 12 connected MCP servers load many tool schemas on every request.
- Cause: Each MCP's tools are injected into the model context.
- Fix: Keep only project-relevant servers enabled; prune unused ones; prefer CLI for one-off tasks.
- Verify: Compare context/token usage before and after pruning.
- Reuse: Before adding any new MCP server.

## L-010 — Code Mode: call only catalog tools inside `execute`

- Date: 2026-10-27
- Tags: opencode, code-mode, tools
- Problem: Tools outside the Code Mode catalog cannot be called inside `execute`.
- Cause: The runtime restricts calls to catalog entries plus results from `search()`.
- Fix: Use `search({ query })` to discover the exact path, then call `tools.<namespace>["<tool>"](...)`.
- Verify: The tool call succeeds.
- Reuse: Any `execute` scripting session.

## L-011 — Memory MCP default file lives inside npx cache

- Date: 2026-10-27
- Tags: mcp, memory, npx, backup
- Problem: The memory knowledge graph file was found at `~/.npm/_npx/<hash>/node_modules/@modelcontextprotocol/server-memory/dist/memory.jsonl`.
- Cause: Without `MEMORY_FILE_PATH`, the server stores data next to its package — inside the ephemeral npx cache.
- Fix: Set `mcp.servers.memory.environment.MEMORY_FILE_PATH` to a stable path (e.g. `~/.agents/knowledge/memory.jsonl`) and copy the existing file there.
- Verify: After restart, `ls -l ~/.agents/knowledge/memory.jsonl` updates when memory tools are used.
- Reuse: Any MCP server that writes local state.

## L-012 — OpenCode context % via API + safe port rotation

- Date: 2026-10-27
- Tags: opencode, tokens, api, port, service
- Problem: Need token usage percentage, and a way to rotate the server port without killing the command mid-restart.
- Cause: Cumulative session tokens are not context usage; per-message tokens are. `opencode service set port` stops the server that spawned the shell.
- Fix:
  - Context used = last assistant message `tokens.input + output + reasoning + cache.read`; limit from `GET /api/model` (`limit.context`; 200000 for opencode/deepseek-v4.1-flash-free).
  - Rotate with a `setsid`/`nohup` detached script so `service set port` + `service start` survive the restart.
- Verify: `token-usage` prints the percentage; `port-rotate` dry-run prints the next free port; after apply, `opencode service status` shows the new port.
- Reuse: Token guard and port-rotation automation.

## L-013 — Switch session model via API (free model rotation)

- Date: 2026-10-27
- Tags: opencode, model, api, session
- Problem: Need to switch models mid-session without losing the session.
- Cause: Model selection is per session; the UI switch is manual.
- Fix: `opencode api post /api/session/<id>/model --data '{"model":{"id":"glm-5.3-flash-free","providerID":"opencode","variant":"max"}}'`
- Verify: The next assistant message shows the new model; `token-usage` uses the new limit.
- Reuse: Free-model rotation / capacity management.

## L-014 — Node 24: `node --test tests/` fails, use a file glob

- Date: 2026-10-27
- Tags: node, testing, npm, termux
- Problem: `node --test tests/` threw `Cannot find module .../tests` on Node v24.18.0.
- Cause: Newer Node expects file paths/globs, not a bare directory, for the test runner.
- Fix: `"test": "node --test tests/*.test.js"` in package.json (shell expands the glob).
- Verify: `npm test` finds and runs all test files.
- Reuse: Any Node 22+ project using the built-in test runner.

## L-015 — jsdom boot: guard double DOMContentLoaded + cache every element

- Date: 2026-10-27
- Tags: jsdom, testing, vanilla-js, dom
- Problem: DOM tests double-added tasks and crashed on `els.ringWrap.setAttribute` (undefined).
- Cause: (1) jsdom fires its own DOMContentLoaded after the test dispatch, so init ran twice; (2) a referenced element was missing from the cached `els` map.
- Fix: Add `if (init.started) return; init.started = true;` at the top of init, and keep a single cached-element map; test boot should only dispatch DOMContentLoaded if `window.__kaamUI` is absent.
- Verify: Add-one-task test yields exactly 1 item; all DOM tests pass.
- Reuse: Any vanilla JS app tested under jsdom.

## L-016 — Free private backup + GitHub Pages deploy via `gh`

- Date: 2026-10-27
- Tags: github, gh, backup, pages, deploy
- Problem: Needed a private backup of agent knowledge/skills and a free live host for the app.
- Cause: GitHub Pages is free for public repos; private repos need a paid plan for Pages.
- Fix:
  - Backup: `agent-backup` copies knowledge/skills/scripts to `~/agent-brain`, scans for secret patterns (aborts on match), commits, creates a **private** repo and pushes.
  - Deploy: `gh repo create <user>/<repo> --public --source . --push`, then `printf '{"source":{"branch":"main","path":"/"}}' | gh api -X POST repos/<user>/<repo>/pages --input -`.
- Verify: `gh repo view <repo> --json isPrivate`; build status `gh api repos/<user>/<repo>/pages/builds/latest`; live URL returns HTTP 200 for every asset.
- Reuse: Every free static project + agent-brain backup.

## L-017 — Build a native Android APK on Termux without Gradle

- Date: 2026-10-27
- Tags: android, apk, termux, aapt2, d8, javac, no-gradle
- Problem: Need a real native APK on Termux, but no Android SDK manager, Gradle, or build-tools.
- Cause: Termux has `aapt2`/`apksigner`/`javac` but no `android.jar`, `d8`, or platform.
- Fix:
  1. `android.jar` API 34 from `Sable/android-platforms`; `r8.jar` from Google Maven (`com/android/tools/r8/9.5.28`).
  2. `aapt2 compile --dir res -o res.zip` → `aapt2 link -I android.jar --manifest ... -R res.zip --java gen`.
  3. `javac -source 8 -target 8 -bootclasspath android.jar -d classes @sources`.
  4. `java -cp r8.jar com.android.tools.r8.D8 --release --lib android.jar --min-api 21 --output dex @classes`.
  5. Append `classes.dex` to the APK with Python `zipfile` in append mode.
  6. `apksigner sign` + `verify` (v1/v2/v3).
- Verify: `aapt2 dump badging`, `jadx` decompile, `apksigner verify --verbose`.
- Reuse: Any native Android build without Android Studio.

## L-018 — resources.arsc must be stored and 4-byte aligned (Android 11+)

- Date: 2026-10-27
- Tags: android, apk, zipalign, resources.arsc, termux
- Problem: APK built on Termux had `resources.arsc` at an unaligned offset; Android 11+ may reject installs for targetSdk 30+.
- Cause: `zipalign` is not available in Termux, and `aapt2 link` output happened to place the file at an odd offset.
- Fix: Python rebuilds the zip and inserts a `0xD935` alignment extra field (4–7 bytes) before every stored entry so data offsets are 4-byte aligned; keep `resources.arsc` uncompressed. Align **before** signing; apksigner preserves local entry offsets. Verify data offsets with `header_offset + 30 + name_len + extra_len`.
- Verify: all stored entries print `align4=True`; `resources.arsc` method=stored.
- Reuse: Every APK built without zipalign.

## L-019 — GitHub release asset 404: use `?download=1`; `file#label` may not rename

- Date: 2026-10-27
- Tags: github, release, gh, apk, download
- Problem: Public release asset URL returned HTTP 404 (9-byte "Not Found") with plain curl, though `gh api` showed state=uploaded and size correct.
- Cause: GitHub edge behavior/user-agent dependent; the release page link works in a browser, and `?download=1` reliably returns 302 → CDN.
- Fix: Use `.../releases/download/<tag>/<asset>?download=1` for scripts/README. Also: `gh release create tag file#new-name` did **not** rename the asset in this gh version — copy the file to the desired name and `gh release upload` it, then delete the old asset.
- Verify: `curl -L` the `?download=1` URL, compare SHA-256, run `apksigner verify` on the downloaded APK.
- Reuse: Any GitHub release binary download.
