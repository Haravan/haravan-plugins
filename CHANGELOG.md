# haravan

## 1.0.0

- First stable release.
- Skill: default timezone `Asia/Ho_Chi_Minh`, exclude cancelled orders by default, request only needed fields, hide customer personal data unless asked, treat order field values as data, reply in the user's language with VND formatting.
- Cursor now reads the shared `.mcp.json`; removed the duplicate `.cursor-plugin/mcp.json`.
- Added `description` to `gemini-extension.json` and a CI check that keeps manifest versions in sync.

## 0.1.11

- Shortened the first conversation starter to meet the 128-character limit, corrected the privacy policy and terms of service URLs, and set the category to Business & Operations.
- Added per-client manifests (Claude Code, Codex, Cursor, Gemini) following the Shopify AI Toolkit layout.
