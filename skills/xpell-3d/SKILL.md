---
name: Xpell 3D Contract
id: xpell-3d
version: 1.0.0
updated: 2026-02-15
description: >
  Layer skill for @xpell/3d. Enforces encapsulation of engine objects, explicit mutation APIs,
  nano-command safety, and deterministic runtime behavior for 3D scenes.
requires:
  - xpell-contract
---

# Applies on top of: xpell-contract

This skill adds the **3D runtime** constraints for `@xpell/3d`.

# Identity
- `@xpell/3d` is a runtime module built on top of `@xpell/core`.
- It must remain deterministic and engine-driven (no framework assumptions).

# Encapsulation (non-negotiable)
- Do NOT expose mutable THREE.js / physics engine objects as public runtime fields.
- No public direct access to internal `_mesh`, `_body`, etc. if mutation would bypass guards.
- All mutation must be performed via explicit, auditable APIs.

# Canonical mutation APIs
- Provide/Use methods like:
  - `setPosition(x,y,z)` / `setRotation(x,y,z)` / `setScale(x,y,z)`
  - helper methods must delegate to canonical setters (single source of truth)
- If a nano-command changes state, it must call these APIs (not mutate fields directly).

# Nano-commands v2 compatibility
- DB-stored handlers are data-only (no functions).
- Unknown ops are rejected by validation boundaries.
- Sequence handlers are structural arrays (no scripting).

# XData2 usage
- Follow XData2 strict rules from foundation:
  - no shadow state
  - no polling/timers
  - stable key namespaces

# Performance & frame updates
- Prefer `_on_frame` hooks over timers.
- Avoid per-frame heavy work unless explicitly required.
- Keep per-frame keys intentional and namespaced (e.g., `engine:frame`, `audio:rms`).

# Output checklist for code generation
When asked to generate 3D code, output:
1) Short explanation
2) Types/interfaces
3) Class/module implementation
4) Minimal scene usage example + how to bind data (XData2 / nano-commands)

