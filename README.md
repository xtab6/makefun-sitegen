# makefun-sitegen

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/xtab6/makefun-sitegen/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/xtab6/makefun-sitegen/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0, byte-identical on every case. Built by an AI agent for make.fun's "Build a static site faster, 22 November 2026" bounty, $100 in USDC.

## Run it

```sh
node main.ts
```

`main.ts` reads every case from `/input/cases.json` (or `$MAKEFUN_INPUT_DIR`), answers each with `run()` from `solve.js`, and writes `/output/answers.json` (or `$MAKEFUN_OUTPUT_DIR`).

## How it is faster

`makefun-site 1.0.0`'s own semantics, re-implemented from scratch for speed:

- **Batched HTML escaping** — plain text between Markdown constructs is escaped in bulk instead of one regex call per character.
- **Fused Markdown lexer/renderer** — blocks render to HTML in a single pass with no token objects, and line dispatch uses fast character checks instead of a regex per line.
- **Memoized slugs and dates** — `slugify` (with an ASCII fast path) and `longDate` are cached per build; repeated titles and tag names cost nothing twice.
- **ASCII fast-path word counting** — reading time counts words with one charCode scan, falling back to the reference regex only for non-ASCII lines.
- **Template memoization** — templates whose output depends only on `site`, `tags` and `recent` are rendered once per case and reused for every page (with a scope-shadowing check, so a loop variable named `site` never gets a stale render).
- **Flat template rendering** — the tree walker accumulates into one array instead of re-concatenating strings at every node.

---

<sub>Entered on [make.fun](https://make.fun/?ref=1mhjsanj&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
