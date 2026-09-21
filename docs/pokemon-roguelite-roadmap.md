<!--
SPDX-FileCopyrightText: 2026 JasonLiaoUH

SPDX-License-Identifier: CC-BY-NC-SA-4.0
-->

# Pokémon Roguelite fork roadmap

## Baseline

Upstream: `pagefaultgames/pokerogue`  
Fork: `JasonLiaoUH/pokemon-roguelite`  
Clean sync branch: `beta`  
Product development branch: `dev/pokemon-roguelite`

Initial fork baseline commit:

`7a83d855dc6b6a668bff12ce7602c5ebcb557c97`

At fork creation, this matched the upstream `beta` HEAD.

## Product direction

The project intentionally starts from PokéRogue rather than reimplementing its mature battle engine. The main objective is to create a distinct customized web roguelite experience while retaining the upstream systems that already solve Pokémon battle rules, species/forms, moves, abilities, items, biomes, trainers, rewards, saves, localization, and test infrastructure.

The default strategy is:

- preserve battle correctness first;
- customize through configuration and extension points;
- keep upstream sync reasonably possible;
- separate product-specific code from upstream-derived code where practical;
- avoid large rewrites until tests prove equivalent behavior;
- keep licensing and asset provenance visible.

## Milestone 0 — Reproducible fork baseline

Goal: prove the fork is healthy before changing behavior.

Acceptance gates:

```bash
pnpm install
pnpm update-submodules
pnpm typecheck
pnpm biome:ci
pnpm test:silent
pnpm build
```

Record:

- Node and pnpm versions;
- submodule SHAs;
- test count and failures;
- build output;
- any failures already present on the untouched upstream baseline.

No gameplay changes belong in this milestone.

## Milestone 1 — Product identity layer

Create a small fork-owned configuration surface for:

- game/product display name;
- default locale preference;
- title-screen labels;
- build/version branding;
- feature flags for fork-only behavior.

Requirements:

- no broad search-and-replace renaming of internal PokéRogue types;
- no battle-rule changes;
- no save-schema break;
- upstream-compatible defaults should remain easy to identify.

## Milestone 2 — Theme and UI customization layer

Introduce a fork-owned theme/config layer before redesigning screens.

Priorities:

- title/menu;
- HUD treatment;
- battle command panels;
- starter-selection presentation;
- reward-selection presentation;
- responsive desktop/mobile layout.

Preserve existing input behavior unless intentionally changed.

Do not replace copyrighted visual/audio assets during this milestone. Placeholder or properly licensed fork-owned assets may be introduced later.

## Milestone 3 — Traditional Chinese product experience

Target locale: `zh-TW`.

Goals:

- prefer Traditional Chinese on first launch for this product;
- keep English and other upstream locales available;
- customize product-owned strings through fork-owned locale keys;
- avoid machine-rewriting upstream localization wholesale.

Any changes intended for upstream must follow upstream translation contribution rules.

## Milestone 4 — Starter and run onboarding

Customize the player-facing flow while retaining underlying battle correctness:

- starter-selection UX;
- starter cost presentation;
- party preview;
- run configuration;
- tutorial/onboarding;
- first-wave transition.

Add tests for any new rules, filters, limits, or saved preferences.

## Milestone 5 — Classic-mode customization

Only after baseline/UI work is stable:

- progression pacing;
- encounter configuration;
- biome presentation;
- trainer/boss cadence;
- reward pools;
- item rarity/weight rules;
- economy values.

Prefer data/config changes where the architecture already supports them.

Every gameplay-rule difference from upstream should be documented and tested.

## Milestone 6 — New fork-exclusive systems

Candidate areas, implemented one at a time:

- alternative run modes;
- fork-exclusive modifiers;
- new reward families;
- progression/meta systems;
- additional challenge settings;
- optional visual skins with documented licensing.

Each feature must have an explicit save-compatibility decision.

## Upstream sync policy

Keep `beta` as the clean upstream-sync branch.

Suggested flow:

1. Sync official `pagefaultgames/pokerogue:beta` into fork `beta`.
2. Run baseline validation on `beta`.
3. Merge/rebase the product development branch only after reviewing upstream changes.
4. Resolve conflicts in small groups.
5. Run the full validation suite again.

Do not mix upstream sync with feature development in the same commit.

## Asset/licensing policy

This fork inherits a mixed licensing environment.

- Source code is generally AGPL-3.0-only unless a file states otherwise.
- Documentation is generally CC-BY-NC-SA-4.0 unless otherwise noted.
- Upstream assets have file-specific licensing/provenance rules.
- Assets without explicit licensing information must not be assumed reusable outside the conditions under which they were obtained.

Before a public release, perform a dedicated asset/licensing audit and decide which assets must remain, be attributed, be replaced, or be removed.

## Definition of done for every milestone

A milestone is not complete until:

- intended behavior is implemented;
- relevant automated tests pass;
- `pnpm typecheck` passes;
- `pnpm biome:ci` passes;
- `pnpm build` passes;
- user-visible behavior is manually verified when applicable;
- new licensing/SPDX obligations are satisfied;
- remaining known limitations are documented.
