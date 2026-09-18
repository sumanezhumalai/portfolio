# Portfolio

Static personal portfolio with plain HTML, CSS, and vanilla JS. No framework, no build step, no runtime dependencies.

Current sections: intro/hero, skills (`section.values` — static monochrome SVG grid, CSS-only hover, responsive), plus background/about, contact, and theme switching via CSS variables.

## Branches

- `main` — always stable and deployable. Only receives finished features via fast-forward merge.
- `feature/skill-section` — finished: static skills grid (merged into `main` at `16fda32`). Renamed from `feature/skills-diagram`.
- `feature/projects-section` — active: next isolated feature, branched off updated `main`.

## Workflow (isolated feature branches)

1. Branch fresh off `main`: `git checkout main && git checkout -b feature/<name>`
2. Work + commit only on that `feature/*` branch. Push with `git push -u origin feature/<name>`.
3. Finish: `git checkout main && git merge --ff-only feature/<name> && git push origin main`
4. Next feature: create a new `feature/*` from updated `main`. Don't rename an old branch, don't merge feature-to-feature.
5. Fixes to an old feature go via `main` first, then update the active branch: `git checkout feature/<active> && git merge main`

Normal `merge`/`push` is non-destructive. Avoid `reset --hard`, `push --force`, and `branch -D` unless you mean to discard work.
