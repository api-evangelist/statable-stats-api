---
name: Create goals and funnels and run a funnel report
description: Turn events, pages or scroll depth into goals, chain them into a funnel, and run the funnel report.
api: openapi/statable-stats-api-openapi.yml
operations: [createGoal, listSiteGoals, createFunnel, listFunnels, runFunnelReport]
---

# Goals and funnels

Requires the **Manage sites** permission (`sites:write` scope over OAuth).

1. **Create a goal** — `createGoal` (`POST /sites/{id}/goals`) from an event, page or scroll depth. Duplicate name → `409 goal_exists`.
2. **List goals** — `listSiteGoals` (`GET /sites/{id}/goals`).
3. **Create a funnel** — `createFunnel` (`POST /sites/{id}/funnels`) with 2–8 steps drawn from pages, events and goals. Duplicate name → `409 funnel_exists`.
4. **List funnels** — `listFunnels` (`GET /funnels`).
5. **Run the report** — `runFunnelReport` (`POST /funnels/{id}/report`) for step-by-step conversion and drop-off.

A Member cannot write (`403 not_site_owner`); `403 tracking_inactive` means the site stopped collecting. Branch on `code`.
