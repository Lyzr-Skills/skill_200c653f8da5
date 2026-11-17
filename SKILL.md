---
name: florida-material-risk
description: Use for PMGT - Inventory Planning V6.0 material-shortage questions (especially Florida), impacted finished goods using the supplied HPE BOM PDF, transfer donors and committed PO pull-forward. Use Scenario 1 when no scenario is specified. Route to only the needed workflow; protect first-question response time with a bounded read and an honest coverage limit.
---

# Materials planning: bounded-response router

Use only the named Anaplan model and supplied HPE BOM PDF. Read only; never write cells or run imports/processes. Never invent stock, suppliers, dates, lead times, substitutes, orders, or completeness. If a required source cannot be read, stop and explain.

## Route one question to one workflow
- Material shortfalls / Florida next 26 weeks -> `references/shortfalls.md` (read this file).
- Finished goods / BOM -> `references/finished-goods.md`.
- Donor / largest excess / transfer -> `references/transfers.md`.
- Open/committed PO pull-forward -> `references/purchase-orders.md`.
- Comprehensive report -> run these workflows in order, passing verified findings without rereading unchanged measures.

## Common rules
- **Default Scenario 1** when the user omits a scenario; do not ask which scenario. A specifically requested scenario overrides it. Scenario 1 can be a list axis rather than native Versions. Pin all native Versions to one version if present; do not treat scenario and Versions as interchangeable.
- Carry forward an explicit site, scenario and horizon in conversation. On a new relative-time request, use `list_time_anchors` then `set_time_anchor`; if candidates conflict, ask which model period to use. Use the model calendar, not real-world date arithmetic, to identify 26 consecutive weeks. If anchor already valid for the same bound model and no refresh requested, reuse it. Model FY labels need not align with today's date.
- `Shortfall (U)` means unmet demand, not negative inventory. `Excess Available for Push and Pull (U)` is modeled excess, not released transfer stock. Verify the actual line-item format and dimensions before quoting quantities. Do not sum recurring weekly shortages as if they are new demand.
- On a new request, check existing bound model and bind PMGT - Inventory Planning V6.0 only if needed. Reuse verified schema within a session. Discover narrowly; do not catalog the whole model.
- Protect interactive response time: **at most one broad inventory read** for a new shortfall query. If it times out, do not repeat it or launch a 26-week fallback loop. Use at most one genuinely smaller diagnostic read if useful, then tell the user exactly what was and was not checked. A partial result is not an exhaustive report. Never mask a failed read as no shortage.
- Follow-ups may use earlier figures only if labelled prior-read and dated; refresh numerical cells when an up-to-date answer is requested. Give site, scenario, model horizon, data-read date, actual coverage and any mismatch with wall-clock date.
- Risk ratings: Critical within 4 model weeks or confirmed unbuildable; High in weeks 5–8; Medium thereafter or safety-stock-only. Exact BOM identifiers only. A material shortfall alone cannot establish unbuildable finished-good units. All action quantities/dates are conditional unless availability, demand and transit/lead times are verified.
