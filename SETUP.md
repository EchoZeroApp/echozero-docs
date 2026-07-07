# Mintlify setup checklist

One-time steps to go from this repo to a live site at `docs.echozero.app`.

## 1. Create the GitHub repo

1. Create an empty public repo: **EchoZeroApp/echozero-docs** (do **not** initialize with a README).
2. Push this directory:

```bash
cd echozero-docs
git init
git add .
git commit -m "Initial Mintlify docs site"
git branch -M main
git remote add origin git@github.com:EchoZeroApp/echozero-docs.git
git push -u origin main
```

## 2. Mintlify dashboard

1. Sign up at [mintlify.com](https://mintlify.com) (free tier; GitHub sign-in).
2. **Create new project** → connect `EchoZeroApp/echozero-docs` for deployments (the docs content repo).
3. Select the **Starter** template if prompted (this repo already has `docs.json` + MDX pages).
4. Mintlify will detect `docs.json` and start a preview deploy on push.

**GitHub icon wrong URL?** Sidebar and footer links come from `docs.json` (`EchoZeroApp/echozero-sdk`). The Mintlify **deployment** repo must be `EchoZeroApp/echozero-docs` (dashboard → Settings → GitHub). Redeploy after pushing config changes.

## 3. Verify the preview build

After the first deploy, open the Mintlify preview URL (e.g. `https://echozero-docs.mintlify.app`).

Checklist:

- [ ] Home page loads with Echo Zero branding
- [ ] **API Reference** tab lists endpoints from the live spec
- [ ] Open a known endpoint (e.g. `GET /api` health) and confirm request/response shapes match mcp-main
- [ ] API playground shows server URL `https://mcp.echozero.app` (requires mcp-main deploy with OpenAPI `servers` field)

Confirm the spec is public and includes a server URL for Mintlify code samples:

```bash
curl -I https://mcp.echozero.app/api/docs-json   # Expect: HTTP/2 200
curl -sS https://mcp.echozero.app/api/docs-json | python3 -c "import json,sys; print(json.load(sys.stdin).get('servers'))"
# Expect: [{'url': 'https://mcp.echozero.app', 'description': 'Production'}]
```

If `servers` is empty, Mintlify falls back to placeholder `api.example.com` in curl examples.

## 4. Custom domain (`docs.echozero.app`)

Mintlify **custom domains may require a paid plan** — check the current free-tier feature list in the dashboard.

### If custom domain is available

1. Mintlify dashboard → **Settings → Custom domain** → add `docs.echozero.app`.
2. In your DNS provider, add the CNAME Mintlify provides (typically `docs` → `cname.mintlify.com` or similar).
3. Wait for TLS provisioning (often 15–60 minutes).
4. Confirm: `curl -I https://docs.echozero.app` → `200`.

### If custom domain is Pro-only

Use the Mintlify subdomain (`echozero-docs.mintlify.app`) until you upgrade. Update landing-page links temporarily, or set a DNS redirect from `docs.echozero.app` → the Mintlify URL at your CDN/DNS layer.

## 5. Report back

Share these three URLs with the team:

| Item | Example |
| --- | --- |
| Live docs URL | `https://docs.echozero.app` or `https://echozero-docs.mintlify.app` |
| Docs repo | `https://github.com/EchoZeroApp/echozero-docs` |
| OpenAPI JSON | `https://mcp.echozero.app/api/docs-json` |

## 6. Landing page

The `/developers` page on echozero.app already links to `https://docs.echozero.app`. Once DNS + Mintlify are live, the "View the full API reference →" link will resolve.

## Ongoing

- **Prose updates**: edit MDX in `guides/` and open PRs; Mintlify redeploys on merge.
- **API changes**: ship endpoints in mcp-main; Mintlify rebuild picks up the latest `docs-json` automatically.
- **Local preview**: `mintlify dev` from this repo root.
