# Claude Coached — Build Instructions

You are building the open-source release of the "Claude Coached" project — an AI cycling coach powered by Claude Code.

## What This Project Is

Claude Coached turns Claude Code into a personal cycling coach using a system prompt (`CLAUDE.md`), athlete data files, MCP integrations (Strava + Google Calendar), and skills. Users clone this repo, fill in their athlete profile, set up MCP servers, and get a fully personalized AI coach. Code (scripts, tools, etc.) should be added whenever it's the best solution for a feature — this is not a prompt-only project.

## Source Material

The original personal project lives at `/Users/thijssluijter/Desktop/Scratch/coachbot/`. Use it as the source for:
- `CLAUDE.md` — the coaching system prompt (needs PII removal)
- `data/*.md` — file structure to replicate as templates
- `.claude/skills/analyse/SKILL.md` — copy verbatim (already PII-free)

**Do NOT copy any personal data.** All PII must be stripped. See the PII audit checklist below.

## Implementation Plan

Follow these 8 steps in order. Each step is one commit.

### Step 1: Initialize repo + community files
- `git init`
- `.gitignore` — exclude `.claude/settings.local.json`, `.claude/projects/`, `.DS_Store`
- `LICENSE` — AGPLv3 (full text from https://www.gnu.org/licenses/agpl-3.0.txt)
- `CLA.md` — Contributor License Agreement: contributors grant the project owner (Thijs Sluijter) a perpetual, worldwide, non-exclusive, royalty-free license to use, modify, and sublicense their contributions under any license, including commercial licenses. Keep it short, human-readable, max 1 page.
- `CODE_OF_CONDUCT.md` — Contributor Covenant v2.1
- `CHANGELOG.md` — Start with `## v0.1.0` header, list initial features
- **Commit:** `Initial repo setup with license and community files`

### Step 2: Core coaching prompt
- Copy `coachbot/CLAUDE.md` and apply these PII removals:

| What | Current | Replace with |
|------|---------|-------------|
| Athlete name | "for Thijs Sluijter" | "for the athlete described in `data/athlete.md`" |
| Lifestyle note | "Thijs is not a pro athlete" | "The athlete is not a pro" |
| Calendar ID | Hardcoded `7e10fd3eb...` | "see `data/athlete.md` for your calendar ID" |
| Nutrition intro | "Thijs does NOT want..." | "Provide practical nutrition advice — no rigid calorie counting unless specifically requested" |
| Hardcoded calculations | `(~640-800g carbs)`, `(~130-160g for 80kg)` | Remove parenthetical examples, keep the g/kg formulas |

- Everything else (protocols, formulas, zone tables, coaching logic) stays exactly as-is
- **Important:** This CLAUDE.md replaces the current build-instructions CLAUDE.md. After this step, the repo's CLAUDE.md IS the coaching system prompt.
- **Commit:** `Add core coaching system prompt`

### Step 3: Data file templates
- Create `data/` files with same section structure as `coachbot/data/` but with `<!-- FILL IN: ... -->` placeholders
- `data/athlete.md` — rider profile, power profile table, racing history, goals, current metrics (FTP/W/kg/weight/maxHR/LT1), power zones (formulas not absolute watts), training availability, race calendar (A/B/C priority), equipment, Sport calendar ID field
- `data/coaching-notes.md` — section headers only: Strengths, Limiters, Observations, Plan Decisions
- `data/training-log.md` — empty with commented-out example week showing table format
- `data/training-plan.md` — empty season overview table + phase template + blocked days section
- **Commit:** `Add athlete data templates`

### Step 4: Example athlete profile
- Create `examples/athlete.example.md` — fully filled-in profile for a fictional athlete (e.g. "Alex Rivera", FTP 260W, 72kg, 3.6 W/kg) with realistic but fake data
- **Commit:** `Add example athlete profile`

### Step 5: Analysis skill
- Copy `coachbot/.claude/skills/analyse/SKILL.md` verbatim to `.claude/skills/analyse/SKILL.md`
- **Commit:** `Add workout analysis skill`

### Step 6: Documentation
- `docs/setup-guide.md` — Step-by-step: prerequisites, clone, Strava MCP setup (create API app, install server, authenticate), Google Calendar MCP setup (enable API, OAuth creds, install, find calendar ID), fill in athlete.md, verify, troubleshooting
- `docs/how-it-works.md` — Architecture: CLAUDE.md = coaching protocols, data/ = athlete state, MCP = API layer, skills = features, code welcome for scripts/tools
- `docs/roadmap.md` — Feature ideas: new skills (/weekly-review, /build-plan, /race-prep, /ftp-test, /compare), new integrations (intervals.icu, weather, TrainingPeaks, Wahoo/Garmin), coaching enhancements (running analysis, HR-only mode, mental training, heat/altitude, women's health periodization), QoL (CTL/ATL/TSB charts, season summaries, equipment tracking)
- **Commit:** `Add documentation`

### Step 7: README + contribution guides
- `README.md` — Title + one-liner, what is this, what you get (bullet list), quick start (5 steps), skills reference, requirements, contributing link, license
- `CONTRIBUTING.md` — How to contribute, types of contributions welcome (prompts, skills, code, docs, integrations), testing approach, style guide, CLA note explaining why it exists (preserves dual-licensing option)
- `.github/ISSUE_TEMPLATE/bug_report.md` — for bad coaching advice or wrong metric calculations
- `.github/ISSUE_TEMPLATE/feature_request.md` — for new skills/integrations/protocols
- `.github/PULL_REQUEST_TEMPLATE.md` — what changed, testing, PII check
- `.github/workflows/cla.yml` — CLA Assistant GitHub Action
- **Commit:** `Add README and contribution guides`

### Step 8: PII audit + tag
- Grep the entire repo for: "Thijs", "Sluijter", "380W", "80kg", Strava ID "94833695", calendar ID "7e10fd3eb", friend names (Mehmed, Kroon, Kroonie), race names (Utrecht Ultra, Green Divide), settings.local.json, .claude/projects/
- Fix any leaks found
- `git tag v0.1.0`
- **Commit (if fixes needed):** `Remove leaked PII`

## Target Repository Structure

```
claude-coached/
├── README.md
├── LICENSE                              (AGPLv3)
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── CHANGELOG.md
├── CLA.md
├── .gitignore
├── .github/
│   ├── workflows/
│   │   └── cla.yml
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
├── CLAUDE.md                            (coaching system prompt — replaces this file at Step 2)
├── .claude/
│   └── skills/
│       └── analyse/
│           └── SKILL.md
├── data/
│   ├── athlete.md
│   ├── coaching-notes.md
│   ├── training-log.md
│   └── training-plan.md
├── examples/
│   └── athlete.example.md
└── docs/
    ├── setup-guide.md
    ├── how-it-works.md
    └── roadmap.md
```

## PII Audit Checklist (Step 8)

None of these should appear anywhere in the final repo:
- [ ] "Thijs" or "Sluijter"
- [ ] "380W" or "80kg" (outside of formulas/zone table context)
- [ ] Strava ID "94833695"
- [ ] Calendar ID `7e10fd3eb20a8bd34e1fb66393dd403f75f16d1f66783d15f117e31df26ef2d2`
- [ ] Friend names: Mehmed, Kroon, Kroonie
- [ ] Specific race results: Utrecht Ultra, Green Divide
- [ ] `settings.local.json` file
- [ ] Any `.claude/projects/` memory files

## Key Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Template approach | Placeholders in `data/*.md` directly | CLAUDE.md references exact paths — no renaming needed |
| Setup script? | No — markdown guide only | Audience is Claude Code users (technical), MCP setup varies by OS |
| `settings.local.json` | Excluded via `.gitignore` | Auto-generated by Claude Code when users approve tool access |
| Example data | Separate `examples/` dir with fictional athlete | Shows complete profile without PII risk |
| Versioning | Semver starting at v0.1.0 | MAJOR = breaking template changes, MINOR = new skills/protocols, PATCH = prompt fixes |
| License | AGPLv3 + CLA | Community uses freely, derivatives must stay open-source. CLA preserves dual-licensing option for future commercialization |
