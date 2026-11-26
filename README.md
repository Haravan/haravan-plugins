# Haravan Plugins - AI Agent Plugin

Connect your AI tools to your Haravan shop.

The plugin turns Haravan shop data into sales and profit reports, channel and product breakdowns, order lists, and low-stock and inventory summaries. It can also confirm, close, re-open or cancel orders and add or remove order tags; every change is shown on a confirmation card and runs only after you press Confirm. It does not create orders, issue refunds or edit products.

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

- **Haravan MCP server** (`https://mcp-plugins.haravan.app/mcp`): read tools `search_orders`, `get_order`, `analyze_sales`, `list_low_stock`, `get_inventory_balances`, `list_inventory_transfers`, `list_locations`, and order actions `preview_confirm_orders`, `preview_close_orders`, `preview_reopen_orders`, `preview_cancel_orders`, `preview_add_order_tags`, `preview_remove_order_tags` (each shows a confirmation card; the change is applied only from the card)
- **`haravan-api-workflow` skill**: guides the agent through sales and stock reports, order lists, stock transfers, order actions, pagination and timezones

## Repository layout

| Path | Used by |
| --- | --- |
| `plugin.json`, `mcp.json` | [Agent Plugins](https://agent-plugins.org) manifest (incl. OpenAI review metadata) |
| `.claude-plugin/` | Claude Code plugin + marketplace |
| `.codex-plugin/`, `.agents/plugins/` | OpenAI Codex plugin + marketplace |
| `.cursor-plugin/` | Cursor plugin + marketplace |
| `.mcp.json` | MCP config for Claude Code, Codex, Cursor, Copilot |
| `gemini-extension.json` | Gemini CLI extension |
| `skills/` | Agent skills shared by all clients |
| `assets/` | Logos |

When releasing, bump `version` in every manifest and add an entry to `CHANGELOG.md`.
