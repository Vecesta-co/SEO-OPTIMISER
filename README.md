# SEO Optimiser

SEO Optimiser is a practical workflow for improving a site's search performance and readiness to appear in AI-generated answers. It brings together four independent tools so a team can connect search data, find opportunities, diagnose site issues, make targeted improvements, and measure what changes over time.

This repository is an integration guide and workspace—not a single application that installs or runs all four tools automatically. Each tool has its own setup, dependencies, and license. [`use.md`](use.md) contains the detailed local setup instructions, configuration examples, commands, and end-to-end workflow.

## What the workflow helps you do

- **Find opportunities from real search behavior.** Use Google Search Console (GSC) queries, impressions, click-through rates, positions, and URL inspection to identify pages that are underperforming or need attention.
- **Research what to target.** Use OpenSEO's keyword, competitor, backlink, and rank-tracking capabilities to investigate search demand and competing sites. Its DataForSEO-backed features require your own API credentials.
- **Diagnose and prioritize fixes.** Use Claude SEO for technical, on-page, content, schema, local, and other specialist audits. Its recommendations can be investigated and validated rather than treated as guaranteed ranking outcomes.
- **Improve AI-search discoverability.** Use GEO Optimizer to check AI citation readiness and review areas such as content citability, structured data, robots rules, and `llms.txt`.
- **Close the loop.** Re-run relevant checks and compare GSC performance after changes, so the team can see whether the work produced measurable results.

No tool can guarantee rankings, traffic, or citations. Treat audit output as evidence and recommendations to review against your site, goals, and data.

## How the pieces work together

| Stage | Tool | Role |
| --- | --- | --- |
| Establish a baseline | Google Search Console MCP | Connect a verified property and inspect queries, pages, search trends, and URLs. Its MCP server makes Search Console functions available to a compatible MCP client. |
| Research demand and competition | OpenSEO | Explore keywords and competitors, then use the findings to choose pages and topics to investigate. |
| Audit the site | Claude SEO | Run focused or broad SEO reviews covering technical health, content, schema, local SEO, and related areas. |
| Check AI visibility | GEO Optimizer | Audit readiness for AI discovery and citations; inspect suggested improvements such as schema and `llms.txt`. |
| Apply and verify | Your team, with the tools above | Review recommendations before changing the site, validate the implementation, and monitor relevant GSC metrics over time. |

The tools can also be used separately. The sequence above is an example workflow, not a requirement or an automatic pipeline.

## Included projects

- **[`mcp-gsc/`](https://github.com/AminForou/mcp-gsc)** — Google Search Console analytics and URL inspection through an MCP server.
- **[`open-seo/`](https://github.com/every-app/open-seo)** — AI-native SEO research and competitive intelligence.
- **[`geo-optimizer-skill/`](https://github.com/Auriti-Labs/geo-optimizer-skill)** — GEO audits and AI-discovery tooling, available as a CLI and MCP server.
- **[`claude-seo/`](https://github.com/AgriciDaniel/claude-seo)** — SEO audit skills and specialist agents for Claude Code.

These are Git submodules pointing to their upstream repositories. This keeps the original project history, ownership, and licenses intact.

## Getting started

Clone this repository together with its submodules:

```sh
git clone --recurse-submodules <repository-url>
cd SEO-OPTIMISER
```

If you already cloned it without the tools:

```sh
git submodule update --init --recursive
```

Choose the tool or tools you need and follow the detailed instructions in [`use.md`](use.md). In brief:

1. **For Search Console data:** set up `mcp-gsc` with Python 3.11 or newer, install its dependencies with `uv sync`, and configure OAuth or a Google service account for a verified GSC property.
2. **For keyword and competitor research:** set up `open-seo` with Node.js and pnpm. Configure your own `DATAFORSEO_API_KEY` before using DataForSEO-backed features.
3. **For a quick GEO check:** install GEO Optimizer with `pip install -U geo-optimizer-skill`, then run `geo audit --url https://your-site.example`. Basic audits do not require an API key; optional citation checks may require a supported provider key.
4. **For a Claude SEO audit:** install the Claude SEO plugin using its upstream instructions, then run commands such as `/seo audit https://your-site.example` from Claude Code.
5. **For an integrated review:** use the setup and phased workflow in [`use.md`](use.md) to combine audit findings with search data and verify changes.

The commands above are starting points; each upstream project documents its complete requirements and supported options.

## Configuration and credential safety

`.env.example` and [`mcp-config-example.json`](mcp-config-example.json) are templates, not working credentials. Supply your own API keys and Google credentials in local configuration, and keep real `.env` files, OAuth client secrets, service-account keys, and tokens out of Git. Do not expose credentials in issue reports, logs, or screenshots.

## Repository layout

- [`use.md`](use.md) — detailed setup, integration steps, and operational examples
- [`CLAUDE.md`](CLAUDE.md) — toolkit overview and project-specific guidance
- [`.env.example`](.env.example) — names and placeholders for optional configuration
- [`mcp-config-example.json`](mcp-config-example.json) — example MCP server configuration
- The four submodules listed above — independently maintained tools

## Licensing

The MIT license at this repository's root applies to the integration materials authored for this repository. Each submodule retains its own upstream license and attribution; see the license file inside each project before using or redistributing its contents.
