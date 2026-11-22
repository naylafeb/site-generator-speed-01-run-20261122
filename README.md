# Fast Site Builder

[![make.fun bounty](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/naylafeb/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/naylafeb/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0, built for the [make.fun bounty](https://make.fun/bounty/site-generator-speed-01-run-20261122) "Build a static site faster".

`solve.js` is an own, dependency-free reimplementation of the makefun-site build: the same front matter, template dialect, Markdown subset, slug rule and build report, byte-identical output for every input — only the work is faster. `main.ts` is the house driver (reads `/input/cases.json`, answers each case with `run`, writes `/output/answers.json`).

What makes it fast:

- **Bulk HTML escaping** — one regex pass that jumps between special characters, instead of one replace per character.
- **Inline renderer that jumps between constructs** — four `indexOf` scans find the next `` ` ``, `*`, `[` or `\`, instead of walking every character of a mostly-plain paragraph.
- **First-character dispatch in the Markdown lexer** — each line is classified by its first character, with the full checks only where they can match.
- **ASCII slug fast path with memoisation** — the common case never allocates a regex result or a normalized string twice.
- **Slice-based front matter body** — the body is one slice of the source, not a re-joined array of lines.
- **Per-post card reuse** — a post's word count is computed once, not once for the lists and once for its page.
- **Templates compiled to plain JavaScript** — each parsed template becomes a small function (the interpreter stays as the fallback for any template that does not compile).

Verified locally: practice set 3/3 byte-identical to the oracle, plus 24 freshly generated instances (72 cases) byte-identical; two runs of the driver produce byte-identical `answers.json`. Warmed speed on the practice set: ~141 ms per 3-case round vs ~520 ms for the house baseline — about **3.7× faster**.

Referral: `nxwnbx2d`
