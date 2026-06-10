# gcore-marketplace

A Claude Code plugin marketplace with Gcore MCP servers.

## Plugins

| Plugin | MCP server | What it does |
|---|---|---|
| `gcore-hubspot` | `gcore-hubspot` (npx) | HubSpot MCP |
| `gcore-atlassian` | `mcp-atlassian` (uvx) | Jira MCP |
| `gcore-supabase` | `supabase-selfhosted` (mcp-remote) | Self-hosted Supabase MCP |

## Prerequisites

- [Claude Code](https://claude.com/claude-code) installed.
- `node` / `npx` available (for hubspot and supabase plugins).
- `uvx` available — install with `pipx install uv` or `brew install uv` (for the atlassian plugin).

## Step 1 — Set the secrets (env vars)

Secrets are **not** stored in this repo. Each MCP reads them from environment
variables. Add them to your shell profile (`~/.zshrc`), then restart your shell.

```sh
# gcore-hubspot
export HUBSPOT_ACCESS_TOKEN="..."

# gcore-atlassian (Jira)
export PYPI_USERNAME="..."          # for the private pypi index
export PYPI_PASSWORD="..."
export JIRA_PERSONAL_TOKEN="..."

# gcore-supabase
# base64 part from "Authorization: Basic <THIS>"
export SUPABASE_AUTH_BASIC="..."
```

> Tip: you only need the vars for the plugins you actually install.

## Step 2 — Add the marketplace

**Local (for testing):**

```
/plugin marketplace add /Volumes/ssd/code/github.com/zmiievskyi/cc-thingz
```

**From GitHub (after you push):**

```
/plugin marketplace add zmiievskyi/cc-thingz
```

## Step 3 — Install plugins

```
/plugin install gcore-hubspot@gcore-marketplace
/plugin install gcore-atlassian@gcore-marketplace
/plugin install gcore-supabase@gcore-marketplace
```

The MCP server starts automatically when the plugin is enabled.

## Step 4 — Test

1. Check the marketplace was added:
   ```
   /plugin marketplace list
   ```
2. Check the plugin is installed:
   ```
   /plugin list
   ```
3. Check the MCP connection (server should be `connected`):
   ```
   /mcp
   ```
4. Ask Claude to use a tool from the server, e.g. "list my Jira projects"
   or "search HubSpot contacts".

## Updating

After you change plugin files and push:

```
/plugin marketplace update gcore-marketplace
```

## Notes

- `gcore-supabase` sets `NODE_TLS_REJECT_UNAUTHORIZED=0` to allow a self-signed
  TLS certificate on the internal Supabase host. This disables certificate
  checking for that process — expected for internal use, but keep it in mind.
- `gcore-atlassian` defaults `JIRA_PROJECTS_FILTER` to `ISVS`. Edit
  `plugins/gcore-atlassian/.claude-plugin/plugin.json` to change it.

## Troubleshooting

- **MCP shows `failed` in `/mcp`** — a secret env var is missing or wrong.
  Re-check Step 1 and restart Claude Code so it picks up new env vars.
- **`uvx` / `npx` not found** — install the prerequisite (see above).
- **Marketplace not found** — make sure the path in Step 2 points to the repo
  root (the folder that contains `.claude-plugin/marketplace.json`).
