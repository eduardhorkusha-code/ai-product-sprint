# Claude Code Infrastructure & Bootcamp Context
**Source:** Gemini Deep Research  
**Date:** 2026-06-04  
**Purpose:** Technical reference for SKELAR × Claude Bootcamp preparation

---

## 1. Local Infrastructure Setup

### System Requirements
- Node.js v18+ (required for MCP server management, local scripts, hooks)
- Install: `npm install -g @anthropic-ai/claude-code`
- Recommended: use official native installer (bundled Node.js, avoids version conflicts)

### Corporate network issues
```bash
export HTTPS_PROXY=http://proxy.example.com:port
export NODE_EXTRA_CA_CERTS=/path/to/company-ca.pem  # SSL self-signed cert fix
```

### Common Errors
| Error | Fix |
|-------|-----|
| EACCES on install | `npm config set prefix ~/.npm-global` (never use sudo) |
| Node conflict in WSL | `export PATH="$HOME/.nvm/versions/node/$(nvm current)/bin:$PATH"` in ~/.bashrc |
| SSL SELF_SIGNED_CERT_IN_CHAIN | Set NODE_EXTRA_CA_CERTS to company CA |
| OAuth blocked in SSH/container | Copy link → authorize on any device → paste code back |

### Key CLI Flags
| Command | Use |
|---------|-----|
| `claude` | Interactive session in current dir |
| `claude "query"` | Start with initial prompt |
| `claude -p "query"` | Non-interactive (CI/CD pipelines) |
| `claude -c` | Resume last conversation |
| `claude --worktree <name>` | Isolated Git worktree session |
| `claude auth login --console` | API billing (not subscription) |
| `claude doctor` | Local environment diagnostics |

---

## 2. MCP Protocol — Integrations

### Architecture
```
MCP HOST (Claude Desktop / CLI)
  └─ MCP CLIENT (translates to JSON-RPC)
       └─ MCP SERVER (DB / filesystem / API access)
```

Servers self-describe their tools at connection time — no hardcoded integration code needed.

### Config file locations
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

### Key Connectors
| Service | Auth | Read | Write |
|---------|------|------|-------|
| Slack MCP | OAuth 2.0 | Search messages, read threads, channels | Send messages, create channels, post canvas, react |
| Notion MCP | Internal Integration Token | Search workspace, query DBs, read pages | Create pages, add DB records, edit blocks |
| Google Workspace CLI | OAuth2 / Service Account | Drive files, Docs export, Sheets, Calendar | Upload files, append to Docs, create events |

**Notion gotcha:** after creating the integration, you MUST manually connect it to specific pages/databases inside Notion UI — otherwise search returns empty even with valid auth.

---

## 3. Parallel Agents & Git Worktrees

### 4 Coordination Models
1. **Subagents** — narrow isolated tasks (security audit, fix specific tests) in own context window
2. **Agent View** (`claude agents`) — monitor/manage multiple parallel sessions
3. **Agent Teams** — manager agent decomposes → distributes → collects results
4. **Dynamic Workflows** — scripts orchestrating many subagents for large migrations

### Git Worktrees — Why They Matter
Running multiple agents on one codebase creates: merge conflicts, port collisions, DB transaction conflicts.

```bash
claude --worktree <name>
# Creates: .claude/worktrees/<name>/ on a new branch
```

Config for copying local uncommitted state:
```json
{ "worktree": { "baseRef": "head" } }
```

Auto-cleanup after merge. Use dynamic port allocation + Neon PostgreSQL branch per agent to avoid collisions.

---

## 4. Token / Context Math

```
T_total = T_system + T_claude_md + T_files + T_skills + T_history
```

- `T_system` — fixed Claude Code system prompt
- `T_claude_md` — your CLAUDE.md loaded every session
- `T_skills` — **lazy-loaded**: only metadata (~10 tokens/skill) until invoked

→ Skills save context: full instructions load only when the skill is actually called.

---

## 5. Memory Systems

| Layer | Storage | Scope | Update |
|-------|---------|-------|--------|
| CLAUDE.md | Markdown in repo root | Shared team rules | Manual + git |
| Auto Memory | `.claude/` local markdown | Individual project context | Auto by model |
| Auto Dream | Background consolidation service | Local idle cleanup | Auto on idle |
| memsearch | Local vector DB | Cross-agent knowledge base | Auto-indexed |

**Auto Dream formula:** runs semantic compression during idle time, resolves contradictions, keeps memory under 200 lines.

### CLAUDE.md Best Practices
- Max 200-300 lines (longer = model ignores rules)
- Only project-specific rules — no generic "write clean code"
- Hierarchical: root CLAUDE.md = global; `/src/components/CLAUDE.md` = module-specific
- Be precise: "2 spaces for indentation", not "use proper indentation"
- Evolve it: add rules every time the model makes a repeated mistake

---

## 6. Custom Skills — Structure

```
.claude/skills/my-skill/
  SKILL.md          # YAML metadata + instructions
  template.md       # optional templates
  scripts/
    run-check.sh    # optional validation scripts
```

### SKILL.md format
```yaml
---
name: skill-name
description: One-line description (used for lazy-loading decisions)
user-invocable: true
disable-model-invocation: true   # if true — only /skill-name, model can't auto-invoke
argument-hint: "[arg]"
allowed-tools:
---
```

### Debugging Skills
```bash
/context    # shows all loaded system prompts + skills + MCP tools
/skills     # lists all indexed skills; user-only flag = disable-model-invocation
cd /tmp && CLAUDE_CONFIG_DIR=/tmp/claude-clean claude   # test in clean env
```

---

## 7. Bootcamp Context — SKELAR

### Track: "Creating Digital Product"
- **Date:** 7 June 2026 (per repo README) / 30 May per research doc
- **Duration:** Full day (see sprint flows in CLAUDE.md)
- **Output:** Working mobile productivity app demo
- **Lead mentor:** @Andrii Zadorozhnyi
- **Competition:** Teams demo, voting in #platform_ai channel

### Pre-bootcamp Checklist (per participant)
- [ ] Claude Team plan activated (corporate SKELAR email)
- [ ] Claude Desktop installed
- [ ] Node.js v18+ installed → `node --version`
- [ ] Claude Code CLI installed → `claude doctor`
- [ ] Slack + Google Drive connectors configured in Claude Desktop Settings → Connectors
- [ ] 1-2 real work tasks prepared: pain → data source → desired output
- [ ] Real files ready: xlsx / csv / docx template

### Other Teams (for competitive context)
| Group | Focus |
|-------|-------|
| 1 | HR/Recruiting automation — candidate tracker, offer/NDA generation |
| 2 | PM/Feedback — Slack+email+Notion aggregation, weekly digests |
| 3 | Ops monitoring — status reports from Notion, KPI dashboards |
| 4 | Marketing — competitor monitoring, one-pagers, unit economics |
| 5 | Finance — budget calculators, act/invoice generation, P&L analysis |
| 6 | Legal — contract review (PDF red flags), NDA generation |

---

## 8. Four Bootcamp Skills (ready templates)

### memory-configurator
Validates CLAUDE.md presence, checks Node versions, creates `.claude/rules/`, prints memory status table.

### history-analyzer  
`git log -n 10` + `git blame` on a file → architectural decision report table (date, author, change, refactor risk).

### hr-onboarding-assistant
Reads onboarding template → fetches profile from GWS Drive → generates personalized guide → drafts Gmail reminder.

### talent-marketing-monitor
Searches Slack recruitment channels + GWS Drive resume folder → unified `recruitment-status.md` → Slack canvas card.
