# How It Works

Claude Coached turns Claude Code into a personal cycling coach through four layers working together.

## Architecture Overview

```
┌─────────────────────────────────────────────┐
│                 Claude Code                  │
│                                              │
│  ┌─────────────┐  ┌──────────────────────┐  │
│  │  CLAUDE.md   │  │  .claude/skills/     │  │
│  │  (coaching   │  │  (analysis, etc.)    │  │
│  │   protocols) │  │                      │  │
│  └──────┬───────┘  └──────────┬───────────┘  │
│         │                     │              │
│  ┌──────┴─────────────────────┴───────────┐  │
│  │              data/*.md                  │  │
│  │  (athlete profile, plan, log, notes)   │  │
│  └──────┬─────────────────────┬───────────┘  │
│         │                     │              │
│  ┌──────┴───────┐  ┌─────────┴────────────┐  │
│  │  Strava MCP  │  │  Google Calendar MCP │  │
│  │  (workouts)  │  │  (scheduling)        │  │
│  └──────────────┘  └─────────────────────┘  │
└─────────────────────────────────────────────┘
```

## Layer 1: Coaching Protocols (`CLAUDE.md`)

The system prompt defines how the coach behaves. It contains:

- **Startup protocol** — what to read at the start of every conversation
- **Subjective feedback protocol** — how to ask about RPE, sleep, stress, lifestyle
- **Workout analysis protocol** — step-by-step process for computing metrics (NP, IF, TSS, VI, EF, aerobic decoupling)
- **Training plan protocol** — periodization, weekly structure, calendar integration
- **Weekly review protocol** — comparing planned vs actual, tracking CTL/ATL/TSB
- **Nutrition guidance** — race fueling, training nutrition, general principles
- **Key metrics reference** — power zone table, training load formulas, HR zones

This is a plain markdown file. You can read it, modify it, and extend it to change how the coach operates.

## Layer 2: Athlete State (`data/*.md`)

Four markdown files store everything the coach knows about you:

| File | Purpose | Updated by |
|------|---------|------------|
| `athlete.md` | Profile, FTP, zones, goals, race calendar | You (manually) |
| `training-plan.md` | Current mesocycle, weekly plans, blocked days | Coach (with your approval) |
| `training-log.md` | Weekly session summaries, TSS, metrics | Coach (with your approval) |
| `coaching-notes.md` | Observations, limiters, plan decisions | Coach (with your approval) |

These files persist between conversations. Every time you start a new chat, the coach reads them to pick up where it left off.

## Layer 3: MCP Integrations

[MCP (Model Context Protocol)](https://modelcontextprotocol.io/) servers give the coach access to external APIs:

### Strava MCP
- `get-recent-activities` — list recent workouts
- `get-activity-details` — summary metrics for a specific activity
- `get-activity-laps` — interval/lap breakdown
- `get-activity-streams` — raw second-by-second power, HR, cadence data
- `get-athlete-zones` — your configured HR/power zones

### Google Calendar MCP
- `list-events` / `search-events` — check your schedule for conflicts
- `create-event` — add workouts to your Sport calendar with structured descriptions
- `update-event` / `delete-event` — adjust the plan

## Layer 4: Skills (`.claude/skills/`)

Skills are reusable, structured workflows that the coach can execute. They're invoked with slash commands.

### `/analyse` — Workout Analysis
The built-in analysis skill (`/analyse`) follows a 9-step process:
1. Load athlete context from data files
2. Find the activity (by ID, date, or description)
3. Fetch data from Strava (details, laps, streams)
4. Compute metrics using Python (NP, IF, TSS, VI, EF, decoupling)
5. Compare against the prescribed session
6. Ask for subjective feedback (RPE, legs, sleep)
7. Present coaching analysis
8. Offer to log in training files
9. Suggest adjustments to upcoming sessions

## Adding Code

This is not a prompt-only project. Scripts, tools, and utilities are welcome additions whenever they're the best solution. For example:

- Python scripts for computing metrics or generating charts
- Data processing tools for importing from other platforms
- Automation scripts for common workflows

If code solves the problem better than a prompt, write code.

## How Conversations Flow

A typical coaching conversation:

1. **Coach reads context** — athlete profile, current plan, recent training, coaching notes
2. **You make a request** — "analyse my ride", "plan next week", "how should I taper?"
3. **Coach fetches data** — pulls from Strava, checks calendar
4. **Coach computes and reasons** — applies protocols from CLAUDE.md
5. **Coach asks for input** — subjective feedback, scheduling preferences
6. **Coach delivers advice** — data-driven, specific, actionable
7. **Coach updates files** — with your permission, logs the session and updates the plan

Each conversation starts fresh but picks up full context from the data files — so the coach always knows your history, current plan, and ongoing observations.
