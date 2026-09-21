<!--
SPDX-FileCopyrightText: 2026 JasonLiaoUH

SPDX-License-Identifier: CC-BY-NC-SA-4.0
-->

# Codex Task M0 — Verify reproducible fork baseline

## Goal

Establish a reproducible, untouched fork baseline for `JasonLiaoUH/pokemon-roguelite` before any gameplay/UI customization.

Work from branch:

`codex/m0-baseline`

Read and obey the repository root `AGENTS.md` and `docs/pokemon-roguelite-roadmap.md` before doing anything else.

## Hard constraints

- Do **not** modify `beta`.
- Do **not** change gameplay, battle rules, save schemas, UI, dependencies, or submodule remotes.
- Do **not** update dependencies merely because newer versions exist.
- Do **not** disable or skip failing checks to manufacture a pass.
- Keep the original upstream licensing/REUSE/SPDX metadata intact.
- If the baseline itself is broken, document the failure rather than fixing unrelated upstream code in this task.

## Baseline identity

Fork: `JasonLiaoUH/pokemon-roguelite`  
Upstream baseline: `pagefaultgames/pokerogue:beta`  
Initial matching commit: `7a83d855dc6b6a668bff12ce7602c5ebcb557c97`

At bootstrap time, fork `beta` and upstream `beta` matched at that commit.

## Tasks

1. Inspect the current checkout and confirm the active branch and commit ancestry.
2. Record:
   - `node --version`
   - `pnpm --version`
   - `git status --short`
   - `git submodule status`
3. Install dependencies using the repository's intended toolchain.
4. Ensure submodules are initialized using the repository-supported command if necessary.
5. Run:
   - `pnpm typecheck`
   - `pnpm biome:ci`
   - `pnpm test:silent`
   - `pnpm build`
6. Do not make gameplay/source changes to turn failures into passes.
7. Create `docs/baseline-validation.md` containing:
   - date of validation;
   - base commit;
   - Node/pnpm versions;
   - assets/locales submodule SHAs;
   - exact commands executed;
   - PASS/FAIL for each gate;
   - test summary/count if available;
   - build result;
   - any environment/upstream failures with the relevant error excerpt summarized;
   - a final baseline status of `READY_FOR_CUSTOMIZATION` only if all required gates pass, otherwise `BLOCKED`.
8. Ensure the new Markdown file has repository-compatible SPDX metadata.
9. If you changed only that report, run the appropriate documentation/repository checks again as required by `AGENTS.md`.
10. Commit only the baseline report (and only genuinely necessary deterministic setup metadata, if any).

## Acceptance criteria

- No gameplay/source behavior changed.
- `beta` remains untouched.
- Baseline results are reproducible and written to `docs/baseline-validation.md`.
- Every required check has an explicit PASS/FAIL backed by an executed command.
- No unexecuted check is reported as PASS.
- Worktree is clean after the final commit.

## Completion report

Return:

- commit SHA;
- validation status;
- Node/pnpm versions;
- submodule SHAs;
- typecheck result;
- Biome result;
- Vitest result and test count;
- build result;
- exact blockers, if any.
