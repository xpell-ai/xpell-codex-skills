---
name: Xpell UI Contract
id: xpell-ui
version: 1.0.0
updated: 2026-02-15
description: >
  Layer skill for @xpell/ui. Enforces XUIObject model, JSON-first views, event + handler conventions,
  XData2 binding via _data_source, no JSX/framework assumptions, no timers, and CSS token usage.
requires:
  - xpell-contract
---

# Applies on top of: xpell-contract

This skill adds the **UI runtime** constraints for `@xpell/ui` and UI-heavy apps.

# Identity
- Xpell UI is NOT React/Vue/Angular.
- Declarative JSON + runtime objects.
- DOM is output only.

# Object model (mandatory)
## Package naming
- Runtime package: `@xpell/ui`
- Skill id: `xpell-ui`

All UI components MUST:
- extend `XUIObject`
- accept **one** `data` object in constructor
- call `super(data, defaults, true)`
- call `this.parse(data)`
- define `static _xtype`
- use `_type` as the only public identifier

# Naming & properties (critical)
- Any non-HTML property MUST start with `_`.
- Allowed HTML passthrough only when intended: `style`, `src`, `href`, `title`, `id`, and `class` (project exception).
- Forbidden: `onClick`, `onChange`, DOM `addEventListener`, React/Vue conventions.

# Event system
- Global events via `_xem` only:
  - `_xem.on("event", handler)`
  - `_xem.fire("event", payload)`
- Object events via `_on_*` or `_on` map only.
- DB-stored handlers must follow nano-commands v2 (data-only).

# Data flow (XData2)
- `_xd` is the only shared runtime state.
- Prefer binding via `_data_source` + `_on_data` handlers.
- No polling; no timers.
- Legacy `_xd._o[...]` is compat only; new code must not write to `_o`.

# No timers
- ❌ `setInterval`, `setTimeout`
- ✅ `_on_frame`, `_on_data`

# Styling
- No hardcoded colors.
- Use CSS tokens: `var(--x-*)`

# Wormholes v2 (web client) — strict
- Canonical import: `import { Wormholes } from "@xpell/ui";`
- Envelope-only protocol; no manual fetch/ws.
- UI consumes ingress data via `_data_source`.

# Output checklist for code generation
When asked to generate UI code, output:
1) Short explanation
2) Types (if any)
3) Class implementation (extends `XUIObject`)
4) Minimal usage example (view JSON + how it's added/mounted)
