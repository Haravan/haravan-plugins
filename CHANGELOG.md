# haravan

## 1.1.0

- Each order operation is now its own read-only tool: `search_orders`, `get_order` and `summarize_sales` replace the generic discover/inspect/execute API tools.
- The app now requests only order read permission.
- Updated the `haravan-api-workflow` skill, README and OpenAI review test cases to match the new tools.

## 1.0.0

- First stable release.
- Cursor now reads the shared `.mcp.json`; removed the duplicate `.cursor-plugin/mcp.json`.
- Added `description` to `gemini-extension.json` and a CI check that keeps manifest versions in sync.

## 0.1.11

- Shortened the first conversation starter to meet the 128-character limit, corrected the privacy policy and terms of service URLs, and set the category to Business & Operations.
- Added per-client manifests (Claude Code, Codex, Cursor, Gemini) following the Shopify AI Toolkit layout.
