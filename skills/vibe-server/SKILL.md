---
name: VIBE Server Contract
id: vibe-server
version: 1.0.0
updated: 2026-02-15
description: >
  Product skill for vibe-server (xnode). Stage 1: deterministic backend for users/products/conversations/messages.
  Strict transport boundary, no external channel APIs yet, no background loops, explicit persistence, structured errors.
requires:
  - xpell-contract
  - xpell-node
---

# Applies on top of: xpell-contract + xpell-node

## Stage 1 scope (explicit)
- Domain: users, products (food/drinks), conversations + messages, channel identities (web now; telegram/whatsapp later).
- Constraints:
  - No ordering
  - No payments
  - No external channel APIs yet (telegram/whatsapp are stubs)
  - No autonomous background behavior
  - No inferred state

## Single source of truth
All externally-visible behavior must flow through `_x.execute(xcmd)`.

## Entities (minimum)
- `users`
- `user_identities` (web/telegram/whatsapp)
- `products` (optional: `product_categories`)
- `conversations`
- `messages` (direction + delivery status)
- `outbox` (recommended for later integrations)

## Persistence pattern (required)
- Repository + Codec boundary per entity:
  - `*_repo.ts` storage adapter CRUD
  - `*_codec.ts` validation + serialization
- Module ops call repos; repos never call modules.
- No persistence work in constructors.

## Wormholes & security
- Wormholes v2 envelope-only.
- Gateway injects server context (`_ctx`) and overwrites any client-provided `_ctx`.
- Transport is untrusted; validate envelope + session + xcmd shape.

## Validation
- Params are `_snake_case` only.
- Centralize enums/constants for channels/status/direction; no scattered strings.

## Error format (mandatory)
Return:
```json
{
  "_ok": false,
  "_error": {
    "_code": "E_SOMETHING",
    "_message": "Human readable",
    "_details": { }
  }
}
```

## Logging
- Use `_xlog`; no `console.log`.
- Never log secrets.

## Output format (when generating code)
1) Short explanation
2) DTO types/interfaces (snake_case)
3) XModule implementation (`_op_*`)
4) Minimal client REQ → server op → RES example

