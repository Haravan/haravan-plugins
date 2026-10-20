# Haravan Plugins - AI Agent Plugin

Connect your AI tools to your Haravan shop.

The plugin turns Haravan order data into sales reports, weekly comparisons, daily trends, order lists, and payment or fulfillment summaries. It is read-only: it does not create or change orders.

## Install

- **For Claude Code**:

  ```
  claude plugin marketplace add Haravan/haravan-plugins
  claude plugin install haravan@haravan-plugins
  ```

- **For OpenAI Codex**:

  ```
  codex plugin marketplace add Haravan/haravan-plugins
  ```

- **For Cursor**: In Cursor Chat, add the plugin from `https://github.com/Haravan/haravan-plugins`.

- **For Gemini CLI**:

  ```
  gemini extensions install https://github.com/Haravan/haravan-plugins
  ```

- **For VS Code**: Enable the [Agent plugins](https://code.visualstudio.com/docs/copilot/customization/agent-plugins) preview, run `Chat: Install Plugin From Source`, and enter `https://github.com/Haravan/haravan-plugins`.

## What you get

- **Haravan MCP server** (`https://mcp-plugins.haravan.app/mcp`): discover, inspect, and call Haravan order APIs with GET requests
- **`haravan-api-workflow` skill**: guides the agent through order reports, pagination, timezones, and calculation basis

## Repository layout

| Path | Used by |
| --- | --- |
| `plugin.json`, `mcp.json` | [Agent Plugins](https://agent-plugins.org) manifest (incl. OpenAI review metadata) |
| `.claude-plugin/` | Claude Code plugin + marketplace |
| `.codex-plugin/`, `.agents/plugins/` | OpenAI Codex plugin + marketplace |
| `.cursor-plugin/` | Cursor plugin, marketplace, MCP config |
| `.mcp.json` | MCP config for Claude Code, Codex, Copilot |
| `gemini-extension.json` | Gemini CLI extension |
| `skills/` | Agent skills shared by all clients |
| `assets/` | Logos |

When releasing, bump `version` in every manifest and add an entry to `CHANGELOG.md`.
