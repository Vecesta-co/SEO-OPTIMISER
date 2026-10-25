# SEO Optimization Toolkit — Local Integration Guide

> **Local repos cloned in this directory:**
> - `mcp-gsc/` (AminForou) — Google Search Console MCP server
> - `open-seo/` (every-app) — All-in-one AI-native SEO tool
> - `geo-optimizer-skill/` (Auriti-Labs) — Generative Engine Optimization
> - `claude-seo/` (AgriciDaniel) — Universal SEO auditing (26 sub-skills + 19 agents)

This guide teaches you how to use all four repos together for the best SEO optimization.

---

## 1. What Each Local Repo Does (Real Files Read)

### `mcp-gsc/` — Google Search Console MCP Server
- **Entry:** `gsc_server.py` (~1,670 lines, FastMCP-based)
- **Config:** `.mcp.json` / `mcp.json` — defines the `mcp-search-console` server
- **Requirements:** `google-api-python-client`, `google-auth-oauthlib`, `mcp[cli]>=1.3.0,<2.0.0`
- **Key capability:** 20 tools (`get_capabilities`, `list_properties`, `get_search_analytics`, `inspect_url_enhanced`, `batch_url_inspection` up to 10 URLs, `compare_search_periods`, `get_advanced_search_analytics`, sitemap tools, etc.)
- **Auth modes:** OAuth (desktop client secrets JSON) OR Service Account (`GSC_CREDENTIALS_PATH` + `GSC_SKIP_OAUTH=true`)
- **Env vars:** `GSC_OAUTH_CLIENT_SECRETS_FILE`, `GSC_DATA_STATE` (`all` vs `final`), `GSC_ALLOW_DESTRUCTIVE`
- **Local run:** `uv sync` then `uv run python gsc_server.py` (needs Python 3.11+)

### `open-seo/` — OpenSEO (Semrush/Ahrefs Alternative)
- **Entry:** `package.json` (v0.1.13, TypeScript / Vite / React)
- **Requires:** `DATAFORSEO_API_KEY` (Base64-encoded `login:password`)
- **Self-host:** Docker (`docs/SELF_HOSTING_DOCKER.md`) OR Cloudflare Workers (`compose.yaml`, `wrangler.jsonc`)
- **Key features:** Keyword research, rank tracking, competitor insights, backlinks, site audits, AI visibility workflows
- **MCP:** `@modelcontextprotocol/server` v2.0.0 included in `package.json`
- **Plugins:** `plugins/openseo/` with `.claude-plugin/` and `.cursor-plugin/`
- **Local dev:** `pnpm install`, `pnpm dev`

### `geo-optimizer-skill/` — GEO Optimizer (AI Citation Readiness)
- **Entry:** `src/geo_optimizer/` (Python 3.9+, Click CLI, FastAPI web demo)
- **Install:** `pip install -U geo-optimizer-skill` or `uvx --from geo-optimizer-skill geo audit --url ...`
- **CLI commands (16):** `geo audit`, `geo fix`, `geo llms`, `geo schema`, `geo diff`, `geo history`, `geo coherence`, `geo monitor`, `geo track`, `geo citations`, `geo snapshots`, etc.
- **MCP server:** `pip install "geo-optimizer-skill[mcp]"` then `claude mcp add geo-optimizer -- geo-mcp`
- **12 MCP tools:** `geo_audit`, `geo_fix`, `geo_llms_generate`, `geo_citability` (47 checks), `geo_schema_validate`, `geo_compare`, `geo_gap_analysis`, `geo_ai_discovery`, `geo_check_bots`, `geo_trust_score`, `geo_negative_signals`, `geo_factual_accuracy`
- **Scoring:** 9 categories / 100 pts (Google AI readiness 20pts, Robots 14, Schema 14, Content 14, Brand 12, Meta 11, llms.txt 6, Signals 6, AI Discovery 3)
- **Research-backed:** Based on Princeton KDD 2024 GEO paper (9 methods ranked by impact: Cite Sources +30–115%, Add Statistics +40%, Quotation Addition +30–40%, etc.)
- **Key output:** `llms.txt`, `robots.txt` rules for 27 AI crawlers, JSON-LD schema, meta tags, AI discovery endpoints
- **No API key required for basic audit** — citation checks (`geo citations`) need Perplexity/OpenAI/Anthropic/Groq/Gemini keys

### `claude-seo/` — Claude SEO (Universal Auditing)
- **Entry:** `CLAUDE.md` (project instructions) + `skills/seo/` (26 sub-skills) + `agents/` (19 agents)
- **Install:** Plugin marketplace (`/plugin marketplace add AgriciDaniel/claude-seo`) OR `git clone` + `bash install.sh` / PowerShell `install.ps1`
- **Commands:** `/seo audit <url>` (full site, up to 17 parallel agents), `/seo page`, `/seo technical`, `/seo content`, `/seo schema`, `/seo geo`, `/seo agentic`, `/seo sitemap`, `/seo backlinks`, `/seo local`, `/seo maps`, `/seo drift`, `/seo ecommerce`, `/seo hreflang`, etc.
- **Requires:** Python 3.10+, Claude Code CLI, optional Playwright Chromium (`install.sh` installs it)
- **Features:** 26 sub-skills across 9 categories (technical, E-E-A-T, schema, GEO/AEO, agent readiness, backlinks, local SEO, commerce, international); 19 specialist agents; falsifiable recommendations with failure checks; PDF report generation (`scripts/google_report.py` via WeasyPrint + matplotlib)
- **4-tier credential system:** Tier 0 (API key only → PageSpeed + CrUX), Tier 1 (+ GSC/Indexing), Tier 2 (+ GA4), Tier 3 (+ Ads Keyword Planner)
- **Security rules:** `url_safety.py` for SSRF protection, `.env`/credentials never committed, `~/.config/claude-seo/` with `0o600` permissions

---

## 2. How They Work Together (Integration Workflow)

This is the technique I created for combining them:

### Phase 1 — Foundation (Data + Intelligence)

**Run together:**
```bash
# A. Start GSC MCP locally (from mcp-gsc/)
cd mcp-gsc
uv sync
uv run python gsc_server.py
# Then configure in Claude Desktop: command points to this .venv/bin/python

# B. Start OpenSEO (from open-seo/)
cd open-seo
# Set DATAFORSEO_API_KEY in .env
pnpm dev  # or docker/run
# Access at localhost:3000 for UI; MCP available via plugin
```

**What this gives:** Real GSC property data (queries, impressions, CTR, positions) + OpenSEO competitive/keyword data (DataForSEO-backed).

### Phase 2 — Technical Audit + E-E-A-T

**From `claude-seo/`:**
```bash
# Inside Claude Code (after /plugin install)
/seo audit https://yourdomain.com
/seo technical https://yourdomain.com
/seo content https://yourdomain.com
/seo schema https://yourdomain.com
```

**Key outputs:** Full audit report with 26 sub-skill coverage, technical SEO scorecard (9 categories), E-E-A-T evaluation, schema validation/generation, Core Web Vitals (LCP/INP/CLS), page-level screenshots, prioritized action plan with falsifiability checks.

### Phase 3 — AI / GEO Readiness (The Missing Piece)

**From `geo-optimizer-skill/`:**
```bash
# Zero install
uvx --from geo-optimizer-skill geo audit --url https://yourdomain.com

# With local install
pip install -U geo-optimizer-skill
geo audit --url https://yourdomain.com --format json
geo fix --url https://yourdomain.com --apply  # writes fixes

# MCP integration
pip install "geo-optimizer-skill[mcp]"
claude mcp add geo-optimizer -- geo-mcp
# Then ask Claude: "audit my site and fix what's missing"
```

**What this adds:** Score 0–100 on AI citation readiness, 27 AI crawlers checked, `llms.txt` generation, JSON-LD schema injection, robots.txt rules for AI bots, content improvements based on Princeton KDD 2024 findings.

### Phase 4 — Combined Optimization (The Best Technique)

Use Claude's own `use.md` (this file) + all repos to run this integrated prompt sequence:

```text
1. /seo audit https://yourdomain.com              # Claude SEO full audit
2. geo audit --url https://yourdomain.com         # GEO readiness score
3. "Call get_capabilities" on GSC MCP            # Verify GSC connection
4. "Show me top 20 queries for mydomain.com last 30 days"  # GSC analytics
5. /seo geo https://yourdomain.com                # Claude SEO GEO analysis
6. geo fix --url https://yourdomain.com --apply    # Auto-fix GEO gaps
7. /seo schema https://yourdomain.com              # Schema validation + generation
```

**Why this is the best technique:**
- **GSC MCP** gives live performance data
- **Claude SEO** gives comprehensive technical/content/AI audits + 17 parallel agents
- **GEO Optimizer** specifically targets AI engine visibility (ChatGPT, Perplexity, Gemini, Claude, Google AI Overviews)
- **OpenSEO** provides competitive/keyword intelligence via DataForSEO
- Together they cover: traditional rankings (GSC + Claude SEO) + AI citations (GEO + Claude SEO geo) + competitive intelligence (OpenSEO) + technical quality (Claude SEO)

---

## 3. Local Setup Commands (Use These Directly)

```bash
# Clone all four (already done in this directory)
git clone https://github.com/AminForou/mcp-gsc.git
git clone https://github.com/every-app/open-seo.git
git clone https://github.com/Auriti-Labs/geo-optimizer-skill.git
git clone https://github.com/AgriciDaniel/claude-seo.git

# Setup mcp-gsc (Python 3.11+ needed)
cd mcp-gsc
uv venv .venv
uv pip install -r requirements.txt
# Configure: edit mcp.json to point to .venv/bin/python + gsc_server.py

# Setup geo-optimizer (Python 3.9+, instant)
pip install -U geo-optimizer-skill
geo audit --url https://example.com   # Test immediately, no keys needed

# Setup open-seo (Node, needs DataForSEO key)
cd open-seo
# Copy .env.example to .env, set DATAFORSEO_API_KEY
pnpm install
# Or use Docker for simple testing

# Setup claude-seo (Claude Code plugin + Python)
cd claude-seo
# Plugin install: /plugin marketplace add AgriciDaniel/claude-seo
# Manual: bash install.sh (creates isolated .venv in Claude plugin data)
```

---

## 4. Local Integration File References

From the repos I just read:

- `mcp-gsc/mcp.json` → MPC server definition
- `mcp-gsc/CLAUDE.md` → Running locally (`uv sync`, auth, env vars)
- `open-seo/.env.example` → `DATAFORSEO_API_KEY`
- `open-seo/compose.yaml` → Docker self-host
- `geo-optimizer-skill/.mcp.json` → MCP config
- `geo-optimizer-skill/pyproject.toml` → Python package config
- `geo-optimizer-skill/LOCAL_SETUP.md` → Local development setup
- `claude-seo/CLAUDE.md` → Full architecture (skills/, agents/, scripts/, 4-tier credentials)
- `claude-seo/requirements.txt` → Security-pinned deps (beautifulsoup4, requests, lxml, playwright, weasyprint, numpy, matplotlib)
- `claude-seo/install.sh` / `install.ps1` → Setup scripts
- `claude-seo/skills/seo/` → 26 sub-skills directory
- `claude-seo/agents/` → 19 agent files

---

## 5. Best Practice Sequence (Use This for Self-Teaching)

1. **Start with GEO** (`geo audit`) — fastest score, no auth, tells you what's broken for AI engines
2. **Run Claude SEO audit** (`/seo audit`) — comprehensive technical + content + E-E-A-T + AI (GEO) analysis
3. **Connect GSC** (`get_capabilities` via MIDI server from `mcp-gsc/`) — get real search data
4. **Fix GEO gaps** (`geo fix --apply`) — robots.txt, llms.txt, schema, meta
5. **Validate with Claude SEO** (`/seo geo`, `/seo schema`) — confirm fixes
6. **Monitor with GSC** (`compare_search_periods`, `get_advanced_search_analytics`) — measure impact
7. **Add OpenSEO** for competitive/keyword data when ready (needs DataForSEO key)

---

*This file was written after cloning all 4 repos locally (`mcp-gsc/`, `open-seo/`, `geo-optimizer-skill/`, `claude-seo/`) and reading their actual README.md, CLAUDE.md, requirements.py, package.json, SKILL.md, and AGENTS.md files. It teaches you exactly how to combine them for maximum SEO optimization.*
