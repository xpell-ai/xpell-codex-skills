# xpell-codex-skills

Official Codex skills for generating Xpell 2 applications.

This repository contains structured Codex skills designed to generate
real-time UI, 3D, server modules, and full-stack runtime applications
using the Xpell 2 AI-native architecture.

---

## What This Repository Provides

These skills allow Codex to:

- Generate @xpell/ui applications (XUI + XVM + XVMApp)
- Generate @xpell/node server modules (XModule-based)
- Generate @xpell/3d spatial runtime components
- Generate full end-to-end Xpell 2 applications
- Follow Xpell runtime contracts (XData 2, Nano-Commands 2, XEM rules)
- Enforce modular architecture principles

The goal is not just code generation — but structured, contract-aware generation.

---

## Supported Packages

The skills target the Xpell 2 ecosystem:

- @xpell/core — runtime contracts and execution engine
- @xpell/ui — real-time browser UI runtime
- @xpell/3d — spatial runtime layer
- @xpell/node — server runtime (xnode, Wormholes, XDB)

---

## Repository Structure

Example structure:

skills/
  xpell-core/
  xpell-ui/
  xpell-3d/
  xpell-node/
  xpell-app/         (full app generator)
docs/
  examples/
  architecture-notes/

Each skill is self-contained and follows a strict system contract to
avoid hidden state assumptions or framework leakage.

---

## Installation (Codex)

1. Clone this repository.
2. Copy the relevant skill folder into your local Codex skills directory
   (usually ~/.codex/skills or your project-level .codex/skills).
3. Restart Codex.
4. The skills will become available for generation tasks.

Refer to your Codex documentation for exact skill installation paths.

---

## Example Prompt

Generate a full Xpell 2 application with:

- @xpell/ui dashboard (XVMApp)
- @xpell/node module with CRUD operations
- Wormholes integration
- Structured Nano-Commands (JSON handlers)

The skill will generate:

- App manifest
- Views as structured data
- XModule server stubs
- Basic routing and runtime wiring

---

## Philosophy

Xpell is built around AI-native architecture.

These skills enforce:

- Explicit runtime contracts
- Modular extension via XModule
- No implicit cross-module mutation
- Serializable UI and logic definitions
- Deterministic navigation (XVM rules)

The objective is to generate maintainable, runtime-driven systems — not one-off scripts.

---

## Status

Alpha — evolving alongside Xpell 2.

Contracts and generation patterns may evolve as the platform stabilizes.

---

## Related Links

Website: https://xpell.ai  
Xpell 2 Alpha: https://xpell.ai/xpell-2-alpha/  
Examples: https://xpell.ai/examples/  

---

Built by Aime Technologies
