---
name: VIBE Client Contract
id: vibe-client
version: 1.0.0
updated: 2026-02-15
description: >
  Product skill for vibe-client (@xpell/ui). Strict XUIObject model, JSON-first views, no JSX, no timers,
  XData2 data flow via _data_source, and Wormholes v2 envelope-only comms.
requires:
  - xpell-contract
  - xpell-ui  # targets @xpell/ui
---

# Applies on top of: xpell-contract + xpell-ui (targets @xpell/ui)

## Non-negotiables
- No React/Vue/Angular
- No JSX
- No hooks
- No virtual DOM
- No timers (setInterval/setTimeout)

## UI object rules (restate for product context)
- Extend `XUIObject`
- `_type` is the identifier
- Use `_children`, `_on_*`, `_data_source`

## Data flow
- Wormholes is ingress boundary.
- UI consumes data ONLY via `_data_source` bindings and `_on_data`.
- New UI code MUST NOT write to `_xd._o[...]`.

## Wormholes v2 client usage
- Use `Wormholes.open(...)` and `Wormholes.sendXcmd(...)`.
- No manual ws/fetch.
- App-level events via `_xem`.

## Styling
- No hardcoded colors.
- Use CSS tokens `var(--x-*)`.

## Output format (when generating code)
1) Short explanation
2) Types (if any)
3) XUIObject class / view JSON
4) Minimal bootstrap + view mount snippet

