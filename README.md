# dsh-vercel-agent-browser

Connect [Vercel agent-browser](https://www.npmjs.com/package/agent-browser) to [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) (`dsh`) via MCP.

Once installed into a dsh profile, the agent gains the agent-browser MCP toolset (core + mobile browser automation tools) with zero manual setup.

## Install

```sh
dsh plugin --profile <name> add dsh-vercel-agent-browser
```

Or add it to your profile's `package.json`:

```json
{
  "dsh": {
    "profile": {
      "bundles": [
        "@deepseek-ai/dsh-base",
        "@deepseek-ai/dsh-web-app",
        "dsh-vercel-agent-browser"
      ]
    }
  },
  "dependencies": {
    "dsh-vercel-agent-browser": "^0.1.0"
  }
}
```

Then restart dsh.

## What it does

The bundle inserts one loader entry (`@deepseek-ai/dsh-mcp-client`) that exposes the browser MCP server:

- **Server**: `npx -y agent-browser@0.42.0 mcp --tools core,mobile`
- **Socket dir**: `/tmp/dsh-agent-browser` (isolated from other agent-browser users)
- **Tool call timeout**: 120 s
- **Startup failure is non-fatal** — dsh still boots if the MCP server cannot start.

The `agent-browser` version is pinned on purpose: it is the version audited before this plugin was published. Upgrading means changing the pin and bumping this plugin's version.

The first start downloads `agent-browser` via `npx` (30–60 s depending on network); after that it is cached and starts in about 1 s.

## Requirements

- dsh `^0.2.2-rc.1`
- Node.js `>= 22` (used by `npx`)

## Uninstall

```sh
dsh plugin --profile <name> remove dsh-vercel-agent-browser
```

## License

ISC
