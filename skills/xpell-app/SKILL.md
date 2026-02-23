# SKILL: xpell-app

Generate a complete **Xpell 2 Alpha** application skeleton (client + server, optional 3D), using **AI-native runtime contracts**.

This skill is an **umbrella/composition** skill that pulls together the existing Xpell skills and produces an end-to-end app structure that Codex can extend safely.

---

## Requires

- xpell-contract
- xpell-ui
- xpell-node
- server-xvm
- xpell-3d (optional; only include when the user asks for 3D)
- vibe-client (optional; only include when working inside the VIBE client repo)
- vibe-server (optional; only include when working inside the VIBE server repo)

> If you are not sure whether 3D or VIBE is needed, do NOT include them.  
> Default to a 2D UI app with xnode + wormholes.

---

## When to use this skill

Use **xpell-app** when the task is one of:

- “Create a new Xpell 2 app”
- “Generate a full app skeleton”
- “Create client + server + wormholes”
- “Create an example app for xpell.ai”
- “Build a CRUD dashboard app (runtime-driven UI)”
- “Create XVMApp app shell + server module ops”
- “Refactor a repo into a clean Xpell 2 app structure”

Do NOT use this skill for small edits inside one module. Use the specific layer skill instead.

---

## Non-negotiable contracts (must follow)

1) **Do not infer missing state.** If files/modules do not exist in repo, create them only when the task explicitly requires it.

2) **XModule is the only behavior extension point** on the server runtime:
- No ad-hoc global functions as “server logic”.
- No cross-module mutation except via commands/events or documented XData usage.

3) **XObject is UI-free.**  
Core runtime objects must not assume UI behavior. UI logic lives only in XUI/XUIObject in `@xpell/ui`.

4) **Single source of truth for state**
- Use XData 2 / documented runtime state keys.
- Do not mirror state in multiple places “for convenience”.

5) **Serializable UI + logic**
- Prefer Nano-Commands v2 (JSON handlers + sequences) over JS functions in views.
- Views should be storable and patchable as pure data.

6) **Wormholes contract**
- Client/server interactions must use Wormholes and structured commands (XCommand-style envelope).
- Do not create “custom API endpoints” unless the task explicitly requests REST.

7) **No frameworks in the generated app**
- Do not introduce React/Vue/Next/Vite/Jekyll/etc.
- If a build tool already exists in the repo, follow it; otherwise keep it minimal.

---

## Output: what this skill generates

When asked to “generate a full app”, produce:

### A) Client (Web UI)
- A minimal **XVMApp** application manifest:
  - App shell layout
  - Containers/regions
  - Routes
  - Initial navigation
- 2–3 views (as structured JSON) demonstrating:
  - List screen
  - Details/Edit screen
  - Create screen (optional)
- A minimal client bootstrap entry that:
  - starts runtime
  - mounts player/root
  - loads the app manifest
  - connects Wormholes (if server enabled)

### B) Server (xnode)
- A server entry that:
  - boots xnode runtime
  - registers one XModule
  - exposes a few ops (list/create/update)
- A single “Example” module (e.g. `ItemsModule`) with:
  - deterministic ops
  - explicit params validation (basic)
  - no hidden cross-module mutations

### C) Shared contracts
- A shared “command envelope” definition and naming conventions:
  - module name
  - op names
  - params schema shape
- A minimal “app config” (json or env) that contains:
  - wormholes url
  - app name
  - feature flags (3d enabled / server enabled)

### D) Docs
- README that explains:
  - how to run client
  - how to run server
  - how wormholes connects them
  - how to extend with new modules/views

---

## Repository layout (default)

If the repo does not already dictate structure, default to:

- /client
  - /src
    - main entry
    - app manifest (XVMApp)
    - views (pure JSON)
    - wormholes client wiring
- /server
  - /src
    - main entry
    - modules/
      - ExampleModule (XModule)
    - wormholes server wiring
- /shared (optional)
  - command envelope shape
  - shared constants (module/op names)

If you are inside an existing monorepo, adapt to its conventions. Do not fight the repo.

---

## “Full App Example” default scenario (if user doesn’t specify)

Default example app: **Items Dashboard**
- Items list
- Item details/edit
- Create item
- Server stores data in-memory initially (unless XDB is already wired)
- One real-time event example (optional):
  - server emits “items-updated”
  - client refreshes list

Do NOT claim persistence unless it’s implemented.

---

## Acceptance criteria (must pass)

- App runs with minimal steps.
- Client can call at least one server op via Wormholes.
- Views are stored as pure JSON (no JS functions) unless repo forces otherwise.
- Exactly one clear module is responsible for the example domain.
- No cross-module mutations except through approved contracts.
- No new frameworks or build chains added.

---

## Prompt templates (for users)

### Template 1: Create a full app skeleton
“Create a new Xpell 2 Alpha app with @xpell/ui + @xpell/node and Wormholes.  
Build an Items Dashboard (list + edit) using XVMApp and data-only views (Nano-Commands v2).  
Keep it minimal, runnable, and contract-correct.”

### Template 2: Add 3D
“Extend the app to include a simple @xpell/3d scene view that reacts to XData 2 state changes.  
Do not break the existing UI and keep 3D optional behind a flag.”

### Template 3: Prepare for xpell.ai example
“Create a ‘full app example’ suitable for linking from xpell.ai: add a README, minimal run steps, and a clean folder structure.”

---

## Notes

- This skill is meant to produce **a runnable baseline**, not a complete product.
- Prefer correctness and contract clarity over features.
- If something is missing (e.g., wormholes server setup not present), implement the minimal scaffolding required and document assumptions explicitly.