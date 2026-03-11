# looselyorganized/ci

Reusable CI workflows for Loosely Organized projects.

## Usage

Each repo calls the reusable workflow via a thin caller at `.github/workflows/ci.yml`.
This caller is managed by `/lo:new` (creates it) and `/lo:status` (updates it).

## CI Responsibility Split

Two environments run checks — they have different jobs:

- **GitHub CI** = clean-environment validation (does it work on a fresh checkout?)
- **Local ship pipeline** = intent validation (does it meet requirements, pass review, have no vulnerabilities?)

### Check Matrix by Stage

| Check | Explore | Build | Open |
|---|---|---|---|
| **Lint** | — | GH CI | GH CI |
| **Test** | — | Local (Gate 3) + GH CI | Local (Gate 3, hard stop) + GH CI |
| **Build** | — | GH CI (if has-build) | GH CI (if has-build) |
| **EARS review** | — | Local (Gate 2) | Local (Gate 2) |
| **Code review** | — | Local (Gate 4, reviewer subagent) | Local (Gate 4, reviewer subagent) |
| **Dependency audit** | — | — | Local (Gate 3, `npm audit`) + Weekly cron |
| **README staleness** | — | Local (Gate 5) | Local (Gate 5) |

Closed behaves like Explore (everything off).

### Why This Split

- **Lint only in GH CI** — local editors already lint on save; CI catches what slips through
- **Tests run in both** — local for fast feedback before push, CI for clean-environment proof
- **Build only in GH CI** — validates the production build works on a fresh checkout
- **EARS, code review, README check are local only** — they require context and judgement that CI can't provide
- **Audit is local + weekly cron** — dependency scanning is a supply chain concern, not a per-PR concern; weekly catches new CVEs without slowing the PR pipeline
