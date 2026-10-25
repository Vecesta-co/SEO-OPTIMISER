# SEO Optimiser

An integration guide for combining four independent SEO and AI-visibility tools:

- [Google Search Console MCP](https://github.com/AminForou/mcp-gsc) for Search Console analytics
- [OpenSEO](https://github.com/every-app/open-seo) for keyword research and competitive analysis
- [GEO Optimizer](https://github.com/Auriti-Labs/geo-optimizer-skill) for generative engine optimization
- [Claude SEO](https://github.com/AgriciDaniel/claude-seo) for technical and content audits

See [`use.md`](use.md) for setup instructions, configuration examples, and the integrated workflow.

## Clone

The four tools are maintained as separate Git repositories and are included here as submodules. Clone this repository with its tools:

```sh
git clone --recurse-submodules <repository-url>
```

If you already cloned it without submodules:

```sh
git submodule update --init --recursive
```

## Credentials

Use the provided `.env.example` and `mcp-config-example.json` as templates. Put real credentials in local, untracked configuration files; never commit API keys, OAuth client secrets, service-account files, or tokens.

## Licensing

The MIT license in this repository applies to the integration materials authored for this repository. Each submodule is an independently maintained project; its own license and attribution apply to its contents.
