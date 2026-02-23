---
name: Server XVM Contract
id: server-xvm
version: 1.0.0
updated: 2026-02-15
description: >
  Feature skill for server-side XVM (persisted apps/views). Storage layout, subscribe model, push-update event contract,
  and validation policy for persisted JSON (no functions, nano-command allowlist, safe navigate/open-url).
requires:
  - xpell-contract
  - xpell-node
---

# Applies on top of: xpell-contract + xpell-node

## Storage layout
- `work/xvm/apps/<env>/<app_id>/app.json`
- `work/xvm/apps/<env>/<app_id>/views/*.json`

## app.json shape
```json
{
  "_app_id": "vibe",
  "_env": "default",
  "_meta": {
    "_name": "VIBE",
    "_version": 1,
    "_entry_view_id": "home"
  },
  "_config": {}
}
```

## Transport context contract
Inbound `REQ` commands are executed with server-injected context:
- `xcmd._ctx._wid` (required)
- `xcmd._ctx._sid` (optional)
`_ctx` is overwritten by server and never trusted from client payload.

## Ops
- `server-xvm:list-apps`
- `server-xvm:get-app`
- `server-xvm:get-view`
- `server-xvm:subscribe`
- `server-xvm:unsubscribe`
- `server-xvm:push-update`

## Push-update contract
On `push-update`:
1. Persist validated view
2. Increment `app._meta._version`
3. Fire internal event: `server-xvm:update`
4. Send Wormholes EVT to subscribers only:
   - `_name: "server-xvm:update"`
   - args: `[ { _app_id, _env, _view_id, _version, _source, _view } ]`

## Validation policy (mandatory)
- No functions anywhere in persisted JSON.
- Allowed handler forms:
  - string nano-command
  - JSON command (`{ _op, _params }`)
  - sequence array
- Unknown nano ops are rejected.
- `navigate` must stay in same app scope.
- `open-url` is disabled by default; only allowed if:
  - `_allow_open_url: true` and
  - same-origin relative paths starting with `/`, or
  - `https://` URLs with host in allowlist.

