<!--
SPDX-FileCopyrightText: 2026 JasonLiaoUH

SPDX-License-Identifier: CC-BY-NC-SA-4.0
-->

# Codex instructions for pokemon-roguelite

This repository is a fork of `pagefaultgames/pokerogue`. The default `beta` branch is the clean upstream-sync baseline. Product work belongs on `dev/pokemon-roguelite` or task branches created from it.

## Project goal

Build a customized browser roguelite monster-battling game on top of PokéRogue while preserving the proven battle/data architecture wherever practical. Prefer incremental adaptation over broad rewrites.

The product roadmap and current sequencing live in `docs/pokemon-roguelite-roadmap.md`.

## Non-negotiable repository rules

- Do not modify, force-push, or rebase the clean `beta` baseline unless the user explicitly requests an upstream sync.
- Do not change the `assets` or `locales` submodule remotes without explicit user authorization.
- Do not remove or weaken upstream license, copyright, REUSE, or SPDX metadata.
- New Markdown documentation in this fork must include SPDX metadata compatible with repository policy.
- New code must preserve the repository's existing SPDX/REUSE conventions.
- Do not add third-party images, music, fonts, sprites, Pokémon assets, or other copyrighted media unless their source/license is documented and the user has explicitly approved their use.
- Existing upstream assets may remain during the fork-baseline phase; do not mass-copy, regenerate, or replace them as part of unrelated tasks.
- Do not rewrite the battle engine, save format, data model, phase system, or networking layer merely for stylistic reasons.
- Prefer compatibility shims, feature flags, theme/config layers, and focused extensions over invasive rewrites.
- Preserve deterministic battle behavior and save compatibility unless a task explicitly changes game rules or migration behavior.
- Never hide errors with `any`, `@ts-ignore`, disabled tests, or blanket lint suppressions.

## Environment

Use the repository-pinned toolchain:

- Node.js: `>=24.9.0`
- pnpm: `10.x`
- TypeScript
- Phaser
- Vite
- Vitest
- Biome

When the checkout is fresh or submodule content is missing:

```bash
pnpm install
pnpm update-submodules
```

For a devcontainer, use `pnpm start:podman`. For normal local development, use `pnpm start:dev`.

## Required validation

Before considering a code task complete, run the narrowest relevant tests during development, then run the full applicable gates for the final patch:

```bash
pnpm typecheck
pnpm biome:ci
pnpm test:silent
pnpm build
```

If a full test run is impractical because of an environment limitation, run all relevant focused tests and clearly report exactly what could not be run and why. Do not claim PASS for unexecuted checks.

UI-only changes still require `typecheck`, `biome:ci`, and `build`; manually verify the UI when an interactive browser is available.

## Development approach

For customization work, use this order unless the user changes priorities:

1. Establish a clean, reproducible upstream baseline.
2. Add product identity/configuration without altering battle semantics.
3. Add a fork-owned theme/UI override layer.
4. Set Traditional Chinese (`zh-TW`) as the preferred product locale without deleting other locales.
5. Customize title/menu/onboarding and starter/run presentation.
6. Customize Classic-mode progression and rewards through isolated rule/config changes.
7. Add new modes/features only after regression coverage exists.
8. Replace or add visual/audio assets only with explicit licensing decisions.

When changing gameplay rules, first locate the current upstream implementation and existing tests. Extend the existing architecture instead of duplicating parallel systems.

## Testing expectations

- Tests must be deterministic.
- Add regression tests for non-trivial battle, data, save, or rules changes.
- Keep UI tests focused on underlying logic where possible; use manual browser verification for visual behavior.
- Reuse existing test utilities, overrides, fixtures, and phase helpers.
- Do not update snapshots or expected values until the behavioral change is understood and intentional.

## Commit/PR hygiene

Follow the repository's Conventional Commit / PR-title conventions.

Keep changes focused. Do not mix upstream-sync changes, dependency upgrades, refactors, gameplay changes, and UI redesigns in one patch unless they are inseparable.

If work derived from AI assistance is ever proposed back to upstream PokéRogue, follow upstream's AI-assistance disclosure requirements. Do not generate upstream contributor communication that violates their contribution policy.

## First milestone

Before major customization, prove the fork baseline can install, typecheck, lint, test, and build on `dev/pokemon-roguelite`. Record any failures as baseline/upstream/environment failures before changing gameplay code.
