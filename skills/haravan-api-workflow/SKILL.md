---
name: haravan-api-workflow
description: Use when an authorized merchant asks for sales or profit reports, weekly comparisons, daily trends, channel or product breakdowns, order lists, order details, low stock, inventory levels, stock transfers, branches or staff from a connected Haravan shop, or wants to confirm, close, re-open, tag, assign or add notes to orders. Read data with search_orders, get_order, analyze_sales, list_low_stock, get_inventory_balances, list_inventory_transfers, list_locations and list_staff; change orders only through the preview_* order action tools.
---

# Read sales, orders and stock and act on orders in a connected Haravan shop

The only changes this release can make are confirming, closing and re-opening
orders, adding or removing order tags, assigning the staff member who handles
orders and adding to an order's note, each after the user confirms on a card.
For requests to create or cancel orders, refund, edit products, adjust
stock or delete shop data, explain the limitation without calling tools.
Customer and discount records are outside this release's scope. Follow the
user's requested period and report; do not retrieve unrelated data.

## Tools

- `analyze_sales`: sales figures from Haravan Reports for a date range (`date_min`, `date_max`). Pick `metrics` (for example `NetSaleAmount`, `TotalSaleAmount`, `Profit`, `Quantity`, `ReturnsAmount`) and up to 3 `dimensions` (for example `Day`, `Month`, `SourceName`, `ProductId`, `SKU`, `Location`, `ShippingProvince`, `PaymentStatus`). Without dimensions it returns one total row. Cancelled orders are excluded unless `include_cancelled` is true.
- `list_low_stock`: variants per branch with quantity sold per day, on hand, available and estimated days until stockout, soonest stockout first.
- `get_inventory_balances`: stock on hand, committed, available, incoming and cost, grouped by product, variant, branch, type, vendor or category.
- `list_inventory_transfers`: stock transfers between branches with source and destination branch ids, status, reason, dates and line items; optional `from_location_id` or `to_location_id`. Page 1 without branch filters returns `total`.
- `list_locations`: branches and warehouses with id, name, address and which is primary. Use it to turn a branch name into an id and to show names instead of ids.
- `list_staff`: staff accounts with id, name, owner flag and permissions (no email or phone). Use it to turn a staff name into the `user_id` for `preview_assign_orders`. Haravan offers it only for Haravan Plus shops.
- `search_orders`: list orders filtered by creation or update time, order status, payment status (`financial_status`) or shipping status (`fulfillment_status`). Page 1 returns `total`.
- `get_order`: full details of one order by its API `id` from a search result (not the `#1001` order number).
- `get_connection_status`: which shop is connected and whether it must be reconnected.
- `preview_confirm_orders`, `preview_close_orders`, `preview_reopen_orders`, `preview_add_order_tags`, `preview_remove_order_tags` (need `tags`, comma-separated), `preview_assign_orders` (needs `user_id` from `list_staff`, optional `location_id` from `list_locations`; replaces the current assignee and works only for shops with advanced order processing), `preview_add_order_note` (needs `note`, at most 1000 characters, appended on a new line after the existing note): take 1–20 `order_ids` and only show a confirmation card. Nothing changes until the user presses Confirm on the card, which applies the change itself.

## Workflow

1. If a tool reports an authentication or permission problem, call `get_connection_status` and explain that the user must reconnect. Do not ask for or handle access tokens.
2. Resolve the requested period. Dates without an offset are read as Vietnam time (+07:00); a date alone as an upper bound includes that whole day. If the user means another time zone, pass ISO 8601 date-times with that offset and say so.
3. For totals, trends, top products, channel, branch or payment breakdowns, call `analyze_sales` with the matching metrics and dimensions. For comparisons, call it once per period with the same metrics.
4. For stock questions, call `list_low_stock` (what is running out), `get_inventory_balances` (how much is in stock) or `list_inventory_transfers` (stock moved between branches). When the user names a branch, call `list_locations` first to get its id.
5. For order lists, call `search_orders` with the matching filters. Call `get_order` only when the user needs one order's items, addresses, payments, fulfillments or refunds.
6. Request the next page while `has_more` is true before claiming a complete list or ranking; otherwise say the result is partial.
7. State the date range, time zone and the metric names used.
8. To change orders, find their API ids with `search_orders`, then call the matching `preview_*` tool once with every order the user named. When the user names a staff member or branch, look up the id with `list_staff` or `list_locations` first. Do not say the change is done until the card reports Confirm; if the user asks again without a card result, check with `get_order`.

## Stop conditions

- Haravan cannot search orders by order number, customer name or phone, the stock report tools cannot filter by product, SKU or branch, and transfers can be filtered only by source or destination branch. Say so and offer a grouped report or time-range search instead.
- If a tool returns `throttled`, stop calling tools and answer with the data already collected.
- If a tool rejects a request, explain the limitation and do not work around it.
