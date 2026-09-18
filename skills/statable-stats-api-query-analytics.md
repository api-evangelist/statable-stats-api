---
name: Query a site's analytics
description: Discover the sites a key can read, then run breakdowns, realtime counts and custom-property queries against the Stats API.
api: openapi/statable-stats-api-openapi.yml
operations: [listSites, query, currentVisitors, listPropKeys]
---

# Query analytics

1. **List sites** — `listSites` (`GET /sites`) turns a key into the `site_id` values every other call needs (required on an all-sites key, optional on a single-site key). Add `date_range` (`7d`, `30d`, `month`, `realtime`, `Nd`) to attach a `stats` block.
2. **Run a query** — `query` (`POST /query`) with `metrics`, optional `dimensions` (one at most — more → `400 too_many_dimensions`), `filters`, `date_range`, and `compare`. Breakdown queries accept `limit`/`offset`; sending them on an aggregate or time series → `400 limit_offset_misuse`. `metric_not_available` means the metric is real but the paired dimension cannot compute it.
3. **Live count** — `currentVisitors` (`GET /current-visitors`).
4. **Discover custom properties** — `listPropKeys` (`GET /props`) before filtering on `event:props:<key>` (which also needs an `event` filter, else `400 event_filter_required`).

Watch `X-RateLimit-Remaining` and slow down before the `429`; ask for more per request rather than polling. See rate-limits/.
