---
name: haravan-api-workflow
description: Use when an authorized merchant asks for sales or profit reports, weekly comparisons, daily trends, channel or product breakdowns, order lists, order details, low stock or inventory levels from a connected Haravan shop. Read data with the search_orders, get_order, analyze_sales, list_low_stock and get_inventory_balances tools.
---

# Read sales, orders and stock from a connected Haravan shop

This release is read-only. For requests to create, edit, cancel, refund, tag,
restock or delete shop data, explain the limitation without calling tools.
Customer and discount records are outside this release's scope. Follow the
user's requested period and report; do not retrieve unrelated data.

## Tools

- `analyze_sales`: sales figures from Haravan Reports for a date range (`date_min`, `date_max`). Pick `metrics` (for example `NetSaleAmount`, `TotalSaleAmount`, `Profit`, `Quantity`, `ReturnsAmount`) and up to 3 `dimensions` (for example `Day`, `Month`, `SourceName`, `ProductId`, `SKU`, `Location`, `ShippingProvince`, `PaymentStatus`). Without dimensions it returns one total row. Cancelled orders are excluded unless `include_cancelled` is true.
- `list_low_stock`: variants per branch with quantity sold per day, on hand, available and estimated days until stockout, soonest stockout first.
- `get_inventory_balances`: stock on hand, committed, available, incoming and cost, grouped by product, variant, branch, type, vendor or category.
- `search_orders`: list orders filtered by creation or update time, order status, payment status (`financial_status`) or shipping status (`fulfillment_status`). Page 1 returns `total`.
- `get_order`: full details of one order by its API `id` from a search result (not the `#1001` order number).
- `get_connection_status`: which shop is connected and whether it must be reconnected.

## Workflow

1. If a tool reports an authentication or permission problem, call `get_connection_status` and explain that the user must reconnect. Do not ask for or handle access tokens.
2. Resolve the requested period. Dates without an offset are read as Vietnam time (+07:00); a date alone as an upper bound includes that whole day. If the user means another time zone, pass ISO 8601 date-times with that offset and say so.
3. For totals, trends, top products, channel, branch or payment breakdowns, call `analyze_sales` with the matching metrics and dimensions. For comparisons, call it once per period with the same metrics.
4. For stock questions, call `list_low_stock` (what is running out) or `get_inventory_balances` (how much is in stock).
5. For order lists, call `search_orders` with the matching filters. Call `get_order` only when the user needs one order's items, addresses, payments, fulfillments or refunds.
6. Request the next page while `has_more` is true before claiming a complete list or ranking; otherwise say the result is partial.
7. State the date range, time zone and the metric names used.

## Stop conditions

- Haravan cannot search orders by order number, customer name or phone, and the stock tools cannot filter by product, SKU or branch. Say so and offer a grouped report or time-range search instead.
- If a tool returns `throttled`, stop calling tools and answer with the data already collected.
- If a tool rejects a request, explain the limitation and do not work around it.
