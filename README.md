# Claude Code plugin marketplace template

This directory is **scaffolding for a separate GitHub repo**, not part of the
`@chaindoc_io/mcp-server` package. It contains the layout you should copy into
a new public repo at `github.com/ChaindocIO/claude-plugins` so Claude Code
users can install the Chaindoc plugin via:

```
/plugin marketplace add ChaindocIO/claude-plugins
/plugin install chaindoc@chaindoc
```

## How to set it up (one-time)

1. Create a new public repo: `github.com/ChaindocIO/claude-plugins`.
2. Copy the contents of this directory into the new repo:
   ```bash
   gh repo create ChaindocIO/claude-plugins --public --clone
   cd claude-plugins
   cp -r /path/to/mcp-server/marketplace/.claude-plugin .
   cp /path/to/mcp-server/marketplace/README.md .
   ```
3. Copy the actual plugin source from `mcp-server/chaindoc-claude/` into a
   `chaindoc/` directory in the new repo:
   ```bash
   cp -r /path/to/mcp-server/chaindoc-claude ./chaindoc
   ```
4. Verify the layout:
   ```
   claude-plugins/
   ├── .claude-plugin/
   │   └── marketplace.json
   ├── README.md
   └── chaindoc/
       ├── .claude-plugin/plugin.json
       ├── .mcp.json
       ├── README.md
       └── skills/
   ```
5. Commit and push:
   ```bash
   git add -A
   git commit -m "Initial Chaindoc plugin marketplace"
   git push -u origin main
   ```

## Releasing a new plugin version

When you bump `chaindoc-claude/.claude-plugin/plugin.json` in the mcp-server
repo, mirror it here:

1. Re-copy `chaindoc-claude/` → `claude-plugins/chaindoc/`.
2. Bump the matching `version` in `.claude-plugin/marketplace.json`.
3. Commit and push.

Users running `/plugin update chaindoc@chaindoc` will pick it up.

## Alternative: keep a single source of truth

If you'd rather not duplicate the plugin source between two repos, you have
two options:

- **Git submodule** — add `mcp-server` as a submodule under
  `claude-plugins/external/mcp-server`, then point `source` in
  `marketplace.json` at `./external/mcp-server/chaindoc-claude`.
- **Single-repo marketplace** — drop the dedicated marketplace repo entirely
  and add a `.claude-plugin/marketplace.json` to the root of `mcp-server`
  itself, with `source: "./chaindoc-claude"`. Users install via
  `/plugin marketplace add ChaindocIO/mcp-server`. Trade-off: every
  `mcp-server` commit looks like a marketplace publish to subscribed clients.

For most teams the duplicated-but-decoupled layout (this template) is the
cleanest because the marketplace repo doubles as a release ledger.
