---
name: haravan-api-workflow
description: Use when an authorized merchant asks for order lists, order details, sales reports, weekly comparisons, daily trends, or payment and fulfillment summaries from a connected Haravan shop. Read order data with the search_orders, get_order and summarize_sales tools.
---

# Read orders from a connected Haravan shop

This release supports read-only order reports. For requests to create, edit,
cancel, refund, tag, or delete shop data, explain the limitation without calling
tools. Requests for customer, product, stock, or discount records are outside
this release's scope. Follow the user's requested period and report; do not
retrieve unrelated data.

## Tools

- `search_orders`: list orders filtered by creation or update time, order status, payment status (`financial_status`) or shipping status (`fulfillment_status`). Page 1 returns `total`.
- `get_order`: full details of one order by its API `id` from a search result (not the `#1001` order number).
- `summarize_sales`: gross order value, order count, average order value, discounts, and breakdowns by day, payment status, sales channel and top products for a date range.
- `get_connection_status`: which shop is connected and whether it must be reconnected.

## Workflow

1. If a tool reports an authentication or permission problem, call `get_connection_status` and explain that the user must reconnect. Do not ask for or handle access tokens.
2. Resolve the requested period. Dates without an offset are read as Vietnam time (+07:00); a date alone as an upper bound includes that whole day. If the user means another time zone, pass ISO 8601 date-times with that offset and say so.
3. For totals, trends, channel or payment breakdowns, call `summarize_sales`. For comparisons, call it once per period. If `complete` is false, split the range into smaller ranges and add the results.
4. For order lists, call `search_orders` with the matching filters. Request the next page while `has_more` is true before claiming a complete list; otherwise state that the list is partial and give `total`.
5. Call `get_order` only when the user needs one order's items, addresses, payments, fulfillments or refunds.
6. State the date range, time zone and calculation basis. Call summed order totals gross order value, not net revenue; cancelled orders are excluded and refunds are not subtracted.

## Stop conditions

- Haravan cannot search orders by order number, customer name or phone. Say so and offer a time-range search instead.
- If a tool returns `throttled`, stop calling tools and answer with the data already collected.
- If a tool rejects a request, explain the limitation and do not work around it.
