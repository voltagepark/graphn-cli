# GraphN CLI

Command-line interface for [GraphN](https://graphn.ai) — build, test, and deploy AI workflows from your terminal.

GraphN is a platform for composing AI workflows from **agents**, **functions**, **MCP tool servers**, and **knowledge bases**. The CLI lets you manage all of these without leaving your editor.

## Install

### Homebrew (macOS / Linux)

```bash
brew tap voltagepark/graphn
brew install graphn
```

### Direct download

Download the binary for your platform:

**macOS (Apple Silicon):**

```bash
curl -sSL https://github.com/voltagepark/graphn-cli/releases/latest/download/graphn-darwin-arm64 -o graphn && sudo install graphn /usr/local/bin/graphn && rm graphn
```

**macOS (Intel):**

```bash
curl -sSL https://github.com/voltagepark/graphn-cli/releases/latest/download/graphn-darwin-amd64 -o graphn && sudo install graphn /usr/local/bin/graphn && rm graphn
```

**Linux (x86_64):**

```bash
curl -sSL https://github.com/voltagepark/graphn-cli/releases/latest/download/graphn-linux-amd64 -o graphn && sudo install graphn /usr/local/bin/graphn && rm graphn
```

**Linux (ARM64):**

```bash
curl -sSL https://github.com/voltagepark/graphn-cli/releases/latest/download/graphn-linux-arm64 -o graphn && sudo install graphn /usr/local/bin/graphn && rm graphn
```

### Verify

```bash
graphn version
```

### Update

```bash
graphn update
```

## Quick start

1. **Sign up** at [graphn.ai](https://graphn.ai) and create an API key in your workspace settings.

2. **Configure the CLI:**

   ```bash
   graphn init
   ```

   This walks you through API URL, API key, and workspace selection. It also generates project files for Cursor and Claude Code (see [IDE integration](#ide-integration) below).

   To configure the CLI only (no project files):

   ```bash
   graphn init --setup-only
   ```

3. **Verify connectivity:**

   ```bash
   graphn whoami
   ```

4. **Explore your workspace:**

   ```bash
   graphn status          # resource counts dashboard
   graphn context         # full workspace state as JSON
   ```

5. **Deploy a blueprint:**

   ```bash
   graphn blueprint list
   graphn blueprint deploy rag-research --name "My Research Bot"
   ```

6. **Test and publish:**

   ```bash
   graphn agent publish-all && graphn func publish-all && graphn mcp publish-all
   graphn workflow dry-run <workflow-id> --input '{"query": "test"}'
   graphn workflow publish <workflow-id> -m "Initial version"
   ```

## Commands

| Command | Alias | Description |
|---------|-------|-------------|
| `graphn agent` | | Manage agents |
| `graphn function` | `func` | Manage functions |
| `graphn mcp-server` | `mcp` | Manage MCP tool servers |
| `graphn workflow` | `wf` | Manage workflows |
| `graphn knowledgebase` | `kb` | Manage knowledge bases (RAG) |
| `graphn blueprint` | `bp` | Browse and deploy workflow templates |
| `graphn model` | | Manage models (built-in, imported, custom) |
| `graphn secret` | | Manage secrets |
| `graphn storage` | | Manage S3-compatible storage |
| `graphn execution` | `exec` | View workflow executions and results |
| `graphn logs` | | Pretty-print execution traces |
| `graphn watch` | | Live tail of workflow executions |
| `graphn chat` | | Interactive chat with agents, workflows, or models |
| `graphn scaffold` | | Generate boilerplate for a component |
| `graphn workspace` | `ws` | Manage workspaces |
| `graphn organization` | `org` | Manage organizations |
| `graphn api-key` | | Manage workspace API keys |
| `graphn config` | | Manage CLI configuration |
| `graphn docs` | | Built-in documentation and skill guides |
| `graphn telemetry` | | Manage anonymous telemetry |

Run `graphn <command> --help` for subcommands and flags. Use `-f text` for human-readable tables and `-v` for HTTP traces.

## Built-in documentation

The CLI ships focused guides you can read without a browser:

```bash
graphn docs skills                    # list available skill guides
graphn docs skills workflow-building  # 9-step workflow formula
graphn docs skills rag-pipeline       # RAG workflow patterns
graphn docs skills debugging          # troubleshooting failed workflows
graphn docs skills dsl-reference      # full DSL schema and API reference
```

## IDE integration

The CLI integrates with **Cursor** and **Claude Code** via [Model Context Protocol (MCP)](https://modelcontextprotocol.io), exposing all commands as structured tools so your AI assistant can manage workflows directly.

### Setup

Running `graphn init` generates project rule files for your IDE:

| File | Purpose |
|------|---------|
| `.cursor/rules/graphn.md` | Cursor rules for GraphN commands and patterns |
| `CLAUDE.md` | Instructions for Claude Code (appended if file exists) |

To connect the MCP server, add to your IDE config:

**Cursor** (`.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "graphn": {
      "command": "graphn",
      "args": ["mcp-serve"]
    }
  }
}
```

**Claude Code** (`~/.claude/settings.json`):

```json
{
  "mcpServers": {
    "graphn": {
      "command": "graphn",
      "args": ["mcp-serve"]
    }
  }
}
```

## Configuration

CLI config is stored in `~/.graphn/config.json`. You can also use environment variables:

| Variable | Purpose |
|----------|---------|
| `GRAPHN_API_KEY` | API key |
| `GRAPHN_URL` | API base URL |
| `GRAPHN_WORKSPACE` | Workspace ID |
| `GRAPHN_GATEWAY_URL` | Gateway URL (required for `graphn workflow run`) |

```bash
graphn config show           # view current config
graphn config set-url <url>  # set API base URL
```

## Telemetry

The CLI collects anonymous telemetry to help diagnose crashes and improve performance. Telemetry is **on by default**.

**What is collected:** command names, error messages, CLI arguments, workspace ID, org ID, API URL, request bodies (for debugging failed API calls), CLI version, OS, architecture, and an anonymous device ID (random UUID, not tied to your account).

**What is never sent:** API keys and bearer tokens are always redacted before any data leaves your machine.

**Sampling rates:**

| Event type | Sample rate |
|------------|-------------|
| Errors (crashes and failed commands) | 100% |
| Performance traces (successful commands) | 20% |

Errors are also written to a local crash log at `~/.graphn/crash.log` regardless of telemetry settings. The log rotates at 1 MB.

**Opt out:**

```bash
graphn telemetry disable                       # disable permanently
GRAPHN_TELEMETRY_DISABLED=1 graphn <command>   # disable for a single session
graphn telemetry enable                        # re-enable
graphn telemetry status                        # check current state
```

You are also prompted to opt out during `graphn init`.

## Documentation

- [Getting started](https://graphn.ai/docs/getting-started)
- [Core concepts](https://graphn.ai/docs/concepts)
- [Developing with agents (CLI & IDE)](https://graphn.ai/docs/developing-with-agents)
- [Workflow DSL reference](https://graphn.ai/docs/dsl-reference)
- [Function SDK reference](https://graphn.ai/docs/sdk-reference)
- [API reference](https://graphn.ai/docs/api-reference)

## License

[Apache-2.0](LICENSE)
