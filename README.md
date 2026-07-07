# Echo Zero Developer Docs

Mintlify documentation for the Echo Zero MCP API. Prose guides live in this repo; the **API Reference** tab is generated from the live OpenAPI spec at `https://mcp.echozero.app/api/docs-json`.

## Local preview

```bash
npm i -g mintlify
mintlify dev
```

Open the URL printed by the CLI (usually `http://localhost:3000`).

## Deploy

Mintlify auto-deploys when changes merge to the connected GitHub repo (`EchoZeroApp/echozero-docs`). See [SETUP.md](./SETUP.md) for first-time Mintlify dashboard and DNS steps.

## Repo layout

| Path | Purpose |
| --- | --- |
| `docs.json` | Mintlify site config, navigation, OpenAPI URL |
| `index.mdx` | Introduction |
| `quickstart.mdx` | Getting started |
| `guides/` | Integration guides (placeholders; prose PRs welcome) |
| `logo/`, `favicon.svg` | Branding assets |

## OpenAPI source

The API reference is **not** checked into this repo. Mintlify fetches `https://mcp.echozero.app/api/docs-json` at build time. When mcp-main ships new endpoints, redeploy docs (or wait for Mintlify’s scheduled rebuild) to refresh the reference.
