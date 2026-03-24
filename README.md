# Claude Coached

An AI cycling coach powered by [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Clone, fill in your profile, connect Strava and Google Calendar, and get a personal coach that analyses workouts, builds training plans, and adapts to your life.

## What You Get

- **Workout analysis** with NP, IF, TSS, aerobic decoupling, interval breakdown, and coaching feedback
- **Periodized training plans** built around your race calendar, goals, and availability
- **Weekly reviews** comparing planned vs actual training load (CTL/ATL/TSB tracking)
- **Calendar integration** — workouts scheduled on your Google Calendar with structured descriptions
- **Subjective feedback** — the coach asks about RPE, sleep, stress, and lifestyle to adapt the plan
- **Nutrition guidance** — race fueling, training nutrition, and recovery advice
- **Real-life awareness** — holidays, parties, and travel are factored in, not ignored

## Quick Start

1. **Clone** the repo
   ```bash
   git clone https://github.com/thijssluijter/claude-coached.git
   cd claude-coached
   ```

2. **Set up MCP servers** for [Strava](https://github.com/nicobako/strava-mcp) and [Google Calendar](https://github.com/nspady/google-calendar-mcp) — see [Setup Guide](docs/setup-guide.md) for details, or just ask Claude to walk you through it

3. **Fill in your profile** — edit `data/athlete.md` with your FTP, weight, goals, and race calendar (see [`examples/athlete.example.md`](examples/athlete.example.md) for reference)

4. **Start coaching**
   ```bash
   claude
   ```

5. **Try it out** — "Analyse my last ride", "Build me a training plan", or "Weekly review"

## Skills

| Skill | Description |
|-------|-------------|
| `/analyse` | Analyse a Strava workout — metrics, prescription comparison, subjective feedback, coaching advice |

More skills on the [roadmap](docs/roadmap.md).

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Strava](https://www.strava.com/) account (power meter recommended but not required)
- [Google Calendar](https://calendar.google.com/) account

## Documentation

- [Setup Guide](docs/setup-guide.md) — Step-by-step installation and configuration
- [How It Works](docs/how-it-works.md) — Architecture and design
- [Roadmap](docs/roadmap.md) — Feature ideas and future directions

## Contributing

Contributions are welcome — prompts, skills, code, docs, and integrations. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

[AGPLv3](LICENSE) — free to use and modify, derivatives must stay open-source. A [CLA](CLA.md) is required for contributions to preserve future licensing flexibility.
