# haravan

## 1.1.4

- New order actions: `preview_confirm_orders`, `preview_close_orders`, `preview_reopen_orders`, `preview_add_order_tags`, `preview_remove_order_tags`, `preview_assign_orders`, `preview_add_order_note`, up to 20 orders per call. Cancelling orders is not offered.
- Each action shows a confirmation card; the change is applied only when the user presses Confirm on the card, never by the model. Adding a note appends to the existing note instead of replacing it.
- New read tools `list_inventory_transfers` (stock transfers between branches, filterable by source or destination branch) `list_locations` (branches and warehouses, to map names to ids) and `list_staff` (staff accounts for assigning orders; Haravan Plus shops only).
- The app also requests `com.write_orders`, `com.read_inventories` and `com.read_shop`; existing connections must reconnect.
- Updated the `haravan-api-workflow` skill, README, manifest descriptions and OpenAI review test cases.

## 1.1.3

- Sales figures now come from Haravan Reports: `analyze_sales` (net sales, profit, quantity, returns, grouped by up to 3 dimensions) replaces `summarize_sales`.
- New `list_low_stock` and `get_inventory_balances` tools for products about to run out and stock on hand.
- The app also requests `report_api`; existing connections must reconnect.
- Updated the `haravan-api-workflow` skill, README, manifest descriptions and OpenAI review test cases.

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
